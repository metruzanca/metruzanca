---
draft: true
title: "SolidJS + Firebase Guide"
date: 2000-01-01T00:00:00.000Z
description: How to use Firebase with SolidJS
tags: ["solidjs", "firebase", "typescript"]
# TODO make a thumbnail
---

[DRAFT]

[Firebase.io's SvelteFire](https://www.youtube.com/watch?v=UbOaAtHWidc&pp=ygUKc3ZlbHRlZmlyZQ==)

After seeing [Jeff](https://twitter.com/fireship_dev)'s [sveltefire](https://github.com/codediodeio/sveltefire), I just knew I had to rewrite my messy solidjs utility functions. Taking great inspiration from [`collectionStore`](https://github.com/codediodeio/sveltefire/blob/master/src/lib/stores/firestore.ts#L83C17-L128) I reimplemented them but with a solidjs spin on things.

But first, lets talk about why this is even necessary.

## FireStore SDK

### Missing Document ID

For the all the good and cool things firebase does, their JS SDK just does some things kinda weird.

The first one you're bound to encounter is how it doesn't include the `id` of the document in the data. You kinda need that `id` if you plan on editing/deleting the document. A pattern you see pretty common is to map the documents and add the id:
```ts
const data = snapshot.docs.map(doc => {
  return { id: doc.id, ...doc.data() } as T;
});
```

If you always do this, then you can write your types to include the id and write your code as if id was always present. Even though it technically isn't present at time of first creation. This is alright most of the time since we're doing the above anytime we get a new snapshot. So long as we're subscribed to a collection we'll always have the `id`. If you're not subscribed, you'll need to refetch the document after creation, thought this step can be skipped if you're creating the `id`s before inserting.

e.g. I had an app where I needed to avoid having duplicate images uploaded to stored, so I was hashing the images and using the hash as file name and document id. And since I had the hash, when I created the document i already added the `id` to the document.
```ts
const hash = await fileHash(file)
await setDoc(doc(db, 'media', hash), { id: hash, /* other data */ })
```

### Converting Snapshots to Signals

---

## Storage SDK
<!-- TODO downloadUrl stuff -->
