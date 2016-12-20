---
layout: post
title:  "Converting Arrays to Hashes (in the desert)"
date:   2016-12-16 10:16:50 -0800
categories: ruby
---

Our story begins in the night. You're stuck in the desert with no food or water. There are yellow eyeballs peering out at you from the darkness and a faint hissing can be heard in the distance. You start to get worried. You look for something, a weapon, a shield, or something that can help you survive the night.

![Desert](http://orig06.deviantart.net/02b1/f/2012/158/0/2/desert_night_sky_by_monkypoo-d52lpy6.jpg)

You stick your hand in your pocket. Out comes an array.

Huh? How did this get into my pocket?

It doesn't matter. You are desperate and you have no choice but to look inside the array.

`['food',  'bubble gum', 'weapon', 'AK47', 'vehicle', 'tank', 'another_vehicle', 'lexus', 'more_food', 'sirloin steak']`

Like yeah, these things are helpful but they are not really helpful in an array like that. You need them to be in a hash like this:

`{"food"=>"bubble gum", "weapon"=>"AK47", "vehicle"=>"tank", "another_vehicle"=>"lexus", "more_food"=>"sirloin steak"}`

So now you have to convert your Hash into an Array.

So you say to yourself, this is easy... and you write a while loop.

### While Loop

```ruby
def put_my_stuff_in_a_hash(array_of_stuff)
  hash = {}
  i = 0
  while i < array_of_stuff.length
    if i % 2 == 1
      hash[array_of_stuff[i]] = array_of_stuff[i + 1]      
    end
    i += 1
  end
  hash
end
```

So you do this, but you forget to increment the i, and you die of starvation. Oh Well...


Here's some of the things you could have done:

### Each with Index

```ruby
def put_my_stuff_in_a_hash(array_of_stuff)
  hash = {}
  array_of_stuff.each_with_index do |item, index|
    if index.even?
      hash[item] = array_of_stuff[index + 1]
    end
  end
  hash
end
```

This is a great way to do it, but it smells a little, because you are creating a hash in memory.

You can't have any smell because the animals are hungry and you will attract their attention.

You can Inject the items into a hash:

### Inject

```ruby
def put_my_stuff_in_a_hash(array_of_stuff)
  index = 0
  array_of_stuff.inject({}) do |obj, item|
    if index.even?
      obj[item] = array_of_stuff[index + 1]
    end
    index += 1
    obj
  end
end
```

This works by taking in an object and iterating through the array and adding the item into the object.

### Each with Object

```ruby
def put_my_stuff_in_a_hash(array_of_stuff)
  array_of_stuff.each_with_object({}).with_index do |(item,  obj), index|
    if index.even?
      obj[item] = array_of_stuff[index + 1]
    end
  end
end
```

This is similar to inject but without having to return the object after each iteration, also you can chain a `with_index` to the `each_with_object` to make it more concise.

(Thanks to Ashley and Eric for their help with this one)

### The Best Way

You are now in heaven and you decide that you would like to find the best way to solve this problem..

![Heaven](http://weknowyourdreams.com/images/heaven/heaven-03.jpg)

If only you would have just just googled how to do that..

[How to convert an array to a hash](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=how%20to%20convert%20an%20array%20into%20a%20hash)

You would get the easiest way to do it.

### Using the Hash Constructor

Your first result is Stack Overflow and you find a few interesting answers, but the shortest and most concise is this one: `Hash[*array.flatten]`. You try it and it works! But howww?

So you look up the documentation for Hash initializer, figuring that would be a good place to start.

Hmmm. You can initialize a Hash with multiple arguments and it will automatically do this for you.

All you have to do is pass the Hash an array inside of brackets:

From the [Ruby Docs](https://ruby-doc.org/core-1.8.7/Hash.html#method-c-5B-5D):

```ruby
Hash[ key, value, ... ] → new_hash

Hash["a", 100, "b", 200]   #=> {"a"=>100, "b"=>200}
```

So you can pass in any even number of values that you want, but there's still a small problem.

The `[]` initializer method of Hash is expecting a bunch of arguments and you are passing in one array. This is breaking the initializer and will not work, but you do have a way to turn that array into many arguments: The splat method!

`*`

What this will do is destructure the array and pass it into the Hash initializer as separate arguments.

Wow! Ruby is Awesome!

The moral of the story is, never use a while loop in the desert or you will surely die.
