---
title: CSS Variables are Awesome!
theme: black
revealOptions:
    transition: 'slide'
    history: true

---

<!-- .slide: class="cssis😮" id="css-variables-are-awesome" -->

# <!-- .element: class="awesome" -->CSS<br>Variables<br><span>Are Awesome</span>

---

## What are CSS Variables?

* <!-- .element: class="fragment" --> Officially called [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*)
* <!-- .element: class="fragment" --> Runtime variables that are aware of DOM scope
* <!-- .element: class="fragment" --> Defined with a <code>&#45;&#45;</code> prefix
  * <!-- .element: class="fragment" --> **TIL**: They're actually browser prefix properties (like `-webkit-transform`) without a browser prefix.
* <!-- .element: class="fragment" --> Natively supported in [modern browsers](https://caniuse.com/#feat=css-variables)


----

## Where Can I Use Them?

* <!-- .element: class="fragment" --> ✅ Anywhere you can normally set a property with `--`.
* <!-- .element: class="fragment" --> ✅ Anywhere you can normally apply a value with `var(--)`.
* <!-- .element: class="fragment" --> `@media (min-width: var(--mobile)) {}` = 😢

----

## When Should I Use Them?

* <!-- .element: class="fragment" --> 🚫 Avoid using them for static, unchanging values, such as theme colors.  LESS can handle these.
* <!-- .element: class="fragment" --> ✅ Use them for dynamic CSS like responsive design or interaction.


----

## Basic Usage

```css
/* definition */
.selector { --my-border-size: 1px; }
```

```css
/* application */
.selector { border-size: var(--my-border-size); }
```

```css
/* define a local default value (defaults to `initial`) */
.selector { border-size: var(--my-border-size, 0); }
```

```css
/* set a global default value */
:root { --my-border-size: 0; }
```

```css
/* set a non-custom value first for backward compatibility */
.selector {
  border-size: 1px;
  border-size: var(--my-border-size, 0);
}
```

---

## Breadcrumbs 🍞

<div data-content-src="example-html-trial"></div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>
<style scoped data-content-src="link-colors"></style>
<style scoped data-content-src="hover"></style>
<style scoped data-content-src="glow"></style>

----

### Design Requirements

<div data-content-src="example-html-trial"></div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>
<style scoped data-content-src="link-colors"></style>
<style scoped data-content-src="hover"></style>
<style scoped data-content-src="glow"></style>

<br />
* Separated by analogous purple slashes
* Can contain 1-N levels
* Hovering container expands slashes (staggered)
* Hovering also adds slight glow

---

### Let’s Do It Live 🎤

----

#### Always design HTML first 🏗

```html
<ul class="crumbs">
  <li class="crumb">
    <a class="link" href="#">Home</a>
  </li>
  <li class="crumb"><a class="link" href="#">Artists</a></li>
  <li class="crumb"><a class="link" href="#">Cher</a></li>
</ul>
```

---

### Then Apply CSS 🎨

----

#### Layout

<div id="example-html">
  <div class="example">
    <ul class="crumbs">
      <li class="crumb"><a class="link" href="#">Home</a></li>
      <li class="crumb"><a class="link" href="#">Artists</a></li>
      <li class="crumb"><a class="link" href="#">Cher</a></li>
    </ul>
  </div>
</div>

<style id="base">
.example { overflow: hidden; border: 2px solid white !important; }
.crumbs, .crumbs > * {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

.crumbs {
  font-family: sans-serif;
}
</style>

<style id="layout-styles" scoped contenteditable>
/* basic layout */
.crumbs {
  display: inline-block;
  display: flex;
  background: black;
  margin: 0;
  padding: 0;
  list-style-type: none;
}

a.link {
  display: block;
  padding: 1em;
  color: white;
  cursor: pointer;
}
</style>

----

#### Add a Skewed Separator

<div data-content-src="example-html"></div>

<style scoped data-content-src="layout-styles"></style>

<style id="skew" scoped contenteditable>
.crumbs {
  overflow: hidden;
}
.crumb {
  transform: skew(-20deg);
  border-right: 10px solid white;
}

.crumb:first-child {
  margin-left: -.6em;
}

.crumb:first-child .link {
  padding-left: 1.6em;
}

.link {
  transform: skew(20deg);
}

</style>

----

#### Add analogous colors

<div data-content-src="example-html"></div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>

<style id="link-colors-before" scoped contenteditable>
/* lots of repetition and having to know how many children */
.crumb:nth-child(1) {
  background-image: linear-gradient(to top, hsl(278, 27%, 31%), hsl(278, 27%, 31%));
}
.crumb:nth-child(2) {
  background-image: linear-gradient(to top, hsl(278, 27%, 51%), hsl(278, 27%, 51%));
}
.crumb:nth-child(3) {
  background-image: linear-gradient(to top, hsl(278, 27%, 71%), hsl(278, 27%, 71%));
}
</style>

----

#### 🤔 Repetitive, Slightly Dynamic CSS?

Maybe we could make that a little better with some CSS Custom Properties…

----

#### Add some props on the HTML

```html
<ul class="crumbs" style="--count: 3">
  <li class="crumb" style="--index: 0;">
    <a class="link" href="#">Home</a>
  </li>
  <li class="crumb" style="--index: 1;">
    <a class="link" href="#">Artists</a>
  </li>
  <li class="crumb" style="--index: 2;">
    <a class="link" href="#">Cher</a>
  </li>
</ul>
```

Any React component could easily include these in the `render()` method.

----

#### Let’s use the props instead

<div id="example-html-props">
  <div class="example">
    <ul class="crumbs" style="--count: 3">
      <li class="crumb" style="--index: 0;"><a class="link" href="#">Home</a></li>
      <li class="crumb" style="--index: 1;"><a class="link" href="#">Artists</a></li>
      <li class="crumb" style="--index: 2;"><a class="link" href="#">Cher</a></li>
    </ul>
  </div>
</div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>

<style scoped contenteditable data-content-src="link-colors-before"></style>

Notes:
```css
.crumb {
  --variance: calc(40% / var(--count) * var(--index));
  --bg-color: hsl(278, 27%, calc(31% + var(--variance)));
  background-image: linear-gradient(to top, var(--bg-color), var(--bg-color));
}
```

----

#### Any count will now work

```html
<ul class="crumbs" style="--count: 4">
  <li class="crumb" style="--index: 0;"><a class="link" href="#">Home</a></li>
  <li class="crumb" style="--index: 1;"><a class="link" href="#">Artists</a></li>
  <li class="crumb" style="--index: 2;"><a class="link" href="#">Cher</a></li>
  <li class="crumb" style="--index: 3;"><a class="link" href="#">Albums</a></li>
</ul>
```

<div id="example-html-4">
  <div class="example">
    <ul class="crumbs" style="--count: 4">
      <li class="crumb" style="--index: 0;"><a class="link" href="#">Home</a></li>
      <li class="crumb" style="--index: 1;"><a class="link" href="#">Artists</a></li>
      <li class="crumb" style="--index: 2;"><a class="link" href="#">Cher</a></li>
      <li class="crumb" style="--index: 3;"><a class="link" href="#">Albums</a></li>
    </ul>
  </div>
</div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>

<style scoped id="link-colors">
.crumb {
  --variance: calc(40% / var(--count) * var(--index));
  --bg-color: hsl(278, 27%, calc(31% + var(--variance)));
  background-image: linear-gradient(to top, var(--bg-color), var(--bg-color));
}
</style>

----

#### Add hover transitions

<div data-content-src="example-html-4"></div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>
<style scoped data-content-src="link-colors"></style>

<style scoped contenteditable id="hover-before">
/* let’s add hover states */
.crumb {

}
</style>

Notes:

```css
.crumb {
  border-right: none;
  background-size: 10px 100%;
  background-repeat: no-repeat;
  background-position: right;
  transition: .2s background-size ease-out calc(.1s * var(--index));
  will-change: background-size;
}

a.link {
  padding-right: calc(1em + 10px);
}

.crumbs:hover .crumb {
  background-size: 100% 100%;
}
```

---

### Glow State 💡

<style scoped id="hover">
.crumb {
  border-right: none;
  background-size: 10px 100%;
  background-repeat: no-repeat;
  background-position: right;
  transition: .2s background-size ease-out calc(.1s * var(--index));
  will-change: background-size;
}

a.link {
  padding-right: calc(1em + 10px);
}

.crumbs:hover .crumb {
  background-size: 100% 100%;
}
</style>

----

#### Add a JS hook for Trial


```html
  <ul class="crumbs" style="--count: 4">
    <li class="crumb" data-track-pointer style="--index: 0;"> … </li>
    …
  </ul>
```

----

#### Add `--pointer-x, --...y` props on `mousemove`

Used via [Trial.js on Github](https://github.com/MarkoCen/trial-js).

```js
Trial('[data-track-pointer]').within(
  { distance: 1000, cord: 'center' },
  (distance, ele, e) => {
    const node = Trial.getEleCord(ele, 'topLeft');
    const pointer = Trial.getPointerPos(e);
    ele.style.setProperty('--pointer-x', pointer.left - node.left)
    ele.style.setProperty('--pointer-y', pointer.top - node.top)
  },
);
```

----

#### Add the Glow

<div id="example-html-trial">
  <div class="example">
    <ul class="crumbs" style="--count: 4">
      <li class="crumb" data-track-pointer style="--index: 0;"><a class="link" href="#">Home</a></li>
      <li class="crumb" data-track-pointer style="--index: 1;"><a class="link" href="#">Artists</a></li>
      <li class="crumb" data-track-pointer style="--index: 2;"><a class="link" href="#">Cher</a></li>
      <li class="crumb" data-track-pointer style="--index: 3;"><a class="link" href="#">Albums</a></li>
    </ul>
  </div>
</div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>
<style scoped data-content-src="link-colors"></style>
<style scoped data-content-src="hover"></style>
<style scoped contenteditable id="glow-before">
.crumb {

}

.link {

}
</style>

Notes:
```css
/* add a layer between crumb and link */
.crumb {
  position: relative;
}

.crumb:before {
  position: absolute;
  content: '';
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.link {
  position: relative;
}

/* position the glow */
.crumb:before {
  /* just for readability.  Should be LESS vars */
  --shape: ellipse 100% 100% at var(--mouse-position);
  --center-color: rgba(255, 255, 255, 0.25);
  --edge-color: rgba(255, 255, 255, 0);

  /* JS exposes --pointer-x and --pointer-y */
  --mouse-position: calc(var(--pointer-x) * 1px) calc(var(--pointer-y) * 1px);

  background: radial-gradient(var(--shape), var(--center-color), var(--edge-color)) no-repeat;
}
```
---

## Final Product

<div data-content-src="example-html-trial"></div>

<style scoped data-content-src="layout-styles"></style>
<style scoped data-content-src="skew"></style>
<style scoped data-content-src="link-colors"></style>
<style scoped data-content-src="hover"></style>
<style scoped id="glow">
/* add a layer between crumb and link */
.crumb {
  position: relative;
}

.crumb:before {
  position: absolute;
  content: '';
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.link {
  position: relative;
}

/* position the glow */
.crumb:before {
  /* just for readability.  Should be LESS vars */
  --shape: ellipse 100% 100% at var(--mouse-position);
  --center-color: rgba(255, 255, 255, 0.25);
  --edge-color: rgba(255, 255, 255, 0);

  /* JS exposes --pointer-x and --pointer-y */
  --mouse-position: calc(var(--pointer-x) * 1px) calc(var(--pointer-y) * 1px);

  background: radial-gradient(var(--shape), var(--center-color), var(--edge-color)) no-repeat;
}
</style>

---

## Takeaways 💭

* <!-- .element: class="fragment" --> 🌳 CSS Custom Properties are perfect for sharing dynamic information down the DOM
* <!-- .element: class="fragment" --> 🏗 React-friendly
* <!-- .element: class="fragment" --> ➗ `calc()`-friendly
* <!-- .element: class="fragment" --> 🚸 Modern supported with easy fallbacks
* <!-- .element: class="fragment" --> 🔬 Solves some previously unsolvable use-cases, like `:nth-child(?)`
* <!-- .element: class="fragment" --> 🗜️ Makes a lot of repetitive code much more compact

---

# Thanks! 🙇 🙏 🖖

### Buy the Mug! ☕

http://thejase.com/go/css-mug

