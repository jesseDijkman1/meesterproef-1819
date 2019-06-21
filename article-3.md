# Classes aren't scary

#### Author: Jesse Dijkman

If you've only been programming functionally, it’s hard to grasp the purpose of object-oriented programming. Atleast I had a hard time with coming up with a usecase to actually practice it. But to be honest for quick testing it's always faster for me to program functionally. When something works I refactor it into object-oriented code. But ever since I've been getting familiar with object-oriented programming I've been doing it a lot more.

### What is Object-oriented programming?

Object-oriented programming is a way of programming where you structure your code in **Objects**. Most of the time these objects are created using a **class**; a kind of template or blueprint.

In this article I'm going to dive into the JavaScript class. And try to convince you to start with object-oriented programming.

### How do classes work?

Classes in JavaScript are used to create objects, these objects are instances of the class. You can create an instance of a class with the `new` keyword, followed by the class name. You pass in the data which the class uses to create an object.

```js
const obj = new Object()
```

Here the `Object` is the constructor and is being called by the `new` keyword. This evaluates into a new, empty object. You can achieve the same result using the curly brackets.

```js
const obj = {}
```

### Writing a class

A class is written with the `class` keyword followed by a name. It's good practice to start the name with an uppercase letter.

```js
class MyClass {…}
```

Inside the curly brackets you start with defining the `constructor` method. If you use a class as a constructor you need the constructor method.

```js
class MyClass {
  constructor() {}
}
```

The data you want to create a new object with is passed to the `constructor`. You can pass data to the constructor just like you would do with a normal function; as a parameter. Then with the `this` keyword you can define a property of the object. This property can be accessed from anywhere in the object, as long as the scoping is OK. In that case you might need to use `bind()`, `call()` or `apply()`.

```js
class MyClass {
  constructor(data) {
    this.data = data
  }
}

new MyClass("WoW")
```

### Uses

Throughout my minor I had a goal to use classes and understand them. But I had trouble coming up with a usecase. Now I try to use them as much as possible. I use them to create components or data. For a course of the minor I had to make something with websockets. I created a drawing app. For this game I created three classes: `Game`, `PenSettings`, `Drawing`. It's also the first time I tried to use `extends` which I'm going to try and explain ... now.

### Extends

`extends` is another keyword you can use with a class. But to understand `extends` you need to know why you would ever you use it.

Let’s say you have a videogame and you have a bunch of characters that can be categorized by:

- Enemy
- Friendly

you'd have the classes:

```js
class Enemy {
  constructor(health, strength) {
    this.health = health
    this.strength = strength
  }
}
```

```js
class Friendly {
  constructor(health) {
    this.health = health
  }
}
```

and you probably don’t want just one single enemy character everytime, you want more kinds of characters that are enemies. Let’s say you want to create a “ghost” or a “zombie” enemy. They both need the basic methods and properties of the enemy class like:

- Health
- Strength
- Attack()

You don’t want to rewrite these properties and functions, you want them to be inherited. So you can create custom enemies that will look different and behave different, without too much effort. You can achieve this by using `extends`.

```js
class Zombie extends Enemy {}
```

But how do you pass the data to `Enemy`? Well, there's another, wait for it ... keyword. The `super` keyword.

> The super keyword is used to access and call functions on an object's parent - [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

When you use `extends` the first thing you need to do is call `super(…)` in the `constructor`.

```js
class Zombie extends Enemy {
  constructor(health, strength) {
    super(health, strength)
  }
}
```

Now that you've call `super`, let's add a unique method to the `Zombie` class, called `eatFlesh`. But you can't use `super` in the `eatFlesh`. You can just access it with the `this` keyword.

```js
class Zombie extends Enemy {
  constructor(health, strength) {
    super(health, strength)
  }

  eatFlesh() {
    this.health += 10
  }
}
```

One last thing about the `super`, you can use it to add a property. For my end assessment of the minor I created a custom-slider component. This component would extend a DraggingEvent class, that takes a target as parameter. But I first had to create the HTML for the custom slider which I did in the CustomSlider class with a method. You can see below what I did.

```js
class CustomSlider extends DraggingEvent {
    constructor(inputRange) {
        super()

        this.slider = this.createSlider()

        super.target = this.slider
    }

    createSlider() {…}
}
```

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

const user1 = new User("Jane Doe", 24, 55)
const user2 = new User("John Doe", 24, 75)
```

Now I want to compare these two users their ages for some reason. I could write a normal function in the global scope, but I dont' want that. It's only used by the `User` class, so it should be inside. But I don't need the compare method on the instances, so I use the `static` keyword.

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

And to use it, you can just call it on the class itself.

```js
const user1 = new User("Jane", 24, 55)
const user2 = new User("John", 24, 75)

User.compareAge(user1, user2) // => true
```

So yeah, that's about it. All you need to know about classes (not really). I hope this article helped to motivate you to learn classes and use them. And when you actually learn them and object-oriented programming, you'll have less trouble learning languages like: Java.

### Sources

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super
- https://developer.mozilla.org/nl/docs/Web/JavaScript/Reference/Klasses/extends
- https://github.com/jesseDijkman1/real-time-web-1819
