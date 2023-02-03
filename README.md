# css-for-js-devs

## Media Queries

The web is an incredibly broad platform: the same HTML and CSS might be tasked with running on a 5" phone screen and a 72" TV!

In order to accommodate screens of different shapes and sizes, CSS features **media queries**, which allow us to apply different CSS in different scenarios.

Media queries use the `@media` syntax. You can kinda think of it as an `if` statement in JavaScript.

```js
if (condition) {
  // Some JS that will run if the condition is met
}
```

```css
@media (condition) {
  /* Some CSS that'll run if the condition is met */
}
```

An example:

```html
<div class="small-only">
  Hello there!
</div>
```

```css
@media (max-width: 300px) {
  .small-only {
    color: red;
  }
}
```

In this case, the condition is `max-width: 300px`. If the window is between 0px and 300px wide, the CSS within will be applied.

### Hiding content

The conditional ability of media queries allows us to also hide content...

It's common to use media queries to have alternative interfaces depending on the screen size

```html
<div class="large-screens">
  I only show up on large screens.
</div>
<div class="small-screens">
  Meanwhile, you'll only see me on small ones.
</div>
```

```css
.large-screens {
  display: none;
}

@media (min-width: 300px) {
  .large-screens {
    display: block;
  }
  .small-screens {
    display: none;
  }
}
```

`display: none` is a declaration that removes an element from the rendering process; it's as if it doesn't exist.

By default, we'll hide any elements with the `large-screens` class.

If the window is at least `300px` wide, however, we apply special overrides. This includes showing `large-screens` elements, and hiding `small-screens` elements.

This trick is **often** used for navigation. Desktop users see a list of links, whereas mobile users see a hamburger icon.

### Valid Conditions

Inside the parentheses, typically `max-width` to add styles on small screens, or `min-width` to add styles on larger ones. 

> `max-width` and `min-width` are not CSS properties, they are media features.
> `max-width` is also a css property, but in the context of media queries, remember its a media feature.

## Selectors

CSS comes with an incredibly rich set of selectors, and those selectors can be mixed and match in interesting ways.

The most straightforward selectors target a specific tag or class:

```css
/* Turn all links red! */
a {
  color: red;
}

/*
  Remove the underline from all elements that
  have been given a class of `navigation-link`
*/
.navigation-link {
  text-decoration: none;
}
```

## Psuedo-classes

Psuedo-classes let us apply a chunk of CSS based on an element's current state.

```html
<style>
  button:hover {
    color: blue;
  }
</style>

<button>Hover over me!</button>
```

This code sample, with display a button with the text "Hover over me!".
Once the user hovers over the button, the text inside the button will turn blue.

This is similar to `onMouseEnter` / `onMouseLeave` events in JavaScript, but with built=in state management.

If done in JS, we'd need to register event listeners, but state management is needed to know if the element is *currently* being hovered.

And to avoid this, CSS makes life easier with Pseudo Classes.

Another example of a pseudo class is: `focus`

HTML comes with interactive elements like buttons, links and form inputs.

When we interact with one of these elements (etiher by clicking on it or tabbing to it), it becomes focused.

It'll capture keyboard input, so we can type into a form field or press 'Enter' to follow a link

The `:focus` pseudo-class allows us to apply styles exclusively when an interactive element has focus:

```html
<button>Hello</button>
<button>world</button>
<button>!</button>
```

```css
button:focus {
  border: 2px solid royalblue;
  background: royalblue;
  color: white;
}
```

When the elements appear, and you click on one of them or tab through the 3 buttons, you'll see the styling specified above.

There's also the `checked` pseudo-class

```html
<h1>Pizza Toppings</h1>
<br />
<label>
  <input type="checkbox" />
  Avocado
</label>
<br />
<label>
  <input type="checkbox" />
  Broccoli
</label>
<br />
<label>
  <input type="checkbox" />
  Carrots
</label>
```

```css
input:checked {
  width: 24px;
  height: 24px;
}
```

Browsers don't offer too much flexibility when it comes to checkboxes and radio buttons, but this neat trick lets you apply certain CSS properties depending on its status.

And finally there's the first/last child pseudo-class

Pseudo-classes aren't just for states like hover/focus/checked
They can also help apply conditional logic

For example, if there's a set of paragraphs within a `<section>`:

```html
<section>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
  <p>
    What do you know, it's a third
    paragraph!
  </p>
</section>
```

```css
body {
  background-color: silver;
}
section {
  padding: 24px;
  background-color: white;
}
p {
  margin-bottom: 1em;
}
```

What this does is add space under the last/final paragraph
And that's caused by the styling we added on the p-selector

This rule is meant to add space between each paragraph, but it also **applies to the final paragraph** and if inspected in the browers dev tools, there will be an orange rectangle that represents margin... but that's not the designated effect.

So here's how to fix it:

```html
<style>
  p {
    margin-bottom: 1em;
  }
  p:last-child {
    margin-bottom: 0px;
  }
</style>

<section>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
  <p>
    What do you know, it's a third
    paragraph!
  </p>
</section>
```

```css
body {
  background-color: silver;
}
section {
  padding: 24px;
  background-color: white;
}
```

The change should be obvious, the `:last-child` pseudo-class will only select `<p>` tags which are the **final** element within its container.
It needs to be the last child within its parent.

Similarly, the `:first-child` pseudo-class will match the first child within a parent container.

```html
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Carrot</li>
  <li>Durian</li>
</ul>
```

```css
li:first-child {
  color: red;
}
```

This will make the first item on the list the color red

Then there's also `:first-of-type` and `:last-of-type`
Almost identical to `:first-child` and `:last-child`, but they have a critical difference.

`:first-of-type` and `:last-of-type` depends on the type of the HTML tag

For example:

```html
<style>
  p:first-child {
    color: red;
  }
</style>

<section>
  <h1>Hello world!</h1>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
</section>
```

The first child within the parent <section> tag is an <h1>.
Our `p:first-child` is looking for situations where a *paragraph* is the first child within a parent container.
It doesn't work in this case.

But, if we switch the selector to `p:first-of-type`, it *does* work:

```html
<style>
  p:first-of-type {
    color: red;
  }
</style>

<section>
  <h1>Hello world!</h1>
  <p>This is a paragraph!</p>
  <p>This is another paragraph!</p>
</section>
```

The :first-of-type pseudo-class ignores any siblings that aren't of the same type.
In this case, p:first-of-type is going to select the first paragraph within a container, regardless of whether or not it's the first child.
