---
title: Darken and Lighen in Pure CSS
date: 2020-12-29T05:48:09.485Z
description: How to make SASS's Darken and Lighten mixins in pure CSS and Styled-Components
tags: ["css", "react"]
publish: true
---

If you're coming from SASS, you're likely used to all the helpful mixins it comes with e.g. lighten and darken.

This feature is missing from many frameworks like my favorite, Styled-Components.

> The Styled-Components specific solution to this is [Polished](https://github.com/styled-components/polished).

But if you want a solution that works in all frameworks by just using Pure CSS, this is what this short post is about.

## Method 1

This first method has the caveat that it will change the entire element's brightness.

Heres 10% lighter and 10% darker.

### CSS

```css
.lighten {
  filter: brightness(110%);
}

.darken {
  filter: brightness(90%);
}
```

### Styled-Components

```ts
const lighten = (value:number) => css`
  filter: brightness(${value + 1});
`

const darken = (value:number) => css`
  filter: brightness(${1 - value});
`
```

```ts

const StyledButton = styled.button`
  background-color: ${theme.bg.primary};
  &:hover{
    ${darken(.2)}
  }
`
```

## Method 2

This next method will only apply the darkening/lightning on the background color.

Once again Heres 10% lighter and 10% darker.

### CSS

```css
.lighten {
  background-image: linear-gradient(
    0deg,
    rgba(255,255,255,0.1) 0%,
    rgba(255,255,255,0.1) 100%);
}

.darken {
  background-image: linear-gradient(
    0deg,
    rgba(0,0,0,0.1) 0%,
    rgba(0,0,0,0.1) 100%);
}
```

### Styled-Components

```ts
const lighten = (value:number) => css`
  background-image: linear-gradient(
    0deg,
    rgba(255,255,255,${value}) 0%,
    rgba(255,255,255,${value}) 100%);
`

const darken = (value:number) => css`
  background-image: linear-gradient(
    0deg,
    rgba(0,0,0,${value}) 0%,
    rgba(0,0,0,${value}) 100%);
`
```

The usage is the same as the previous method for styled-components.
