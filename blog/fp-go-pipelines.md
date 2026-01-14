---
timestamp: 2025-08-30T15:15:08
publish: false
tags:
  - functional-programming
  - golang
  - iterators
  - gleam
title: Functional Style Golang Pipelines
description: Applying functional programming patterns to Golang, efficiently. This article focusses on building Pipelines with and without Iterators.
date: 2025-08-30T15:15:55
canonical_url: https://zanca.dev/blog/fp-go
---

I started to write more Golang, both at home and at work. Since writing Golang last I've picked up Gleam, which is a functional programming language that I've fallen in love with. It's changed how I think about problems a lot. For example now I feel like I have a much better understanding about using recursion in programs as a "solution" to immutable data structures. But I've also found myself wanting to use pipelines more often.

I was writing this little program to take my obsidian notes and generate html files (just like this blog) and I realized that I'd need to:

- Filter for files ending in `.md`
- Map the files to a struct containing path and lines.
- Filter out empty files
- And finally process `md` to `html`

> _...a bunch more things are needed to properly render links, but that's a story for another day_.

I found myself looking thru the go standard library and found there were no slice utils for `Map` and `Filter`. So I made my own!

> It was at this point, I realized Go's generics didn't allow for a generic receiver and a generic signature, so I couldn't do `listInstance.Map(func (item MyType) {})`, which would have been nice as I could have easily implemented a method chaining pattern, but alas Go doesn't let you.

> Apparently Rob Pike tried to do this too, before generics: https://github.com/robpike/filter (and there's a really funny [Pull Request](https://github.com/robpike/filter/pull/1#issuecomment-93866116) in there). And there's HN comment that sums it up quite well.

```go
package lists
// Filter returns a new slice containing all elements of the input slice
// for which the predicate function returns true.
func Filter[T any](slice []T, predicate func(T) bool) []T {
  var result []T
  for _, v := range slice {
    if predicate(v) {
      result = append(result, v)
    }
  }
  return result
}

// Map returns a new slice containing the results of applying the
// transform function to each element of the input slice.
func Map[T any, R any](slice []T, transform func(T) R) []R {
  result := make([]R, len(slice))
  for i, v := range slice {
    result[i] = transform(v)
  }
  return result
}
```

> This pattern of `func Map(list, transform){...}` is also how you would do it in [Gleam](https://github.com/gleam-lang/stdlib/blob/main/src/gleam/list.gleam#L397-L406). _If you don't understand the Gleam implementation, it's worth remembering that Lists in Gleam are Single Link Lists, so the acc is built "backwards" and needs to then be reversed. And you'll also need to remember that there's no loop primitive, it's tail call optimized recursion all the way down_.

This worked splendidly but I wanted to see if this already exists, I couldn't have been alone in wanting this pattern.

Yep, here it is: https://github.com/kauppie/sliceutils

This goated package includes many more variations on this pattern and also includes some very interesting approaches that are unique to Go. For example this [FilterInPlace](https://github.com/kauppie/sliceutils/blob/main/sliceutils.go#L136-L150) method:

```go
func FilterInPlace[T any](slicep *[]T, filterFn func(T) bool) {
	// Pointer could be nil.
	if slicep == nil {
		return
	}
	n := 0
	for _, val := range *slicep {
		if filterFn(val) {
			(*slicep)[n] = val
			n++
		}
	}
	// Possibly shorten the slice to current length.
	*slicep = (*slicep)[:n]
}
```

It will only make sense if you remember that `range *slicep` will actually "copy" the slice's header, so even though we might be mutating the `slicep` as we loop over it, we won't encounter any unexpected behavior.

I really like how this library also uses `*InPlace` to signal that the method will mutate the passed in slice.

Here's the result:

```go
files, err := fs.GetFileList("testdata/obsidian-help/Sandbox")

if err != nil {
	fmt.Println("Error getting file list:", err)
	return
}

mdFiles := sliceutils.Filter(files, func(f string) bool {
	return strings.HasSuffix(f, ".md")
})

mdFileContents := sliceutils.Map(mdFiles, func(f string) *LavaFile {
	return &LavaFile{
		Path:    f,
		Content: fs.ReadFile(f),
	}
})

nonEmptyMdFiles := sliceutils.Filter(mdFileContents, func(content *LavaFile) bool {
	return content.Content != ""
})

for _, file := range nonEmptyMdFiles {
	fs.WriteFile("./build/"+fs.ChangeExtension(file.Path, ".html"), markdown.Markdown2Html(file.Content))
}
```

> commit of this code: [396f64e](https://github.com/metruzanca/lava/commit/396f64e0e51d2679b38056fa74e4da98bcb9a2b5)

This pipeline is a little noisy, comparing again to a language like Gleam, which has a [[Hindley-Milner type system]], which provides static types while omitting types that can be inferred.

> If Go had such a type system, we could omit some unnecessary types e.g. the `Filter` predicate could have been `func (f) { strings.HasSuffix(f, ".md") }`. We know what f is, since it **has to be** an element of `files` and we know what the return is, since it again has to satisfy the predicate signature. But we could take it a step further and the function passed to `Map` would also be much shorter: `func (f) { { Path: f, Content: fs.ReadFile(f) } }` and we would know that this is a `&LavaFile`, because there's no other possible value that it could be. And if your compiler can't figure out what it should be, then you'd be forced to provide a type. HM type systems are awesome.

Back to Golang though... This pipeline is great, its easy to add and remove steps and if you order things correctly, you don't have to handle errors.  (_because we filtered out non-md files `Markdown2Html` will, theoretically, panic with an error_)

However, as this collection grows, doing things this way might slow things down. We're effectively performing 4 loops here. There's got to be a better way to do this that doesn't incur this cost. If this was Rust, we'd be looking for what's known as a "Zero-Cost Abstraction". The solution of this problem is of course iterators, which were added in [Go-1.23](https://tip.golang.org/doc/go1.23#iterators). But before I show a version of this with iterators, I'd like to take a look at how things could have been done previously.
