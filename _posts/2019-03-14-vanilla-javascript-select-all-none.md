---
layout: post
title: Creating a Vanilla Javascript Select All/None Checkbox
---

![select-all-none](/images/select-all-none.gif){:class="img-responsive"}
##### You see it everywhere, but how is it done?

### Motivation

This is my attempt to document the process of learning how to create a checkbox input that either selects all or deselects all of it's siblings.

I am doing this in honor of the principle of "learn publicly" which someone wrote up on github once, but can't find now. I thought it was brilliant and if someone can point me back there, I will be forever grateful.

So, this is a simple feature that is everywhere on the web. Basically, I have a long list of checkboxes in a form, and I want to give the user the option to select all, or to deselect all at once. 

Here is my initial code:


```html
<form id="headers-form"> </form>
```

```javascript
options.forEach((option, i) => {

  // create elements in document
  let p = document.createElement('p')
  let label = document.createElement('label')
  let input = document.createElement("input")
  let span = document.createElement('span')

  // set element properties
  input.id = `option-${i}`
  input.type = 'checkbox'
  span.textContent = option
  label.htmlFor = input.id

  // append elements to dom
  formGroup.append(p)
  p.appendChild(label)
  label.appendChild(input)
  label.appendChild(span)
  headersForm.appendChild(formGroup)    
});
```

So I obviously need to add another checkbox to the top of the form labelled "Select/Deselect All".

Here are my user stories:

- [ ] If all of the boxes are unchecked, the box will display as unchecked, and when a user checks it, it and all other boxes will get checked.

-  [ ] If some are checked and some are unchecked, the box will display as indeterminate and clicking on will check all other boxes and turn it to a check.

- [ ] If all boxes are checked, the box will display as checked, and unchecking it will uncheck everything else.

So I need a way to get the state of the list, let's say there are three states: all-checked, all-unchecked, and some-checked.

I can write a function that will take in the list of checkbox inputs and return the state.

I'll write my plan for the inside of the function in the comments of the function.

```javascript
// inputList: NodeList

function getCheckedState(inputList){
  // filter out only checked elements
  // if length of filtered array is equal to original array length, return 'all-checked'
  // if length of filtered array is 0 return 'all-unchecked'
  // if neither of those conditions are met, return 'some-checked'

  let filteredByChecked = inputList.filter(input => input.checked)
  return filteredByChecked.length === 0 ? 'all-unchecked' :  
    filteredByChecked.length === inputList.length ? 'all-checked' : 'some-checked'
}
```

While I'm writing this, I realized that it would be simpler to name the states, `all`, `some`, and `none`. So I will refactor it below when I'm done.

But first an explanation for the above hard to read but cooler syntax:

I nested one ternary inside another. This is the first time I've ever attempted to do this, I think it is probably a little bit harder to read, but I'm going to try it and see if I can get used to it. What happens is that if the top level condition of the ternary is met, it will return `all-unchecked`, if the condition is not met, it will run **another** ternary, which will check if the length of the filtered list matched the original list, which means all are checked, and return either `all-checked` or `some-checked` based on that condition.

![ternary-dawg](/images/ternary_dawg.jpg){:class="img-responsive"}
###### Yes, I made this meme myself, thank you very much. Back in my day, we had real memes, not like the garbage they have these days

And here is the refactored version:

```javascript
// inputList: NodeList

function getCheckedState(inputList){
  // filter out only checked elements
  // if length of filtered array is equal to original array length, return 'all-checked'
  // if length of filtered array is 0 return 'all-unchecked'
  // if neither of those conditions are met, return 'some-checked'

  let filteredByChecked = inputList.filter(input => input.checked)
  return filteredByChecked.length === 0 ? 'none' :  
    filteredByChecked.length === inputList.length ? 'all' : 'some'
}
```

Now that I have the state, I can start working on the actual checkbox and the change behavior.

I just realized that I will have to update the state every time the user click on a member checkbox.

So I have to attach a handler to the form to detect those changes and update the top one.

The checkbox will start off as unchecked obviously, because nothing is checked yet.

So the truth is, now that I think about it I don't really need to check the state of the whole list. 

I can probably just respond to checks and unchecks. The only problem with this is, how will I know if all of the boxes are checked or unchecked, if I only get the event data on that one checkbox?

So I need to check the state within the event handler to make sure that I have the right state.

Let me actually add my checkbox to the DOM and figure out where to go from there.

