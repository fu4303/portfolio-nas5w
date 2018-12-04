---
layout: article
title: Frontend JavaScript Interview Question - What is Prototypal Inheritance?
date: 2018-12-03 20:00:00+0200
coverPhoto: https://i.imgur.com/MzGy8Bo.png
---

![](https://i.imgur.com/MzGy8Bo.png)

# Introduction

This is a post in my series "Frontend JavaScript Interview Questions," where I provide answers and examples for common frontend JavaScript interview questions. If you appreciate this post, feel free to follow me on [Medium](https://medium.com/@nas5w), [Github](https://github.com/nas5w), or [Twitter](https://twitter.com/nas5w)!

Note: I am getting most of my interview questions from [this excellent reference](https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/src/questions/javascript-questions.md), but I may deviate from time to time!

# This Post's Question: What is Prototypal Inheritance?

`Prototypal inheritance` is a foundational, powerful, and oft misunderstood concept to JavaScript. When you create a function in JavaScript, it will always have a `prototype` property. (Functions can have properties because they are, in fact, objects).

Just try the following:

```javascript
function Person() {}
console.log(Person.prototype);
// {constructor: ƒ}
// constructor: ƒ Person()
// __proto__: Object
```

We can add to this `prototype` property if we would like.

```javascript
Person.prototype.sayHi = function() {
  console.log("hi!");
};
console.log(Person.prototype);
// sayHi: ƒ ()
// constructor: ƒ Person()
// __proto__: Object
```

So far, so... well... unimpressive. But, what this lets us do is create a `new` object based on our `Person` function that will be able to use these prototypal properties! Follow along to see what I mean.

```javascript
function Person() {}
Person.prototype.sayHi = function() {
  console.log("hi!");
};
let nick = new Person();
nick.sayHi(); // hi!
```

Does nick have a `sayHi` method? Not quite...

```javascript
nick.hasOwnProperty("sayHi"); // false
```

What's going on here is `prototypal inheritance`. When `nick.sayHi()` is called, a `sayHi` method directly belonging to `nick` is looked for, but not found. When this happens, `nick`'s prototypal properties are searched and `sayHi` is successfully found and executed. This is called searching the `prototype chain`.

Did you catch that we may have searched the `prototype chain` a second time without even knowing it? `hasOwnProperty` is not a property we defined for `nick`, nor is a property defined for `Person`. In this case, the `prototype chain` is searched back to the `Object` function object.

```javascript
console.log(Object.prototype.hasOwnProperty);
// hasOwnProperty() { [native code] }
```

## Prototypal inheritance is based on pointers

You might be tempted to think the `sayHi` method was somehow copied onto the `nick` object. This is not the case and can lead to trouble. Take this example.

```javascript
function Person() {}
Person.prototype.sayHi = function() {
  console.log("hi!");
};
let nick = new Person();

// Change Person's prototypal `sayHi` method
Person.prototype.sayHi = function() {
  console.log("Greetings!");
};

nick.sayHi(); // Greetings!
```

If the `sayHi` method was copied to the `nick` object, then it would still be logging `hi!`. Instead, the prototypal method that `nick.sayHi` is _pointing_ to has been changed, and that's what's executed. I'm sure you can imagine how this could be counterintuitive behavior if you didn't understand prototypal inheritance!

But really, this only scratches the surface. For a more comprehensive discussion, I recommend diving into [this excellent article](https://codeburst.io/master-javascript-prototypes-inheritance-d0a9a5a75c4e).
