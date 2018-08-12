---
title: CSS Variables are Awesome!
theme: black
revealOptions:

    transition: 'slide'
    history: true
    progress: false
    width: '100%'
    height: '100%'
    margin: 0.1
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
<!-- .slide: class="embedded" -->

## Breadcrumbs 🍞

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----
<!-- .slide: class="embedded" -->

### Design Requirements

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

* Separated by analogous purple slashes
* Can contain 1-N levels
* Hovering container expands slashes (staggered)
* Hovering also adds slight glow

---

### Let’s Do It Live 🎤

----
<!-- .slide: class="embedded" -->

#### Always design HTML first 🏗

<iframe scrolling="no" data-src="//jsbin.com/pagigop/40/embed?html,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

---

### Then Apply Presentation 🎨

----
<!-- .slide: class="embedded" -->

#### Layout

<iframe scrolling="no" data-src="//jsbin.com/pagigop/46/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----
<!-- .slide: class="embedded" -->

#### Skewed and Separated

<iframe scrolling="no" data-src="//jsbin.com/pagigop/48/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----
<!-- .slide: class="embedded" -->

#### Add analogous colors

<!-- <div data-content-src="example-html"></div> -->

<iframe scrolling="no" data-src="//jsbin.com/pagigop/87/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----

#### 🤔 Repetitive, Slightly Dynamic CSS?

Maybe we could make that a little better with some CSS Custom Properties…

----
<!-- .slide: class="embedded" -->

#### Add some props on the HTML

<iframe scrolling="no" data-src="//jsbin.com/pagigop/75/embed?html" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>


Any React component could easily include these in the `render()` method.

----
<!-- .slide: class="embedded" -->

#### Let’s use the props instead

<iframe scrolling="no" data-src="//jsbin.com/pagigop/85/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

Notes:
```css
.link {
  // max - min / total * current
  --alpha: ~"calc(1 - .4 / var(--count) * var(--index))";
  background-color: ~"hsla(@{hsl}, var(--alpha)";
}
```

----
<!-- .slide: class="embedded" -->

#### Any count will now work

<iframe scrolling="no" data-src="//jsbin.com/pagigop/90/embed?html,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----
<!-- .slide: class="embedded" -->

#### Add hover transitions

<iframe scrolling="no" data-src="//jsbin.com/pagigop/108/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

Notes:

```css
.crumbs {
    --hover: 0;
    --stagger: .1s;

    &:hover {
      --hover: 1;
    }
  }
  .link {
    // kill old backgrounds
    background-color: transparent;

    // fill the background using a transitioned linear gradient
    background: linear-gradient(@hsla, @hsla)
                left top / ~"calc(100% * var(--hover))" 100%
                no-repeat;

    // add a staggered delay to each item
    @delay: ~"calc(var(--index) * var(--stagger))";
    transition: background .2s ease-out @delay;
  }
}
```

---
<!-- .slide: class="embedded" -->

### Glow State 💡

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

----
<!-- .slide: class="embedded" -->

#### Add `--pointer-x, --...y` props on `mousemove`

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?js" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>


Used via [Trial.js on Github](https://github.com/MarkoCen/trial-js).

----
<!-- .slide: class="embedded" -->

#### Add the Glow

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?css,output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

---
<!-- .slide: class="embedded" -->

## Final Product

<iframe scrolling="no" data-src="//jsbin.com/pagigop/110/embed?output" frameborder="no" allowtransparency="true" allowfullscreen="true">
</iframe>

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

