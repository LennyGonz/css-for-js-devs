# CSS for JS Devs

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

The first child within the parent `<section>` tag is an `<h1>`.
Our `p:first-child` is looking for **situations** where a *paragraph* is the first child **within** a parent container.
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

The `:first-of-type` pseudo-class ignores any siblings that aren't of the same type.
In this case, `p:first-of-type` is going to select the first paragraph within a container, regardless of whether or not it's the first child.

## Debugging in the browser

Debugging in the browser is what we already know, but a couple nice facts are:

* crossed out properties mean they're invalid or not being applied
* comments can be added in your css, using the `/* */` syntax
* Firefox has a better developer tool


## Rendering Logic 1

### Built-in Declarations and Inheritance

Fundamentally, the goal of CSS is to allow you to control the appearance and layout of your app's content.

You don't quite start with a blank canvas; HTML tags do include a few minimal styles. For example, here are the built-in styles for `<a>` tags, **in Chrome 86**:

```css
a {
  color: -webkit-link;
  cursor: pointer;
  text-decoration: underline;
}
```

These styles are part of the user-agent stylesheet.

Each browser includes their own stylesheet full of base styles like this.
There are some hard rules in the HTML specification, but for the most part, each browser comes up with its own default styles.
That's why focus rings look so different across browsers!

In the past, it was common to use a **CSS reset**, a copy-pastable CSS chunk that strips away these default styles, setting a blank canvas that looks the same across all browsers, to strip away many of these default user-agent styles.

These days, though, browsers are a bit more consistent in how they render elements, and there's a growing awareness that we shouldn't expect our sites/applications to be identical across browsers and devices.

Josh Comeau's CSS reset

```css
/*
  1. Use a more-intuitive box-sizing model.
*/
*, *::before, *::after {
  box-sizing: border-box;
}

/*
  2. Remove default margin
*/
* {
  margin: 0;
}

/*
  3. Allow percentage-based heights in the application
*/
html, body {
  height: 100%;
}

/*
  Typographic tweaks!
  4. Add accessible line-height
  5. Improve text rendering
*/
body {
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

/*
  6. Improve media defaults
*/
img, picture, video, canvas, svg {
  display: block;
  max-width: 100%;
}

/*
  7. Remove built-in form typography styles
*/
input, button, textarea, select {
  font: inherit;
}

/*
  8. Avoid text overflows
*/
p, h1, h2, h3, h4, h5, h6 {
  overflow-wrap: break-word;
}

/*
  9. Create a root stacking context
*/
#root, #__next {
  isolation: isolate;
}
```

### The Cascade

```html
<style>
  p {
    font-weight: bold;
    color: hsl(0deg 0% 10%);
  }

  .introduction {
    color: violet;
  }
</style>

<p class="introduction">
  Hello World
</p>
```

We've created two rules, one targeting a tag (`p`), another targeting a class (`introduction`). Then, we've created an HTML element that matches them both.

You may already know what happens here: we wind up with a bold, violet paragraph.
It plucks the `font-weight` declaration from the `p` tag, and the `color` declaration from the `.introduction` class.

The example shpwws the browser's ***cascade algorithm*** at work.

When the browser needs to display our introduction paragraph on the screen, it first needs to figure out which declarations apply to it

And before it can do that, it needs to collect a set of matching rules.

Once it has a list of applicable rules, it works out any conflicts.

I imagine this as a sort of deathmatch: if multiple selectors each apply the same property, it pits them against each other.

Two fighters enter, but only one emerges.

That's the main idea. The browser will take a set of applicable style rules, and whittle it down to a list of specific declarations that are applicable.

How does it determine which rules win each battle ? It depends on the *specifity* of the selector

The CSS language includes many different selectors, and each selector has a relative power.

For example, classes are "more specific" than tags, so if there is a conflict between a class and a tag, the class wins. IDs, however, are more specific than classes.

The CSS from above could be written in JS as:

```js
const tagStyles = {
  fontWeight: 'bold',
  color: 'hsl(0deg 0% 10%)',
};
const classStyles = {
  color: 'violet',
}

const appliedStyles = {
  ...tagStyles,
  ...classStyles,
}
```

The order that they're merged in is determined by specificity; class styles are more specific than tag styles, so they're merged in later. 
This way, they overwrite any conflicting styles. 
All non-conflicting styles are kept.

### Block and Inline Directions

CSS builds its sense of direction based on this system.

It has a block direction (vertical), and an inline direction (horizontal).

Here's an easy way to remember the directions, for horizontal languages:

* Block direction is like lego blocks: they stack together one on top of the other.

