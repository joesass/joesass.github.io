---
layout: post
title: Basic Refactoring In Javascript
tags: refactoring javascript DRY
---

![Refactoring](https://media3.giphy.com/media/AW9vY1bbQA9MY/giphy.gif)


## Why is Refactoring important?

As Developers, we often find ourselves copying and pasting code. Which is to say, out of convenience and a lack of foreplanning, we tend to take the easy way out when we start writing something. 

Sometimes, when reviewing or trying to modify our code, we realize that modifying one thing means that we need to modify our source in 10 different places. That leads to errors, inconsistencies, tedium, and just plain old [plumbusus](https://i.ytimg.com/vi/eMJk4y9NGvE/maxresdefault.jpg).

While we should certainly plan to not have a lot of repetition in our code, things don't always go according to plan. 

I recently wrote a Google Sheets Add-On to help manage inventory worksheets. 

In the app, I wrote three very similar functions that were almost identical except for slight differences. I did take the easy way out (or the hard way if you look at it in a long-term perspective) and copy and pasted my first function and modified it for each copy. Oops.

So after I got it all to work, I decided to make a minor change that affected all three function, and I realized that I would have to find the relevant code, and change it three times. In this process, I forgot to change it in one of the places, and ended up with a bug that I had to hunt down.

I needed to refactor my code to reuse that line of code. So where to begin?

## Refactoring Variables

So I decided I would like to have all my functions share certain variables. That's the easy part. Or so I thought. 

The easiest way to do this is just to create a global variable that all the functions share. But [global variables are bad!](http://wiki.c2.com/?GlobalVariablesAreBad) (I didn't actually read this but I should). 

Also, I ran into a different Google Sheets related problem: 

The add-on that I created calls the Google API function [`getActiveSpreadsheet()`](https://developers.google.com/apps-script/reference/spreadsheet/spreadsheet-app#getActiveSpreadsheet()) which is only available in container-bound scripts (which means the script is defined to work on one specific sheet). The exception to this rule is if the `getActiveSpreadsheet()` is called from the `onInstall()` or `onOpen()` function (the `onInstall()` actually calls the `onOpen()`). I was also able to make these calls from my functions because they were being defined by `onOpen` through the menu creation. So I was not able to make these calls in my global scope. Therefore I was not able to share the variables :(

## Creating a Shared Object

But, this turned out to be a blessing in disguise, because I had to search for another way of sharing these variables and I came across an [answer on Stack Overflow](https://stackoverflow.com/questions/3201473/jquery-sharing-vars-between-functions/3201563#3201563) which enlightened me. 

Basically, the solution to sharing variables the right way in javascript is to create a function which initializes all the variables and returns an object with getters and setters for each variable.

I happened to skip the part of the getters and setters, but I did create a function which initialized the variables the regular way, and returned them all in one object.

So this is similar to what my code looked like:

```javascript

function sharedObjectCreator(){
  var something = "hello"
  var somethingElse = 20 + 5

  return {
    something: something,
    somethingElse: somethingElse
  }
}

``` 

P.S the above return statement has been given a pretty cool shorthand by ES6. When returning variables as properties with the same name you can skip the colon and the right hand side expression. So in the above example I could have just done something like this:

```javascript
return {
  something,
  somethingElse
}
```

I find this very attractive. (Try it in your browser now, it's really cool)
The sad part is that Google has not implemented this version of javascript yet for Google Apps Scripts, so I couldn't actually use it in this project.


## Using the Shared Object Creator

The way to use this object creator is to run the function as an assignmnt to a new variable which will now store the returned object:

```javascript
var sharedObject = sharedObjectCreator()

// We can now access the variables from the shared object as properties
sharedObject.something // -> "hello"
```
Now, I was able to access these properties form within my functions!

The only caveat (Google Apps specific) which I wasted hours on, is that I needed to initialize the shared object at the top of each function because trying to initialize once globally for all the functions turned out to be a mistake for the same reason as above (calling `getActive()` in the global scope does not work).

## Sharing and Refactoring Functions

Now extracting functions is a little bit harder. What I mean by extracting functions is taking lines of repeated code that is shared and turning it into a function like such:

```javascript
var a = [1, 2, 4, 7]
a.push(5)
a = a.sort()
a = a.map((e) => { return e + 1 })
```

So normally I can just take all that and wrap it in a function:

```javascript
function makeArrayAndDoStuffToIt(){
  var a = [1, 2, 4, 7]
  a.push(5)
  a = a.sort()
  a = a.map((e) => { return e + 1 })
  return a
}

var a = makeArrayAndDoStuffToIt()
```

Now I can share that function across my script.

But what if I the array was defined in the context of the block that it was in? In other words, what if the array was different each time that I wrote this code?

So the answer to this is to make the function skip the creation of the variable, but accept an array as a parameter. Then only do stuff to it. The code would look something like this:

```javascript
var a = [1, 2, 3, 5]
function doStuffToArray(array){
  array.push(5)
  array = array.sort()
  array = array.map((e) => { return e + 1 })
  return array
}

var modifiedA = doStuffToArray(a)
```

Now I can pass in any array and the same operations will be performed on it.

## Sharing is Caring

Now that we know how to extract the function, we can simply define the extracted function in our shared object creator, then return it in our object. Then we can use it across our code!

Here is a simple (completely fantastical and fictional) example of the final result of all the sharing:

```javascript
function sharedObjectCreator(){
  var cookType = ""
  var somethingElse = 20 + 5

  function doStuffToArray(array){
    array.push("potato")
    array = array.map((e) => { return cookType + " " + e })
    return array
  }

  return {
    cookType,
    somethingElse,
    doStuffToArray,
    setCookType: function(type){
      cookType = type
    }
  }
}

var sharedObject = sharedObjectCreator()

function makeBakedPotatoes(){
  var potatoes = ["potato", "potato", "potato"]

  sharedObject.setCookType("baked")
  var bakedPotatoes = sharedObject.doStuffToArray(potatoes)
  console.log(bakedPotatoes)
  return bakedPotatoes
}

function makeFries(){
  // I like fries better usually
  var potatoes = ["potato", "potato", "potato", "potato", "potato", "potato", "potato"]

  sharedObject.setCookType("fried")
  var fries = sharedObject.doStuffToArray(potatoes)
  console.log(fries)
  return fries
}
```

I found this concept of using shared objects really cool and also a fundamental part of the larger theme of using modules and application architecture (which I have explored, but not fully). Hopefully, I will devote some time to writing about those concepts in a future blog post. Thank you for reading!