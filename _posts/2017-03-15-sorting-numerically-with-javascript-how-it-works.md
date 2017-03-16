---
layout: post
title: Sorting Objects Through their Properties Numerically with Javascript - How it Works
---


Recently, when writing the code for my (as of yet unpublished) chrome extension, [Historian](https://github.com/joesasson/historian), which interacts with the browser history and returns interesting statistics, I came across a simple but interesting problem.

I was trying to sort an array of javascript objects by the value of one of the properties. Here is an example:

```js
var arrayOfObjects = [
  { name: "Bob", age: 22, hairColor: 'black' },
  { name: "Mary", age: 28, hairColor: 'blonde' },
  { name: "Steve", age: 15, hairColor: 'red' },
  { name: "Jorge", age: 44, hairColor: 'brown' },
  { name: "Alice", age: 32, hairColor: 'purple' }
]
```

We want to sort this array by the value of the age property in ascending order.

I took a few detours on the way to solving this problem which were pretty interesting. You can read the notes that I took [here](https://github.com/joesasson/historian/blob/master/notes.md). It also has some other interesting notes that I took to solve other problems.

## Basic Sort Functions in Javascript

In order to understand what we're going to implement, we need to understand how the basic sort function works. We'll start with a simple sort algorithm called the [Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort).

The bubble sort simply compares each element in the array to the next element and if the next element is lower than it, swaps it for that element. This procedure repeats until there are no more swaps to be made.

I decided that the best way to really understand the algorithm was to implement it myself.

So here is the challenge:

> Given an array of numbers, modify the original array to be in ascending order.

So for example we have an array:

```js
array = [5, 2, 3, 4, 1]

// We want array == [1, 2, 3, 4, 5]
```
Here are the steps to the solution:


1. Loop through the array with an index (i)
2. Check if the value at the current index (i) is greater than the value at the next index (i + 1)
3. If it is, store the value of i in a temporary variable
4. Set the value of i to the value of i + 1
5. Set the value of i + 1 to the value of the temporary value


Here's the code implementation:
```js
function partialSort(array){
  for(var i = 0; i < array.length - 1; i++){
    if(array[i] > array[i + 1]){
      var temp = array[i]
      array[i] = array[i + 1]
      array[i + 1] = temp
    }
  }
}
// will modify argument of [5, 2, 3, 4, 1] to [2, 3, 4, 1, 5]
```
Do you see what happened?

The 5 moved all the way to the end!

But our array is still not sorted :(
This is because our loop only went through the array once.
We need to wrap it all in a while loop that keeps this procedure going until there are no more swaps.

Let's update our list of steps:


- Set a flag variable called sorted and set it to false
- create a while loop that will run the sort steps that we defined above if sorted is false
- Inside the while loop:
  - Set the sorted value to true
  - Loop through the array with an index (i)
  - Check if the value at the current index (i) is greater than the value at the next index (i + 1)
  - If it is, store the value of i in a temporary variable
  - Set the value of i to the value of i + 1
  - Set the value of i + 1 to the value of the temporary value
  - Set the value of sorted to false

Here's the code:

```js
function fullSort(array){
  var sorted = false
  while(!sorted){
    sorted = true
    for(var i = 0; i < array.length - 1; i++){
      if(array[i] > array[i + 1]){
        sorted = false
        var temp = array[i]
        array[i] = array[i + 1]
        array[i + 1] = temp
      }
    }
  }
}

// If we run fullSort on [5, 2, 3, 4, 1] it will now be [1, 2, 3, 4, 5]
```

It totally works!

Obviously, this is a terrible way to do sorting and there are many performance issues with this solution and there are much better solutions. But it was pretty cool to implement this myself.

## Javascript's built in solution

Javascript has a built-in function on arrays called, you guessed it, `sort()`. The documentation is pretty well written with good examples on [MDN - Array.prototype.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort).

Reading through it we can see that it is pretty easy to use the function. Just call `sort()` on it! So for our example above we would just call `array.sort()` and we would get `[1, 2, 3, 4, 5]`.

This result however, is positively misleading. That's because the sort is not really numerical, it's based on the first character's unicode value.

This means that if you had double digit numbers like `[2, 10, 3, 8]`, the 10 would end up before the 2! This is because the unicode value of 1 is lower than a 2.

## Javscript's built-in solution to numerical sorts

The answer to this, like all things, is just to keep reading.

If we scroll down the page a bit we'll see that the documentators already acknowledged this and addressed it!

The way they addressed it is with something called a `compareFunction`.

## CompareFunction

This is the thing that makes the sort function really useful. What it is is an optional callback parameter that will do the comparison that **we** want to do. Not the dumb built-in one. Just kidding, the built-in one is pretty kickass.

This is how our sort function deals with the compareFunction:

- If the `compareFunction(a, b)` returns a negative number, leave a before b
- If the return value is 0 do nothing
- If the return is a positive number, put b before a

## Sorting Numerically

Now, if we want to compare wether a is greater than b numerically, we simply need to return the value of `a - b`.

If the result is a negative number than we know that b is greater than a and we do nothing.
If the result is a positive number that means that a is greater than b.

Just some examples:

```js
[5, 2, 3, 4, 1]
// 5 - 2 = 3 = positive number = swap
// 2 - 3 = -1 = negative number = don't swap
// 5 - 1 = 4 = positive number = swap
```

The code looks like this:

```js
var numbers = [4, 2, 5, 1, 3];
numbers.sort(function(a, b) {
  return a - b;
})

// number == [1, 2, 3, 4, 5]
```

## Sorting an object based on a property

Now we know how to sort a simple array numerically. But how do we sort the object that we wanted to sort above though?

The answer, yet again, is to keep reading.

We can simply define our compare function to return the difference of the property that we want.

For our example above:

```js
var arrayOfObjects = [
  { name: "Bob", age: 22, hairColor: 'black' },
  { name: "Mary", age: 28, hairColor: 'blonde' },
  { name: "Steve", age: 15, hairColor: 'red' },
  { name: "Jorge", age: 44, hairColor: 'brown' },
  { name: "Alice", age: 32, hairColor: 'purple' }
]

arrayOfObjects.sort(function(a, b){
  return a.age - b.age
})

// arrayOfObjects == [
//   { name: "Steve", age: 15, hairColor: 'red' },
//   { name: "Bob", age: 22, hairColor: 'black' },
//   { name: "Mary", age: 28, hairColor: 'blonde' },
//   { name: "Alice", age: 32, hairColor: 'purple' },
//   { name: "Jorge", age: 44, hairColor: 'brown' }
// ]
```

We now have our objects sorting based on the age. This is useful in many situations.

I know that this was a pretty simple problem to solve and the answer was right there in the documentation, but it was extremely rewarding to get in the weeds on the lower level of things and understanding how the plumbing works.