* Inline direction is like people standing in-line; they stand side by side, not one on top of the other.

#### Logical Properties

Earlier, we learned about "built-in" styles — these are the rules that each browser comes with out-of-the-box, defined in the user-agent stylesheet.

Here are the built-in styles for p tags, in Chrome:

```css
p {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
}
```

You may not have seen `margin-block-start` before, but can you figure out what it means from context? 
Can you figure out why Chrome chose to use these properties, instead of a standard margin property?

These properties are equivalent to the 4 cardinal directions: `margin-top`, `margin-bottom`, `margin-left`, and `margin-right`.

`margin-block` refers to the **vertical axis**.
`margin-block-start` refers to the **top**, since block elements are stacked top-to-bottom. 

Similarly, `margin-inline-start` refers to the left edge, since "inline" is the horizontal direction, and words are placed left-to-right.

Why use these alternatives? Because not all languages are left-to-right, top-to-bottom. If you were to switch your browser's language to Arabic, `margin-inline-start` would add spacing to the right instead of to the left, since Arabic is a right-to-left language.

These alternatives are known as **logical properties**. It's not just margin: there are logical variants for padding, border, and overflow as well.

There are still a few kinks around some of the shorthand variants (eg. margin-block instead of margin-block-start), but the properties we've seen are supported in all major browsers.

### The Box Model

The four aspects that mae up the box model are:

1. Content
2. Padding
3. Border
4. Margin

A helpful analogy is to imagine a person out for a winter walk, wearing a big poofy coat:

* The *content* is the person themselves, the human being inside the coat.
* The *padding* is the polyester stuffing in the coat. The more stuffing there is, the more poofed-up the coat will be, and the more space the person will take up.
* The *border* is the material of the coat. It has a thickness and a color, and it affects the person's appearance.

#### Box Sizing

When we say that an element should have a `wdith` of `100%`, what does that actually mean ?

It turns out, the browser might have a slightly different interpretation than you do. Let's explore.

These aspects affect the size of the element. The code snippet below will draw a black rectangle on the screen (due to the border). What are the dimensions of that rectangle?

```html
<style>
  section {
    width: 500px;
  }
  .box {
    width: 100%;
    padding: 20px;
    border: 4px solid;
  }
</style>

<section>
  <div class="box"></div>
</section>
```

Here's what it looks like in the Chrome devtools:

![box-sizing](images/box-sizing-example.png)

When we set out `.box` to have `width: 100%`, we're saying that the box's **content size** should be equal to the available space, `500px`. The padding and border is *added on top*.

The box winds up being `548px` wide because it adds `20px` of padding and 4px of border to each side: `500 + 20 * 2 + 4 * 2`.

The same thing happens with height: because the element is empty, it has a content size of `0px`, with the same border and padding added on top.

This behavior is surprising, and generally not what we want as developers! Thankfully, browsers provide an escape hatch.

The `box-sizing` CSS property allows us to change the rules for size calculations. The default value (`content-box`) only takes the inner content into account, but it offers an alternative value: `border-box`.

With `box-sizing: border-box`, things behave **much** more intuitively.

Instead of having to remember to swap box-sizing on every layout element, we can set it as the deault value of *all* elements with this handy CSS snippet:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

This is the very first rule in the **custom CSS reset**, and arguably the most important

#### Padding

A helpful way to think about padding is that it's "inner-space".

Imagine that you add a background color to an element with some padding:

```css
.some-fella {
  padding: 48px;
  background-color: tomato;
}
```

Because padding is on the inside, the padded area also receive the background color.

![padding](images/padding.png)

Padding can be set for all directions at once, or it can be specified for individual directions.

```css
.even-padding {
  padding: 20px;
}

.asymmetric-padding {
  padding-top: 20px;
  padding-bottom: 40px;
  padding-left: 60px;
  padding-right: 80px;
}

/* The same thing, but using ✨ logical properties ✨ */
.asymmetric-logical-padding {
  padding-block-start: 20px;
  padding-block-end: 40px;
  padding-inline-start: 60px;
  padding-inline-end: 80px;
}
```

pixels are the best units to use for padding, but are bad when it comes to *font-size*.

We can use percentages for padding:

```css
.box {
  padding-top: 25%;
}
```

We can, but the result is surpsiing and counter-intuitive; **percentages are ALWAYS calculated based on the elements available width.**
This is true for left/right padding, and even for top/bottom padding!

#### Border

Border is a bit of an odd duck in the trinity of padding/border/margin - unlike the other two, it has a **visual/cosmetic** component.

There are 3 styles specific to border:

