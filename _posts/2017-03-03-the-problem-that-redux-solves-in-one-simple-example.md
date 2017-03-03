---
layout: post
title: The problem that Redux solves in one simple example
---

After recently learning about Redux and React in The Flatiron School (which I just graduated from), I decided I would build my own app called [anjou](https://joesasson.github.io/anjou/) using those libraries and principles.

The app has a few very basic capabilities(for now):
  - Take in a name and add it to a list
  - Randomly pair up two names on the list

## Why React?

Obviously, at this level I could have written the app in vanilla javascript and got it working fairly easily. But I chose to write it in React because I know that as I start adding more and more features and capabilities and more components to the app, it will take me more and more time to keep everything interacting properly.

React makes it easy to create reusable components as opposed to copypasta everywhere. It does it with a mixture of different concepts and techniques which also adds the performance factor of the virtual DOM and being able to write JSX which is a lot easier than writing out HTML elements by hand the jquery way.

The main benefit of React to me is the way it allows you to keep different components apart and have them handle their own responsibilities and then when you need to reuse them, you don't need to maintain them in two places.

This was pretty simple to me, even though I realize now that I could have written another whole blog post on that. Maybe I should do that?

## The Code

If you want to take a look at the code, you can check the repo [here](https://github.com/joesasson/anjou).

To give a basic overview of the app as it currently stands, here is my components directory:

```bash
-- /components
---- AppHeader.jsx
---- NameContainer.jsx
---- NameInput.jsx
---- NameList.jsx
---- PairContainer.jsx
---- PairItem.jsx
```

Without getting into the redux implementation, let's take a look at the React components that we are concerned with (actual code may vary, you can visit the [github repo](https://github.com/joesasson/anjou) to see it):

```js
class NameInput extends Component {

  render(){
    return (
      <form onSubmit={this.handleAddNameSubmit.bind(this)} >
        <input ref='name' placeholder='Enter a name' required/>
        <button type="submit">Add Name</button>
      </form>
    )
  }
}

class NameList extends Component {

  generatePairs(){
    let names = this.props.names
    let shuffledNames = this.shuffle(names)
    var newArray = []
    shuffledNames.forEach((el, i) => {
      if(i % 2 !== 0){
        newArray.push([el, shuffledNames[i - 1]])
      } else if (i === shuffledNames.length - 1){
        newArray.push([el, "No Partner Yet"])
      }
    })
    this.props.setPairs(newArray)
  }

  render(){
    return (
      <div>
        <ul>
          {this.props.names.map((name, i) => <li key={`name-${i}`}>{name}</li>)}
        </ul>
        <button onClick={this.generatePairs.bind(this)} >Generate Pairs</button>
      </div>
    )
  }
}

class PairContainer extends Component {

  render(){
    return (
      <ul>
        {this.props.pairs.map((pair, i) => {
          return <PairItem pair={pair} />
        })}
      </ul>
    )
  }
}
```

## The Problem

Ok great, we have components for a name input, a name list, and after the name list generates the pairs which are then displayed in the pairs container. But here's the problem(s):

- How does NameInput send the name into the NameList?
- How does NameList have access to the names?
- How does PairContainer know what the pairs are?

Essentially, *how do my components communicate with each other?*

## A Basic Solution

We can easily envision a solution where a higher level component, something named NameContainer or NameApp would hold the state of the data in it's local state and we can have out lower level components inherit callbacks and data from it, like this:

```js
export default class NameContainer extends Component {

  constructor(){
    this.state = {
      names: []
    }
  }

  changeState(event){
    this.setState({
      names: [...this.state.names, event.target.value]
    })
  }

  render(){
    return (
      <div>
        <NameInput handleSubmit={this.changeState}/>
        <NameList names={this.state.names} />
      </div>
    )
  }
}
```

*Note: The above is not actually implemented code, just a possible way to get the components to talk to each other.*

This can work for a few components and in this case it is probably the best solution. Buuuuuuuut....

## The Problem With The Solution

What if we have a whole bunch of components that make similar changes to this higher lever component?

Also, what if we wanted to change a different part of the state, like the pairs?

What's going to happen in that the state will no longer be coherent because each component doesn't really understand what the other components are doing and therefore are not aware of what changed since they decided that they wanted to change it.

## The Solution

Redux offers a single container for our entire App's state which ensures that we don't have to deal with the mess that I described above.

From the words of the [documentation](http://redux.js.org/):

> Redux is a predictable state container for JavaScript apps.

It is beyond the scope of this article to explain how Redux implements this solution and all of the particular implementations of functional programming and other cool things that are going on in the background. We just need to trust it to do it's job and understand why we are using it.

With Redux we initialize a store for our entire app, this keeps state and reducers which modify that state in a very specific way, making sure that we can keep track of every change.

A big thanks to [Dan Abramov](https://twitter.com/dan_abramov) for putting so much time and energy into this project and making it so easy to learn and understand.

Thanks for reading! If I missed something or made any mistakes, please let me know in the comments section below.

Links for Reference:

- [http://redux.js.org/docs/introduction/Motivation.html](Redux - Motivation)
