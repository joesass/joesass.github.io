---
layout: post
title: Javascript Closures
---

### Why should I care about Closures?

If you want some motivation to learn closures, maybe you should take a look at what some of the public voices are saying about it.

From Eric Elliot's [Master the Javascript Interview Article - What is a Closure?][ref]:

> I’m launching the series with a question that is often my first and last question in my JavaScript interviews. Frankly, you can’t get very far with JavaScript without learning about closures. ...
> Not knowing the answer to this question is a **serious red flag**. (emphasis in original)


### What's a Closure?

Here's the almighty Wikipedia:

> In programming languages, closures (also lexical closures or function closures) are techniques for implementing lexically scoped name binding in languages with first-class functions.

Yeah I totally understand that.

From the introduction on MDN:

> Closures are functions that refer to independent (free) variables
> (variables that are used locally, but defined in an enclosing scope). In
>  other words, these functions 'remember' the environment in which they
>  were created.

Ouch!

Here is a Stack Overflow Thread Comment:

> Two one sentence summaries:

> - a closure is one way of supporting first-class functions; it is an expression that can reference variables within its scope (when it was first declared), be assigned to a variable, be passed as an argument to a function, or be returned as a function result. Or
- a closure is a stack frame which is allocated when a function starts its execution, and not freed after the function returns (as if a 'stack frame' were allocated on the heap rather than the stack!).

My head is hurting. What's going on here?


Here's the simple way I explain it to myself:

> A Closure is a function along with it's outer scope at the time the function was defined

Also interesting to note is that the scope is recreated every time the function gets defined.

### Two main use-cases

The main issue that I had with closures is coming up with a way that they would be useful. So behold I present the two main use-cases:

1 - To Create a custom function generator

```js
// Create an add function that adds a certain number to the argument
var addNumber = function(number){
    return function(num){
      return num + number
    }
}

// Set the Closure var number to 1
var addOne = addNumber(1)

addOne(1)
// ->  2
addOne(3)
// -> 4

// Set the closure var number to 2
addTwo = addNumber(2)
addTwo(1)
// -> 3
addTwo(2)
// -> 4

// Create a Greeter function that greets with a certain word
function sayHi(greeting){
    return function(name){
        return greeting + " " + name
    }
}

// set the closure var greeting to Hello
var sayHello = sayHi("Hello")

sayHello("John")
// -> "Hello John"
sayHello("Bob")
// -> "Hello Bob"

// set the closure var greeting to Bonjour
var sayBonjour = sayHi("Bonjour")
sayBonjour('Frank')
// -> "Bonjour Frank"
sayBonjour('Charlie')
// -> "Bonjour Charlie"
```
2 - To make functions with private variables

```js
var setPassword = function(password){
  return function(passwordInput){
    return password == passwordInput
  }
}

var setPassword = setPassword("1234")
checkPassword('wrongpassword')
// -> false
checkPassword('1234')
// -> true
```


### How does it work


There are two parts to how closures come to be - Lexical Scoping and First Class Functions.

#### Lexical Scoping

When javascript evaluates an expression and sees something that is not a keyword, the way it find out what it is, is by looking in the immediate scope, then looking in the scope right above it and then the one above that one and so on until it gets to the end of the scope chain.


#### First Class Functions

In javascript, unlike some other languages, functions are *first-class-citizens*, and they are treated just like every other value. Which means they can be assigned to a variable, be passed as an argument to a function, or be returned as a function result.

When you combine these two concepts, and you return a function from within another function, due to lexical scoping, the inner function will have access to the outer function's scope, and you will have a beautiful closure.


#### Finding Closures in Chrome Dev Tools

One thing that I found that was really cool, from a youtube video, is that when you inspect a closure on Chrome Dev Tools, you can see the scope, by clicking on scopes then drilling down to the `Closure` scope so that you can inspect what variables are set to in the outer scope:

![Closure_in_dev_tools](http://i.giphy.com/aMUEHbMd88qTC.gif)


### Ruby Dooby doo

For those of you wondering if there is such a thing as closures in Ruby, and I know there are many of you out there, I have some good news for you.

#### Blocks, Procs

Ruby has a datatype called a block that we are familiar with from iterator methods like `#each`

```ruby
array.each do |x|
# This is a block
end
```

A block is not an object and doesn't inherit from anything. It is just a set of procedures.

Similar to a block is a `proc`. From the [Ruby Docs](http://ruby-doc.org/core-2.4.0/Proc.html)

>`Proc` objects are blocks of code that have been bound to a set of local variables. Once bound, the code may be called in different contexts and still access those variables.

Using the Proc datatype, we can create closures the same way we do in javascript, by defining procs within another block or proc and returning them into a variable and viola.

```ruby
def gen_times(factor)
  return Proc.new {|n| n*factor }
end

times3 = gen_times(3)
times5 = gen_times(5)

times3.call(12)               #=> 36
times5.call(5)                #=> 25
times3.call(times5.call(4))   #=> 60
```

As you can see, closures are pretty cool, useful, and important.

Thanks for Reading!

### Links

[Wikipedia](https://en.wikipedia.org/wiki/Closure_computer_programming)

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

[Site Point Article](https://www.sitepoint.com/javascript-closures-demystified/)

[ref]: https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#.t6gjgs913

[Stack Overflow Thread](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)

[Scotch - Ruby Closures](https://scotch.io/tutorials/understanding-ruby-closures)

[Adam Waxman's Blog post about Blocks, Procs, and Lambdas](http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/)
