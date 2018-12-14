---
layout: post
title: This quick trick will make your console.log("10x Better")
---

Yes, it's a clickbaity title, but it's legit. 

![nice-log-console](/images/log-console.jpg){:class="img-responsive"}
###### [A very nice log console](https://logfurnitureplace.com/cedar-lake-log-media-console-clmc.html?gclid=EAIaIQobChMIn_Kqs-Sf3wIVBYbICh1PUgTFEAQYBCABEgIwC_D_BwE)


Herein, I will demonstrate a very simple trick that has changed the way I use `console.log`.

Before I do this though, I just want to make a disclaimer that I in no way endorse the use of `console.log` as your main debugging tool.
Use the `debugger` keyword instead or [breakpoints in the chrome dev tools](https://developers.google.com/web/tools/chrome-devtools/javascript/), [text editor](https://code.visualstudio.com/docs/editor/debugging), or [what have you](https://me.me/i/everything-everything-has-stopped-working-a-problem-caused-the-program-11588426).

But sometimes, you just want to log something out. So in those cases, here is a handy trick for you.

## TLDR

Wrap variables that you are logging with a pair of curly brackets - `{}`.

This will log out the variables using the [object property shorthand](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Property_definitions) to display the name of the variable and the value in a nice readable way without having to manually label what you're logging.

```javascript
const thing = "I am a very lovely thing"
console.log({ thing }) // -> { thing: "I am a very lovely thing" }
// or multiple variables
const nothing = "I am a very lovely nothing"
console.log({ thing, nothing }) // -> { thing: "I am a very lovely thing",
                                //      nothing: "I am a very lovely nothing" }
```

Read on below for more detail.

## Ye Olde Way

Let's set the scene:

You are writing a magnificent intricate program, everything is just sailing right along. 

When suddenly, you look at the screen and everything is <span style="color: red">blood red</span>, and you just don't know where it's coming from.

You say to yourself, hmmm I wonder if the `apples` variable is doing something funny, so you decide to log it out:

```javascript
let apples = 2
let oranges = 5
let bananas = 4
doSomethingWithFruits(apples, oranges, bananas)
console.log(apples) // -> 3
```

Ahh great! `apples` is 3.

Maybe it's a different fruit?

```javascript
let apples = 2
let oranges = 5
let bananas = 4
doSomethingWithFruits(apples, oranges, bananas)
console.log(apples, oranges, bananas) // -> 3 2 8
```

You look at you console and all you see is `3 2 8` and you spend about 5 minutes figuring out which is which.

So then you say, let me label them with text before the value:

```javascript
let apples = 2
let oranges = 5
let bananas = 4
doSomethingWithFruits(apples, oranges, bananas)
console.log("Apples: " + apples, "Oranges: " + oranges, "Bananas: " + bananas)
 // -> Apples: 3 Oranges: 2 Bananas: 8
 ```

This works, but it's time consuming and it's just tedious to do this every time you log something out. 

## console.log() 2.0

Instead, you can log out your variables as follows:

```javascript
let apples = 2
let oranges = 5
let bananas = 4
doSomethingWithFruits(apples, oranges, bananas)
console.log({ apples, oranges, bananas })
 // -> apples: 3 oranges: 2 bananas: 8
 ```

This will give you the same results as the labeling that you did above, but with much less effort.

## How it works

This trick works because of the very useful ES6 feature that I call [object property shorthand notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Property_definitions). 

All this does is take a variable name and turn it into a key value pair of the variable name and it's value.

From the docs:

```javascript
// old way
var a = 'foo', 
    b = 42,
    c = {};

var o = { 
  a: a,
  b: b,
  c: c
};

var a = 'foo', 
    b = 42, 
    c = {};

// Shorthand property names (ES2015)
var o = {a, b, c};

// In other words,
console.log((o.a === {a}.a)); // true
```


## Caveat

If you are trying to log a nested property, this won't work and you will get a syntax error.

```javascript
let car = {
  wheels: 4
}
console.log({ car.wheels }) // -> SyntaxError
```

Instead, you can add a quick label by logging as follows:

```javascript
let car = {
  wheels: 4
}
console.log({ wheels: car.wheels })
```

I hope you enjoyed this trick! 

If you have any questions, please leave a comment below.