* Border with (eg. `3px`, `1em`)
* Border style (eg. `solid`, `dotted`)
* Border color (eg. `hotpink`, `black`)

They can be combined into a shorthand:

```css
.box {
  border: 3px solid hotpink
}
```

The **only** required field is `border-style`. Without it, no border will be shown!

```css
.not-good {
  /* 🙅‍♀️ Won't work – needs a style! */
  border: 2px pink;
}

.good {
  /* 🙆‍♀️ Will produce a black, 3px-thick border */
  border: solid;
}
```

As with padding, you can overwrite broad properties with specific ones. 
This is useful for creating several "variants" of an element.

**Here's a fun fact:** If we don't specify a border color, it'll default to using the element's text color!

```html
<style>
  .box {
    /* No border color specified: */
    border: 4px solid;
  }
  .box.one {
    /* Specify the *text* color: */
    color: hotpink;
  }
  
  .box.two {
    color: slateblue;
  }
</style>

<div class="box one">One</div>
<div class="box two">Two</div>
<div class="box three">Three</div>
```

If you want to specify this behaviour explicitly, it can be done with the special currentColor keyword.

`currentColor` is always a reference to the element's derived text color (whether set explicitly or inherited), and it can be used anywhere a color might be used:

```css
.box {
  color: hotpink;
  border: 1px solid currentColor;
  box-shadow: 2px 2px 2px currentColor;
}
```

##### Border Radius

It's not hard to understand the rationale; the `border-radius` property **rounds** an element even if it has no border!

Like padding, `border-radius` accepts discrete values for each direction.
→ Unlike padding, it's focused on specific corners, not specific sides. Here are some examples:

![square-box](images/square-box_border-radius.png)

![empty-glass](images/empty-glass_border-radius.png)

![individual-properties](images/custom-values_border-radius.png)

> You can also use percentages; 50% will turn your shape into a circle or oval, since each corner's radius is 50% of the total width/height:

![circle](images/cirlce_border-radius.png)

##### Border vs Outline

A common stumbling block for devs is the distinction between outline and border.

In some respects, they're quite similar! They both add a visual edge to a given element.

The **core difference** is that outline doesn't affect layout.

Outline is kinda more like `box-shadow`; it's a cosmetic effect draped over an element, without nudging it around, or changing its size.

Outlines share many of the same properties:

↪ border-width becomes outline-width
↪ border-color becomes outline-color
↪ border-style becomes outline-style

Outlines are stacked outside border, and can sometimes be used as a "second border", for effect:

![border-outline](images/border-outline.png)

More tidbits about outlines:

↪ Outlines will follow the curve set with `border-radius` in all modern browsers. This is a relatively recent change, which landed in browsers between 2021 and 2023 

↪ **Outlines have a special `outline-offset` property.** It allows you to add a bit of a gap between the element and its outline

#### Margin

The third in the trinity: ***margin***.

Margin increase the space **around** an element, giving it some breathing room. As we saw earlier, margin is "personal space".

In some ways, margin is the most amorphous and mysterious. It can do wacky things, like pull an element outside a parent, or center itself within its container.

##### Syntax

The syntax for margin looks an awful lot like padding:

```css
.spaced-box {
  margin: 20px;
}

.asymmetrically-spaced-box {
  margin: 20px 10px;
}

.individually-specified-box {
  margin-top: 10px;
  margin-left: 20px;
  margin-right: 30px;
  margin-bottom: 40px;
}
.logical-box {
  margin-block-start: 20px;
  margin-block-end: 40px;
  margin-inline-start: 60px;
  margin-inline-end: 80px;
}
```

##### Negative margin

With padding and border, **only** positive numbers (including 0) are supported. With margin, however, we can drop into the negatives.

A negative margin can **pull** an element outside its parent or it can also pull an element's sibling closer:

![negative-margin](images/negative-margin.png)

Interesting, right? The top `.pink.box` has a negative bottom margin, and the result is that its sibling is **pulled up**, and overlaps the pink box.

→ It's easy to fall into the trap of thinking that margin is exclusively about changing the selected element's position.
→ Really, though, it's about changing the gap between elements.
→ Negative margin shrinks the gap below an element, causing the next element to scoot up closer.

Negative margin can affect the position of **all siblings**. This is what happens if negative margin is applied to only the first box

![negative-margins-siblings](images/negative-margin_siblings.png)

The interesting thing is those two black boxes: **they "follow" the deep pink box up**.

↪ When we use margin to tweak an element's position, **we might also be tweaking every subsequent element as well**.

↪ This is different from other methods of shifting an element's position, like using `transform: translate`.

##### Auto Margins

Margins have one other trick: they can be used to center a child in a container.

