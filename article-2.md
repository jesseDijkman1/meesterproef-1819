# RegEx - the basics

#### Author: Jesse Dijkman

In this article I'll go over the basics of regular expressions; what they are, basic syntax and examples. If you've never written a regular expression and like to start somewhere, this article might be helpful for you. With the help of examples I hope the concept will clear up a bit. At the end I'll write a little program that uses a regular expression.

### What are regular expressions?

> A regular expression (regex or regexp for short) is a special text string for describing a search pattern. You can think of regular expressions as wildcards on steroids - [Source](https://www.regular-expressions.info/)

With regular expressions you can find certain patterns in strings. Like a certain word, a combination of words, numbers, (special) characters, etc.

A great thing about regular expressions is that almost all programming languages have regular expressions. So when you learn one, you can learn another pretty easily.

In this article I'm going to focus on the JavaScript regular expressions.

### Syntax

**Warning: Regex syntax is scary**

Well, I warned you, so when you have a mental breakdown at the end you can't blame me. Ofcourse I'm joking, but I will say that regular expressions are one of the scarier looking things in programming.

Let's get into the actual syntax of JavaScript regular expressions and how you create one in JavaScript.

In JavaScript you have two ways of creating a regular expression.

- The literal way
- The constructor-function way

#### Literal way

```js
const regX = /…/
```

#### Constructor-function way

```js
const regX = new RegExp("…")
```

But for now you just need to focus on the literal way because that's the one you will probably use the most. The constructor way also has it's use cases but I won't discuss those here right now, but I'll use it at the end when I make the little program.

### Flags

In regular expressions you can use flags. Flags affect the way the regular expression searches through your text. You put flags at the end of the regular expression or as the second parameter of the constructor-function.

```js
const regX = /…/flag
```

One of the flags you will probably use the most is the **global flag**, `g`. The global flag tells the regular expression to look for all the matches. When you don't use the global flag, the regular expression will stop its search after the first match. Two other flags you will see in this article include the **multiline flag** `m` and **case insensitive flag** `i`, but those will be explained later on.

### Searching specific things

> _I like regular expressions_

If you want to check a piece of text for a certain word, character or anything specific, you can do this by just typing it as the pattern.

Let's just select the character `e`.

```js
const regX = /e/
```

> _I lik**e** regular expressions_

Whoops, looks like I forgot to add the global flag. Let's fix this.

```js
const regX = /e/g
```

> _I lik**e** r**e**gular **e**xpr**e**ssions_

Now that's more like it. You can do the same with words and numbers.

```js
const regX = /like/g
```

> _I **like** regular expressions_

See, it's not that hard ... yet.

### Character classes

When writing regular expressions, most times you'll be using a things called: **character classes**. Some useful ones are:

- `\w` Select all none special characters
- `\d` Select all digits
- `\s` Select all whitespace (tabs and spaces)
- `\W` Select all characters that aren't a letter or digit
- `[abc]` Select a, b or c
- `[^abc]` Don't select a, b or c
- `[a-z]` Select all charactes between a and z

> _I like regular expressions_

```js
const regX = /\w/g
```

> _**I like regular expressions**_

But wait, don't be fooled. It won't select the words like this yet. It will look more like this:

> _**I &nbsp;&nbsp; l i k e &nbsp;&nbsp; r e g u l a r &nbsp;&nbsp; e x p r e s s i o n s**_

Why this happens is because the regular expression just matches characters that fit the pattern. Because the pattern matches every non-special character it's going to match them individually. To actually match the words you need a **quantifier**.

### Quantifiers

Another thing you will use a lot when writing regular expressions is a quantifier. A quantifier tells the regular expression to match as many as possible or a certain amount.
Here's a list of some useful quantifiers:

- `*` Match 0 or more
- `+` Match 1 or more
- `?` Match 0 or 1
- `{3}` Match exactly 3
- `{1, 3}` Math between 1 and 3

Using a quantifier for the previous example would look like this:

```js
const regX = /\w+/g
```

Now you get the whole words as matches instead of the individual characters.

### Groups

And finally the last thing I'm going to add is the use of groups. To write a group you can use normal brackets `()`. In JavaScript a group is useful when you want to select different parts of a match. Here's an example.

> _Jesse Dijkman_

Say you have a user input for a name, and you have a first and lastname. The expression will look like this:

```js
const regX = /(\w+)\s{1}(\w+)/
```

### Multiline and case insensitive flag

A very useful flag that you can use for form validation, data parsing and other things is the **mutliline flag** `m`. The multiline flag comes with two anchors that only work when the multiline flag is declared. The start `^` and end `$` anchor. Another flag that might be useful is the case **insensitive flag** `i`, which makes the regular expression select words where capitalization doesn't matter.

> _Jesse Dijkman_

First I need to think about what I want the expression to do.

- Check for non-special, non-digit characters
- Select only when there are two words
- Capital letters don't matter

Let's write the first part.

```js
const regX = /([a-z]+)/i
```

> _**Jesse** Dijkman_

The first name is now matched. Let's select the lastname. To do this you need to keep in mind that you need a space in between. So the expression will look like this:

```js
const regX = /([a-z]+)\s([a-z]+)/i
```

> _**Jesse Dijkman**_

But this expression will also match the following:

> _**Jesse Dijkman** - professional dumbass_

This might not seem like a problem but when I want to validate this input, I want it to match EVERYTHING or nothing at all. This is where the multiline flag and the anchors come in handy. Because I want the expression to look for two words on one line. So I need to specify a beginning and an ending. Let's add this to our expression.

```js
const regX = /^([a-z]+)\s([a-z]+)$/im
```

This won't match this:

> _Jesse Dijkman - professional dumbass_

but will match this:

> _**Jesse Dijkman**_

You can test it out yourself by using [regexr.com](https://regexr.com/). Which is a tool I used throughout the minor I followed.

### Example

Let's create an example that uses a regular expression and the function `replace()`. There are more functions where regular expressions can be used, you can find a list on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#Working_with_regular_expressions).

I'm going to make a function that can convert a string to CamelCase. What I want is to add a prototype function to the `String` constructor. So I can call it directly on a string, just like the `.toUpperCase()` function.

```js
String.prototype.toCamelCase = function() {
  // I'm empty inside :C
}
```

Next step is to write the regular expression. And get the actual string from the constructor.

```js
String.prototype.toCamelCase = function() {
  const regX = /\s+(\w)/g
  const string = this.toString()
}
```

What this regular expression does is search for whitespaces that are followed by a single character. I grouped the character because I need to replace the whitespace and character with the same character uppercase.

Now let's add the replace function.

```js
String.prototype.toCamelCase = function() {
  const regX = /\s+(\w)/g
  const string = this.toString()

  string.replace(regX, (fullMatch, group1) => {})
}
```

When you use replace with a regular expression the first group is the fullmatch and the second is the first group.

Now we just need to return the capitalized character.

```js
String.prototype.toCamelCase = function() {
  const regX = /\s+(\w)/g
  const string = this.toString()

  return string.replace(regX, (fullMatch, group1) => {
    return group1.toUpperCase()
  })
}
```

Which is the same as

```js
String.prototype.toCamelCase = function() {
  const regX = /\s+(\w)/g
  const string = this.toString()

  return string.replace(regX, (fullMatch, group1) => group1.toUpperCase())
}
```

Done!

To sum it all up. We went over the basics of the syntax. Showed some examples and created a function that uses a regular expression.

I personally used regular expressions throughout my minor for a variety of things, like:

- HTML cleaner
- Markdown parser
- Extracting cookies (session.id) from websockets
- Data parsing
- And probably more

I hope this article showed you a glimpse of what you can do with regular expressions. I personally love regular expressions because they're powerful and have a lot of usecases.

### Sources

- https://regexr.com/
- https://www.regular-expressions.info/
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions
