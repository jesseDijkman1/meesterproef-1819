# Classes

#### Author: Jesse Dijkman

If you have been programming functionally it’s hard to grasp the purpose of object oriented programming. Atleast I had a hard time of coming up with a usecase to actually practice it. But to be honest for quick testing it's always faster for me to program functionally. When something works I refactor it into object-oriented code. But since I've been getting familiar with object-oriented programming I have also been using it a lot more.

### What is Object-oriented programming?

Object-oriented programming is a way of programming where you structure your code in **Objects**. Most of the time these objects are created using a **class**; a kind of template.

In this article I'm going to dive into the JavaScript class. How it works? Keywords and more.

### How do classes work?

Classes in JavaScript are used to create instances of objects. This is done using the `new` keyword. You pass in the data which the class uses to create an object. Creating an object using the `new` keyword is really easy.

```js
const obj = new Object()
```

This just creates an empty object, it's the same as using the curly brackets notation.

```js
const obj = {}
```

The `Object` is a constructor function. You can create your own constructor functions with classes and call them using the `new` keyword.

### Writing a class

A class is written with the `class` keyword followed by a name. It's good practice to start the name with an uppercase letter.

```js
class MyClass {…}
```

Inside the curly brackets you start of by defining the `constructor` property.

```js
class MyClass {
  constructor() {}
}
```

The constructor is used to actually pass data to. You can pass data to the constructor just like you would do with a normal function; as a parameter. Then with the `this` keyword you can add the parameter as a property. Which can be accessed everywhere from the class. But you might need to bind some things. I'll get to that later.

```js
class MyClass {
  constructor(data) {
    this.data = data
  }
}

new MyClass("jesse")
```

### Uses

Throughout my minor I had a goal to use classes and understand them. But I had trouble with coming up with a use case. Now I try to use them as much as possible. I use them to create components or data. I once created a drawing game, and this game would take some values and turn them into objects.

### Extends

`extends` is another keyword you can use with a class. But first into the why then into the how.

Let’s say you have a videogame and you have a bunch of characters that can be categorised in the following:

- enemies
- friendly (npc)

you'd have the classes:

```js
class Enemy {}
```

```js
class Friendly {}
```

and you probably don’t want just one single enemy character, you want more kinds of characters that are enemies. Let’s say you want to create a “ghost” and “zombie” enemy. They both need the basic functionalities and properties of the enemy class like:

- Health
- Strength
- Attack()

You don’t want to rewrite these properties and functions, you want them to be inherited. So you can create custom enemies that will look different, have different health and strength, without hardcoding them. You can achieve this by using `extends`. You use `extends` like this:

```js
class Enemy {
  constructor(health, strength) {
    this.health = health
    this.strength = strength
  }
}

class Zombie extends Enemy {}
```

But how do you pass the data to enemy? Well, there's another, wait for it ... keyword 🎉. The `super` keyword. You use the `super` keyword like this:

```js
class Zombie extends Enemy {
  constructor(health, strength) {
    super(health, strength) // When using extends super must be the first thing called in the constructor
  }
}
```

Okay, I called the `super` now what? Let's add a unique method to the `Zombie` class, called `eatFlesh`, which adds health.

```js
class Zombie extends Enemy {
  constructor(health, strength) {
    super(health, strength)
  }

  eatFlesh() {
    this.health += 10 // Health can be accessed
  }
}
```

One last thing about the `super`, you can use it to add a property. For my end assessment of the minor I created a custom-slider component. This component would extend a DraggingEvent class, that takes a target as parameter. But I first had to create the HTML for the slider which I did in the CustomSlider class. So this is basically what I did:

```js
class CustomSlider extends DraggingEvent {
    constructor(inputRange) {
        super()

        super.target = this.createSlider()
    }

    createSlider() {…}
}
```

I added the property later, but this can only be done inside the `constructor`.

### Static

This is the last keyword, I promise. The `static` keyword is used to define a "static method". When I first read this on MDN I didn't get any wiser from it. Then I found an explanation (don't remember where) that made it clearer. So I'll try explain it.

Let say you have a `User` class. And two instances of this class.

```js
class User {
  constructor(name, age, weight) {
    this.name = name
    this.age = age
    this.weight = weight
  }
}

const user1 = new User("Jane", 24, 55)
const user2 = new User("John", 24, 75)
```

Now I want to compare these two users their ages for some reason. I could write a normal function to do that. But to add it as a static method to the `User` class is cleaner, in my opinion. You do this with `static`.

```js
class User {
  constructor(name, age, weight) {
    this.name = name
    this.age = age
    this.weight = weight
  }

  static compareAge(user1, user2) {
    return user1.age === user2.age
  }
}
```

And then you call it directly on the class like so:

```js
…

const user1 = new User("Jane", 24, 55)
const user2 = new User("John", 24, 75)

User.compareAge(user1, user2) // => true
```

and that's the end of the keywords.

But why should you become an object-oriented programmer. In my opinion you're code becomes cleaner and better organized. And almost all the other languages use object-oriented programming. So it reduces the step from JavaScript to Java for example. Atleast that's what I think.