This is what happens when an element's left and right margin are set to `auto`.

![auto-margin](images/auto-margin.png)

If we inspect this element in the devtools, the browser shows that it has applied an equal amount of margin on either side of the element:

![auto-margin-devTools](images/auto-margin_2.png)

→ The `auto` value seeks to fill the ***maximum available space***. It works the same way for the `width` property.

→ When we set both `margin-left` and `margin-right` to `auto`, we're telling them each to take up as much space as possible.

→ Like a game of Hungry Hungry Hippos, both sides try to gobble up all of the free space around the element.

  ↪ They're evenly-matched, though, so neither side wins; they always end in a draw.

→ If you take the free space around an element and distribute it evenly on both sides, you wind up centering that element. This is a happy byproduct of this mechanism.

Two caveats:

→  This only works for **horizontal margin**.
&nbsp; &nbsp; ↪ Setting top/bottom margin to auto is equivalent to setting it to 0px

→ This only works on elements with an explicit width.
&nbsp; &nbsp; ↪ Block elements will naturally grow to fill the available horizontal space, so we need to give our element a width in order to center it.

###### Exercise

In Flow layout, elements are stacked neatly into boxes. Things don't overlap by default.

Sometimes, though, we want to pull something a little outside its parent container, for aesthetic effect.

The goal is to reproduce this image:

![exercise](images/margin-exercise.png)

```css
  body {
    background: #222;
    padding: 32px;
  }

  .card {
    background-color: white;
    padding: 32px;
    border-radius: 8px;
  }

  h1 {
    background: deeppink;
    padding: 16px 32px;
    margin-bottom: 24px;
    font-size: 2rem;
    text-align: center;
    border-radius: 4px;
  }
```

Given the provided CSS, we know that we're going to have to play with negative-margin.

We give it a random value to just get a baseline -> `margin-top: -15px;` And while we notice that it does decrease the amount of white space above the pink box, it also pulls up the sibling elements.

But that's fine, it's just a matter of figuring out how many pixels the pink box requires.

We notice that the values for `padding: 16px 32px;` and `margin-bottom: 24px;` are multiples of 8 and even the `padding` of the card: `padding: 32px;` is a multiple of 8.

This suggests that we should stay within a multiple of 8, which eventually will lead us to `margin-top: -48px;`

![solution](images/margin-exercise-solution.png)

###### Exercise Part 2: Stretched Content

Our `.card` element has served us well so far, but if we want to include a picture of an otter, and we want it to stretch out, to fill the available container width:

![otter](images/stretched-content_margin.png)

The container's padding is getting in our way.

![problem-with-width](images/exercise-part2-width_margin.png)

→ We've stretched the image but it seems to have only stretched to the left side and it's because of the `width: 100%` on the image.

→ Images are weird, they're replaced elements.

→ So the image itself isn't part of the DOM... It's kind of a foreign object that we're embedding in the DOM.

→ If we remove the width property all-together, the image takes up its natural size. 

An alternative is to use `calc`, but there is another way. **We can use a wrapping element**.

We wrap the `img` tag in a `div`, and that way the iamge fills that container. 

![solution-margin](images/exercise-part2-solution_margin.png)

More explanation for the use of a **wrapping element**.

→ The default value for the `width` property is `auto`.
→ By default, for most elements, this means “automatically grow to fill as much space as possible”.

→ Images, as well as other “replaced elements” like `<video>` and `<canvas>`, are special.
→ They don't automatically expand to fill the available space.
&nbsp; &nbsp; ↪ Instead, they rely on their intrinsic size.

→ For example, suppose I take a photo on an old webcam. That photo has a native size of `640 × 480`.
&nbsp; &nbsp; ↪ When I embed this image on the page, using an `<img>` tag, it'll have a default width of `640px`.

**This is the problem.** `width: auto` has a different meaning for replaced elements. It doesn't mean "stretch out and fill all of the space", it means "use your natural width"!

## Flow Layout

When it comes to layout, CSS is more like a collection of mini-languages than a single cohesive language.

Every HTML element will have its layout calculated by a layout algorithm. These are known as “*layout modes*”, and there are 7 distinct ones.

We'll cover several layout modes in this course, including:
 
1. Positioned layout
2. “Flexible Box” layout (AKA Flexbox), and 
3. Grid layout (AKA CSS Grid)
 
For now, though, let's focus on Flow layout.

Flow layout is the default layout mode. Everything we've seen so far has used Flow layout.
A plain HTML document, with no CSS applied, uses Flow layout exclusively.

I like to think of Flow layout as the “Microsoft Word” layout algorithm. It's intended to be used for document type layouts.