```javascript
let headersForm = document.getElementById("headers-form")
let formGroup = document.createElement("div")

// create my "master" checkbox here
let selectAllP = document.createElement('p')
let selectAllLabel = document.createElement("label")
let selectAllInput = document.createElement("input")
let selectAllSpan = document.createElement("span")
selectAllInput.id = "master-option"
selectAllInput.type = "checkbox"
selectAllLabel.htmlFor = selectAllInput.id
selectAllSpan.textContent = 'Select All/None'
selectAllLabel.appendChild(selectAllSpan)
formGroup.appendChild(selectAllP).appendChild(selectAllLabel).appendChild(selectAllInput)

options.forEach((option, i) => {

  // create elements in document
  let p = document.createElement('p')
  let label = document.createElement('label')
  let input = document.createElement("input")
  let span = document.createElement('span')

  // set element properties
  input.id = `option-${i}`
  input.type = 'checkbox'
  span.textContent = option
  label.htmlFor = input.id

  // append elements to dom
  formGroup.append(p)
  p.appendChild(label)
  label.appendChild(input)
  label.appendChild(span)
  // add listener here

  headersForm.appendChild(formGroup)    
});
```
If I need to do this one more time, I'm going to have to refactor to a higher abstraction, but for now it's okay.

Okay, now that I have the checkbox rendering, I can add a change event handler to all of the checkboxes.

Here are my user stories:

- [ ] If all of the boxes are unchecked, the box will display as unchecked, and when a user checks it, it and all other boxes will get checked.

- [ ] If some are checked and some are unchecked, the box will display as indeterminate and clicking on will check all other boxes and turn it to a check.

- [ ] If all boxes are checked, the box will display as checked, and unchecking it will uncheck everything else.

So, if a user clicks on the master checkbox, I will change the status of all the checkboxes to `checked`

I think I want to use an `onChange` listener onto the form, then I'll detect wether it's the master, or a different one.

So I will just add the listener right before I add the submit listener.

I will plan it in the comments.

```javascript
function outputForm(){
  //...
  headersForm.addEventListener("change", changeHandler)
}

function changeHandler(){
  // get all the input children to iterate over later
  // detect if currentTarget is master
    // if it is, check if it's status is checked or unchecked
      // if it's unchecked (or indeterminate) change all checked statuses to true
      // if it's checked, change everything to false
    // if currentTarget is not master, then it's a member
      // check if it's checked or not
        // if it's unchecked
          // check it, then 
          // Check the state of the list
          // If it's "some", change the master's status to indeterminate
          // If it's "all", change the master to checked
        // if it's checked
          // uncheck it
          // then check the state of the list
          // if it's "none", uncheck the master
          // if it's some, master is indeterminate 
}
```

So that's the plan. Very conditionally nesty. I need to just implement it one step at a time.

I'm going to write it as simple nested ifs, then I'll try to come up with something more elegant.

Maybe I can just write a function that will set the master just based on the state and nothing else.

Because ultimately I'm going to have to check the state anyways.

Let me try it both ways.

Getting the input nodes should be pretty easy:

```javascript
// get all the input children to iterate over later
let checkboxes = e.currentTarget.querySelectorAll('input')
```

Okay, so I mixed up currentTarget and target in my comments up there.

The currentTarget refers to where the handler has been attached, as opposed to where the event happened which is the target. Per the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget).

I should also get a reference to the master to make changes to it specifically as well.

```javascript
let master = e.currentTarget.getElementById("master-option")
```

Now my code is looking like this:

I'm going to change the places where I wrote currentTarget to target.

```javascript
function changeHandler(){
  // get all the input children to iterate over later
  let checkboxes = e.currentTarget.querySelectorAll('input')
  let master = e.currentTarget.getElementById("master-option")
  // detect if target is master
    // if it is, check if it's status is checked or unchecked
      // if it's unchecked (or indeterminate) change all checked statuses to true
      // if it's checked, change everything to false
    // if target is not master, then it's a member
      // check if it's checked or not
        // if it's unchecked
          // check it, then 
          // Check the state of the list
          // If it's "some", change the master's status to indeterminate
          // If it's "all", change the master to checked
        // if it's checked
          // uncheck it
          // then check the state of the list
          // if it's "none", uncheck the master
          // if it's some, master is indeterminate 
}
```
So next I'm going to check if it's master by checking if the id matches the id I set for the master.

```javascript
if(e.target.id = "master-option"){}
```
Next I have to see if it's checked. Now I ran into a little unexpected behavior here. Maybe you can figure out why before I reveal it.

When I was debugging the code in devtools, the `checked` attribute was the opposite of what I expected. When I clicked on an unchecked box, the `checked` attribute said `true` and vice versa. 

In retrospect the issue is obvious (it always is). The handler runs after I change the status of the input, therefore when the handler is being called, the status of my `checked` attribute is `true`, because that's what I changed it to. Super Obvious.

