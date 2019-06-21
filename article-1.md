# CSS tricks

#### Author: Jesse Dijkman

Did you know that you don't need JavaScript when you have CSS? Ofcourse this isn't serious but there are some use cases where this is true. You probably thought CSS was just for styling, but it can also be used for some functionalities. Two examples where CSS can be used instead of JavaScript are:

- Dark/Light mode switch
- Dropdown menus

These are just two examples, I could imagine a lot more use cases where CSS hacks can make JavaScript obsolete.

Ok, now let's get into the how of it all. First of all, most hacks (that I'm going to show atleast) are achieved use two very cool selectors:

- `+` **Adjacent sibling combinator**
- `~` **General sibling combinator**

### Adjacent sibling combinator

Let's say you have the following HTML:

```html
<h1>Title</h1>
<p>First p after h1</p>
<p>Second p after h1</p>

<section>
  <h1>Another title</h1>
  <p>Again the first p</p>
</section>
```

What I want to do now is give all the p elements that follow an h1 a top-border. Do I gave all those p elements a class? No, this is where you can use the adjacent sibling combinator. The adjacent sibling combinator, selects an element that is immediately after another. This will look like this:

```css
h1 + p {
  border-top: solid 2px black;
}
```

This will affect the following two p elements:

```html
<p>First p after h1</p>

<p>Again the first p</p>
```

### General sibling combinator

Let's say you have the following HTML:

```html
<h1>Title</h1>
<div>
  <p>First p in first div after h1</p>
</div>
<p>First p after h1</p>
<p>Second p after h1</p>
```

What I want to do is select all the p elements the are placed after an h1. But not the ones nested in elements after the h1. This is what the general sibling combinator does. It will select all non-nested siblings.

```css
h1 ~ p {
  color: grey;
}
```

This will affect the following two p elements:

```html
<p>First p after h1</p>
<p>Second p after h1</p>
```

### Pseudo-classes

If you've ever written more than just a few lines of CSS. You probably know what a pseudo-class is. If it's doesn't ring a bell, than `:hover` might. This is a pseudo-class. Pseudo-classes are used to define the state of an element. Element states might include:

- `:hover` _The user hovers over an element_
- `:target` _The user clicks on an element_
- `:checked` _Used for checkboxes and radio buttons_

