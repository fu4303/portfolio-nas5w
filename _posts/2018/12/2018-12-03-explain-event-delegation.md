---
layout: article
title: Frontend JavaScript Interview Question - What is Event Delegation?
date: 2018-12-03 20:00:00+0200
coverPhoto: https://i.imgur.com/6rVWfET.png
---

![](https://i.imgur.com/6rVWfET.png)

# Introduction

This is a post in my series "Frontend JavaScript Interview Questions," where I provide answers and examples for common frontend JavaScript interview questions. If you appreciate this post, feel free to follow me on [Medium](https://medium.com/@nas5w), [Github](https://github.com/nas5w), or [Twitter](https://twitter.com/nas5w)!

Note: I am getting most of my interview questions from [this excellent reference](https://github.com/h5bp/Front-end-Developer-Interview-Questions/blob/master/src/questions/javascript-questions.md), but I may deviate from time to time!

# This Post's Question: What is Event Delegation?

Event delegation is adding an `event listener` to a parent element on the DOM rather than attaching it to each child element. To see why we would want to do this, let's take a common example: items in a list. Here is our HTML:

```html
<ul id="my-list">
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

Let's say we want to `console.log` the contents of a list item when it is clicked. Without event delegation, we may write the following javascript:

```javascript
let listItems = document.querySelectorAll("#my-list li");
for (let i = 0; i < listItems.length; i++) {
  listItems[i].addEventListener("click", e => {
    console.log(e.target.innerHTML);
  });
}
```

That will work, but let's say we need to dynamically add another list item. Our javascript becomes something like this:

```javascript
let listItems = document.querySelectorAll("#my-list li");
for (let i = 0; i < listItems.length; i++) {
  listItems[i].addEventListener("click", e => {
    console.log(e.target.innerHTML);
  });
}

let list = document.querySelector("#my-list");
let newItem = document.createElement("li");
newItem.innerHTML = "Four";
list.appendChild(newItem);
```

But now if we try to click this fourth item, nothing is logged in the console (see this in all its failing glory [here](https://jsfiddle.net/nas5w/kxdc74gz/4/)). That's because the fourth item did not exist when the `event listeners` were added to the other list items. We _could_ technically add a new `event listener` every time we add a new list item, but this is cumbersome. Instead, we can use **event delegation**!

To use `event delegation`, we simply have to realize that the click event _bubbles up_ from the `li` elements to the `ul` element. We can attach a click `event listener` to the parent `ul` element and perform our `console.log` action if the click event `target` is an `li`. Our javascript becomes this:

```javascript
let list = document.querySelector("#my-list");
list.addEventListener("click", e => {
  if (e.target && e.target.nodeName == "LI") {
    console.log(e.target.innerHTML);
  }
});

let newItem = document.createElement("li");
newItem.innerHTML = "Four";
list.appendChild(newItem);
```

As you can see in [this fiddle](https://jsfiddle.net/nas5w/kxdc74gz/7/), the desired `console.log` works perfectly on the fourth element without having to manually add additional `event listeners`.