So maybe I should restructure my plan a little bit:

```javascript
function changeHandler(e){
    // get all the input children to iterate over later
    const checkboxes = e.currentTarget.querySelectorAll('input')
    const master = e.currentTarget.getElementById("master-option")
    // detect if target is master
    if(e.target.id = "master-option"){
      // if it is, check if its status is checked or unchecked
        // if it's checked now, change everything to true
        // if it's unchecked now, change all checked statuses to false
      }
      // if currentTarget is not master, then it's a member
        // check if it's checked or not
          // if it's checked
            // Check the state of the list
            // If it's "some", change the master's status to indeterminate = true, checked = false
            // If it's "all", change the master to checked = true, indeterminate = false
          // if it's unchecked
            // then check the state of the list
            // if it's "none", masters checked = false, indeterminate = false
            // if it's "some", master  indeterminate = false, checked = false
  }
```
About the indeterminate thing, [it won't change the checked status](https://css-tricks.com/indeterminate-checkboxes/). 

So I need to figure out how to deal with that. I still want to have it so I can show the user that some are selected.

Let me rework my plan.

Essentially, the only time I want to have a checked status on the master is when everything is checked, the indeterminate will "mask" an unchecked state.

Let me first implement the behavior for when I click on the master.

Then I'll work on changing that.

```javascript
const checkAll = checkboxes => checkboxes.forEach(cb => cb.checked = true)
```

I realized that I had a bug in my code.

```javascript
const master = e.currentTarget.getElementById("master-option")
```

There is no `getElementById` on `currentTarget` which returns a node. The only only thing you can call `getElementById` on is `document`. Lesson learned.
Since there is only one unique id per page, that's the only node that makes sense to search from.

This fixed it:

```javascript
const master = document.getElementById("master-option")
```

My next step is to deal with the deselection of all when they are all selected. Let me start when master is checked, I will just do the reverse of what I just did before.

```javascript
const uncheckAll = checkboxes => checkboxes.forEach(cb => cb.checked = false)
```

And I will run these inside the conditionals:

```javascript
if(e.target.id === "master-option"){
  // if it is, check if its status is checked or unchecked
  const targetChecked = e.target.checked
  if(targetChecked){
    // if it's checked now, change everything to true
    checkAll(checkboxes)
  } else {
    // if it's unchecked now, change all checked statuses to false
    uncheckAll(checkboxes)
  }
}
```

Moving along!

The only thing that's left is updating the status of the master when all of the boxes are checked or unchecked from a member, and setting indeterminate when some are checked.

It's really just polish, but let's git r done!

Here are my user stories:

- [x] If all of the boxes are unchecked, the box will display as unchecked, and when a user checks it, it and all other boxes will get checked.

- [ ] If some are checked and some are unchecked, the box will display as indeterminate and clicking on will check all other boxes and turn it to a check.

- [x] If all boxes are checked, the box will display as checked, and unchecking it will uncheck everything else.

Okay, so I implemented all of the nested conditionals, now my code looks like this:

```javascript
function changeHandler(e){
    // get all the input children to iterate over later
    const checkboxes = e.currentTarget.querySelectorAll('input')
    const master = document.getElementById("master-option")
    // detect if target is master
    const targetChecked = e.target.checked
    if(e.target.id === "master-option"){
      // if it is, check if its status is checked or unchecked
        if(targetChecked){
          // if it's checked now, change everything to true
          checkAll(checkboxes)
        } else {
          // if it's unchecked now, change all checked statuses to false
          uncheckAll(checkboxes)
        }
      } else {
        // if currentTarget is not master, then it's a member
        // if it's checked
        let listState = getCheckedState(checkboxes)
        if(targetChecked){
          // Check the state of the list
          if(listStatus === "some"){
            // If it's "some", change the master's status to indeterminate = true, checked = false
            master.checked = false
            master.indeterminate = true
          } else {
            // If it's "all" (the only other option), change the master to checked = true, indeterminate = false
            master.checked = true
            master.indeterminate = false 
          }
        } else {
          //  if it's unchecked
          // then check the state of the list
          if(listSatus === "none"){
            // if it's "none", masters checked = false, indeterminate = false
            master.checked = false
            master.indeterminate = false
          } else {
            // if it's "some", master  indeterminate = true, checked = false
            master.indeterminate = true
            master.checked = false
          }

        }
      }
    }
```

I tried running it, and I got an error in my `getCheckedState` function.

It said that `inputList` does not have a function `.filter`. That's because it's nodelist, and I have to convert it to an array. A quick search brought me to [this SO page](https://stackoverflow.com/questions/32765157/filter-or-map-nodelists-in-es6) which gives a very simple way. I used the first one. It now looks like this:

```javascript
function getCheckedState(inputList){
  let filteredByChecked = [...inputList].filter(input => input.checked)
  return filteredByChecked.length === 0 ? 'none' :  
    filteredByChecked.length === inputList.length ? 'all' : 'some'
}
```

Aaand, now I have a syntax error. `listSatus` is not defined. Oh really? Maybe it's because I don't want to define it. Maybe it's because I think `listSatus` is unique and doesn't deserve to be labelled by society. So what?

Ahh, but what's this? I defined it, and now I'm getting **`listStatus`** is not defined. These guys are really independent minded, sheesh.

Because I called one state and one status.

I'm just going to call all of them state.

So my final code looks like this:

```javascript
function changeHandler(e){
    // get all the input children to iterate over later
    const checkboxes = e.currentTarget.querySelectorAll('input')
    const master = document.getElementById("master-option")
    // detect if target is master
    const targetChecked = e.target.checked
    if(e.target.id === "master-option"){
      // if it is, check if its status is checked or unchecked
        if(targetChecked){
          // if it's checked now, change everything to true
          checkAll(checkboxes)
        } else {
          // if it's unchecked now, change all checked statuses to false
          uncheckAll(checkboxes)
        }
      } else {
        // if currentTarget is not master, then it's a member
        // if it's checked
        const listState = getCheckedState(checkboxes)
        if(targetChecked){
          // Check the state of the list
          if(listState === "some"){
            // If it's "some", change the master's status to indeterminate = true, checked = false
            master.checked = false
            master.indeterminate = true
          } else {
            // If it's "all" (the only other option), change the master to checked = true, indeterminate = false
            master.checked = true
            master.indeterminate = false 
          }
        } else {
          //  if it's unchecked
          // then check the state of the list
          if(listState === "none"){
            // if it's "none", masters checked = false, indeterminate = false
            master.checked = false
            master.indeterminate = false
          } else {
            // if it's "some", master  indeterminate = true, checked = false
            master.indeterminate = true
            master.checked = false
          }

        }
      }
    }
```

Yes! It works perfectly. Try it out yourself. Wait, are you still here? Hello? Where did you go? You're not following along? Whatever dude. It works, just trust me.

Here are my user stories:

- [x] If all of the boxes are unchecked, the box will display as unchecked, and when a user checks it, it and all other boxes will get checked.

- [x] If some are checked and some are unchecked, the box will display as indeterminate and clicking on will check all other boxes and turn it to a check.

- [x] If all boxes are checked, the box will display as checked, and unchecking it will uncheck everything else.

If you want the full code on github you can find it [over here](https://github.com/joesasson/needless/blob/37338f9f8b445b304e0c32a07722f302860dc817/src/other/clientJs.html). It's part of a google sheets addon that's part of a larger project that I'm working on. 

To summarize some of the things I've learned from this:
  - Nested ternaries can be a good option for a simple nested conditional
  - NodeLists need to be converted to an array before being able to iterate with `map`, `filter`, etc.
  - currentTarget is the node where the handler is attached, target is where the event originated
  - indeterminate state of a checkbox is only stylistic
  - The handler runs after the input's state is changed, duh
  - `getElementById` can only be called from `document`

So while trying to record a video for this article, I realized that I'm actually not done. Instead of the master getting checked when the status changes to "all" from a member, it's just staying indeterminate.

I ran the debugger, and the `listState` is returning "some". But whhhhyyy?

I got the bug!!

The problem is that I'm checking the filtered list, but that doesn't include the master, which is now unchecked. It's like a chicken and egg problem.

So what are my options here? 

If I change the comparison to include only the members, that would probably work. 

Let me try that.

I need to exclude the master from all of the checks, on both sides of the equation.

I will just add a filter to the end of the chain to remove it.

```javascript
let filteredByChecked = [...inputList].filter(input => input.checked).filter(input => input.id !== "master-option")
```

Still not working.

Okay, so I realized that the `inputList` will never equal the `filteredByChecked` because it will always be one less, even if everything is checked.

So I need to change the ternary:

```javascript
function getCheckedState(inputList){
  let filteredByChecked = [...inputList].filter(input => input.checked).filter(input => input.id !== "master-option")
  return filteredByChecked.length === 0 ? 'none' :  
    filteredByChecked.length === inputList.length - 1 ? 'all' : 'some'
}
```

And now everything works.

Well, I hope you enjoyed this journey with me. I will try to continue to put these out, because it was a very helpful process for me, and it really helped clarify my thought process. If you liked it and want more, or if you have feedback (or if you catch a mistake somewhere), please let me know in the comments below.

Thank you for reading!