There are two main element types in Flow layout:

→ **Block elements:** things like headings, paragraphs, footers, asides. The chunks of content that make up a page

→ **Inline elements:** things like links, or a string of bold text. Generally inline elements are meant to highlight bits of text, or elements within a block container.

Each HTML tag has a default type. `<div>` and `<header>` are block elements, `span` and `<a>` are inline elements.

We can toggle any particular element with the `display` property:

```css
/* Transform a particular <a> tag from `inline` to `block`: */
a.nav-link {
  display: block;
}
```

There's also a third value, `inline-block`, which is a sort of fusion between the two types.

In flow layout, block elements stack in the block direction, and inline elements stack in the inline direction.

It's more than just direction, though. Each `display` value comes with its own behaviour, its own rules.

#### Inline elements don't want to make a fuss

If you've ever tried to adjust the positioning or size of an inline element, you've likely been confounded by the fact that a bunch of CSS properties just don't work.

For example, this snippet will have no effect:

```css
strong {
  height: 2em;
}
```

You can picture inline elements as go-with-the-flow-type folks.
They don't want to inconvenience anyone by pushing any boundaries.

You *can* shift things in the inline direction with `margin-left` and `margin-right`, since that pushes it around in the inline direction, but you can't give it a `width` or `height`.

When it comes to layout, an inline element is where it is, and there's not much we can do about it.

> Replaced elements, `img`, `video`, `canvas` are technically inline, but they're special: they can affect block layout.


#### Block elements don't share

When you place a block level element on the page, its content box greedily expands to fill the entire available horizontal space.

A heading might only need `150xp` to contain its letters, but if you put it in an `800px` container, it will expand to fill `800px` of width.

If we want to shrink the container to the minimum size required for the letters, we can do this with the special `width` keyword `fit-content`.

```html
<style>
  h2 {
    width: fit-content;
    border: 2px dotted;
  }

  .box.red {
    width: 50px;
    height: 25px;
    background: red;
  }
</style>

<h2>
  Hello World
</h2>
<div class="box red"></div>
```

There will be plenty of space left on that first row, regardless the `h2` will sit underneath the heading. 

`h2` does not want to share any inline space.

In other words, elements that are `display: block` will stack in the block direction, regardless of their size.

#### Inline elements have "magic space"

In the box model lesson, we saw 3 ways to increase sapce around an element.

![magic-space](images/magic-space.png)

The image is 300px tall, but the parent div is 306px tall. The extra pixels are called "magic space" because the browser treats inline elements as if they're typography. 

It makes sense that with text, you'd want a bit of extra space, so that the lines in a paragraph aren't crammed in too tightly.


There are 2 ways to fix this:

1. Set images to `display: block` - if you're noticing this problem, there's a good chance your images aren't interspersed with text, so setting them to display as blocks makes sense.
2. Set the `line-height` on the wrapping div to `0`.

![line-height](images/magic-space_line-height.png)

This space is proportional to the height of each line, so if we reduce the line height to 0, this "magic space" goes away.
Because our container doesn't contain any text, this property has no other effect.

##### Space between inline elements

HTML is *space-sensitive*, at least to an extent. The browser can't tell the difference between whitespace added to separate words in a paragraph, and whitespace added to indent our HTML and keep it readable.

This problem is specific to Flow-layout. Other layout modes, like Flexbox, ignore whitespace altogether. So, the simplest thing is to switch the container to use Flexbox.

Inline elements can also line-wrap. 

#### The deal with inline-block

Most confusing things about Flow layout is the Frankenstein `display: inline-block` value.

An `inline-block` element is a block-level element that can be placed in an inline context. We can think of `inline-block` as "a block in inline's clothing".

In terms of layout, it's treated as an inline element. We can drop it in the middle of a paragraph, without totally borking the layout. But *internally*, it acts much more like a block element.

![inline-block](images/inline-block.png)

The first image demonstrates when `strong` doesn't have the property `display: inline-block`.

While the second demonstrates what it looks like with the property applied.

This element now has access to the *full universe* of CSS. Usually, properties like `width` and `margin-top` have no effect on an **inline element**, but they *do* work on **inline-block** elements.

We've effectively turned out `strong` element into a block element, as far as its **own** CSS declarations are concerned.

But from the paragraph's perspective, it's an inline element. It lays it out as an inline element, in the inline direction beside the text.

In many ways, `inline-block` allows us to have our cake and eat it too.

However, `inline-block` doesn't line-wrap.

![line-wrap](images/line-wrap_inline-block.png)

### Width Algorithms

### Height Algorithms