You can find a full list of pseudo-classes on [w3schools](https://www.w3schools.com/css/css_pseudo_classes.asp).

### Checkbox trick

Let's look at the `:checked` pseudo-class. What if we combine this with the general sibling combinator? Well, with this combination you get great chemistry. Because now you've created a toggle button that can affect any element of choosing that it preceeds.
Take a look at the HTML below.

```html
<input class="toggler" type="checkbox" />

<ul>
  <li>Foo</li>
  <li>Bar</li>
  <li>Baz</li>
</ul>
```

What I want, is a list that can be toggled on and off. So basically it's hidden by default and can be shown by toggling it. Well with the use of the `:checked` and `+` selectors I can do this. Without writing a single line of JavaScript.

The CSS would look like the following.

```css
.toggler + ul {
  display: none;
}

.toggler:checked + ul {
  display: block;
}
```

Great, now you have a really simple interactive list.

You could use this trick also on a much bigger scale. Because when you use this trick on a top-level element (directly after the body tag), you could target a lot of elements at once.

```html
<body>
  <input id="global-toggler" type="checkbox" />

  <div id="page-wrapper">…</div>
</body>
```

And with the use of a label I can target the checkbox from anywhere in HTML.

```css
#global-toggler:checked + #page-wrapper {…}
```

By using this as a prefix to other elements you can do a lot. Like make a dark and light mode, which I will get to later.

And a great thing about this trick is that it's supported almsot everywhere. On [caniuse](https://caniuse.com/#search=selectors) it shows you a 98.67% browser support, which is great.

### Visuallyhidden

When you use the checkbox trick, you probably don't want to see the checkbox itself; just the label. When you try to use `display: none` the checkbox trick won't work. Because the checkbox doesn't exist. Do hide the checkbox and not break it, you can use the visuallyhidden trick. How you do it is up to you, I have my way of doing it.

The HTML:

```html
<input id="myId" class="visuallyhidden" type="checkbox" />
```

The CSS:

```css
.visuallyhidden {
  position: absolute;
  right: 100%;
  z-index: -1;
  opacity: 0;
}
```

You can also use this trick to help screenreaders. You could place navigation at the top, so blind users can navigate quickly. The HTML looks like this:

```html
<body>
  <a href="#header-hook" class="visuallyhidden">Skip to header</a>
  <a href="#article-hook" class="visuallyhidden">Skip to article</a>
  <a href="#footer-hook" class="visuallyhidden">Skip to footer</a>

  <!-- Main content -->
</body>
```

### CSS variables

Another great thing CSS has to offer are CSS variables. CSS allows you to create variables that you can use throughout the page, or just on a component. With a browser support of 91.11% I think it's save to use, but that just depends on the use case.

You can create and use CSS variables really easily. You can add them to an element and the element its children will have access to it. See below.

```css
.article {
  --variable: red;
  --longer-variable-name: blue;
}
```

You can also define them at the page's root element like so:

```css
:root {
  --primary-color: yellow;
  --secondary-color: red;
}
```

And how you actually use them is also really easy. This is shown below.

```css
.article {
  background: var(--primary-color);
  color: var(--secondary-color);
  /* You can also use them in calc() */
  width: calc(var(--container-width) - 10%);
}
```

### Import

There's one more thing that I learned that I want to share. The `@import` rule. The `@import` rule makes it possible to use styles from another stylesheet. It's a great way to modularize you CSS. It again, is really easy.

Let say I have three main elements:

- Header element
- Article element
- Footer element

I can write a seperate file for each element, and put them in a components folder. By doing this I can keep my CSS files/code more organized and.

```css
/* Main.css; the place to import all your modules */
@import "components/header.css";
@import "components/article.css";
@import "components/article.css";
```

### Example

Earlier in this article I mentioned a dark and light mode. Well we're here. Let's use the things I discusses and make a simple dark and light mode.

First the HTML:

```html
<body>
  <input id="mode-switch" class="visuallyhidden" type="checkbox" />

  <!-- The purpose of the page-wrapper is to have a second <body> that can store variables. These variables can be modified by using :checked The page-wrapper should be fullpage width and height.  -->
  <div id="page-wrapper">
    <label for="mode-switch">Switch to:</label>
    <main>
      <header>
        <h1>Main title</h1>
        <p>Jesse Dijkman</p>
        <small>Publication date: Today</small>
      </header>
      <section>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Ea eos natus
          nulla, esse provident harum asperiores, dicta laboriosam fugiat
          possimus obcaecati, quia repellendus quaerat. Est mollitia explicabo
          facilis nisi cumque.
        </p>
      </section>
    </main>
  </div>
</body>
```

Now the CSS:

```css
* {
  /* CSS RESET */
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.visuallyhidden {
  /* Invisible not display: none */
  position: absolute;
  right: 100%;
  opacity: 0;
  z-index: -1;
}

label[for="mode-switch"] {
  border: solid 1px var(--text-color);
  padding: 0.5em;
  position: absolute;
  right: 1em;
}

label[for="mode-switch"]::after {
  content: var(--switch-to);
}

#mode-switch:checked + #page-wrapper {
  --background-color: black;
  --text-color: white;
  --switch-to: " light mode";
}

#page-wrapper {
  --background-color: white;
  --text-color: black;
  --switch-to: " dark mode";
  position: relative;
  border: solid 1px red;
  min-height: 100vh;
  background: var(--background-color);
  color: var(--text-color);
  padding: 0 1em;
}

main {
  max-width: 800px;
  margin: 0 auto;
}

header + section {
  margin-top: 1em;
}
```

Here you go. By just combining the `:checked` pseudo-class and the general sibling selector I can alter the variables in the `#page-wrapper`. [DEMO](https://codepen.io/WillyW/pen/agJoBq?editors=1100)
