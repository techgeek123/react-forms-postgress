# React Refs, Keys

## Learning Competencies
At the end of this topic you should be knowing about 

- What are Refs and when to use them ?
- Adding `ref` to the DOM element and class component
- When you need refs instead of ID’s ?
- What are keys in react ?
- Implementing `ref` and `key` in application
- Learn about ReactDOM in detail

## Overview

## Refs : 
According to react docs , refs are used to get reference to a DOM(Document Object Model) node or an instance of a component in a React Application i.e. refs would return the node we are referencing

#### Adding `refs` to a DOM element: 

Our example shows how to use refs to clear the input field. clearInput function is searching for element with `ref = myInput` value, resets the state and adds focus to it after the button is clicked.

```js
App.jsx

import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {

   constructor(props) {
      super(props);
        
      this.state = {
         data: ''
      }

      this.updateState = this.updateState.bind(this);
      this.clearInput = this.clearInput.bind(this);
   };

   updateState(e) {
      this.setState({data: e.target.value});
   }

   clearInput() {
      this.setState({data: ''});
      ReactDOM.findDOMNode(this.refs.myInput).focus();
   }

   render() {
      return (
         <div>
            <input value = {this.state.data} onChange = {this.updateState} 
               ref = "myInput"></input>
            <button onClick = {this.clearInput}>CLEAR</button>
            <h4>{this.state.data}</h4>
         </div>
      );
   }
}

export default App;
```

```js
main.js

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.jsx';

ReactDOM.render(<App/>, document.getElementById('app'));
```

Once the button is clicked, the input will be cleared and focused.

The `ref` attribute takes a callback function, and the callback will be executed immediately after the component is mounted or unmounted.
For example, this code uses the `ref` callback to store a reference to a DOM node:

```js
App.jsx

import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {

   constructor(props) {
      super(props);
        
      this.state = {
         data: ''
      }

      this.updateState = this.updateState.bind(this);
      this.clearInput = this.clearInput.bind(this);
   };

   updateState(e) {
      this.setState({data: e.target.value});
   }

   clearInput() {
      this.setState({data: ''});
      this.textInput.focus();
   }

   render() {
      return (
         <div>
            <input value = {this.state.data} onChange = {this.updateState} 
               ref = {(input) => { this.textInput = input; }}></input>
            <button onClick = {this.clearInput}>CLEAR</button>
            <h4>{this.state.data}</h4>
         </div>
      );
   }
}

export default App;
```

#### Adding `refs` to Class Component

When the ref attribute is used on a custom component declared as a class, the ref callback receives the mounted instance of the component as its argument. 
For example, if we wanted to wrap the CustomTextInput above to simulate it being clicked immediately after mounting:

```js
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

Note that this only works if CustomTextInput is declared as a class:

Refs should be avoided in most cases but they can be useful when you need DOM measurements or to add methods to your components.

### When to Use Refs
There are a few good use cases for refs:

- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

## Keys: 

React keys are useful when working with dynamically created components or when your lists are altered by the users. Setting the key value will keep your components uniquely identified after the change.

Let's dynamically create Content elements with unique index (i). The map function will create three elements from our data array. Since key value needs to be unique for every element, we will assign i as a key for each created element.

App.jsx
```js
import React from 'react';

class App extends React.Component {
   constructor() {
      super();
        
      this.state = {
         data: 
         [
            {
               component: 'First...',
               id: 1
            },
                
            {
               component: 'Second...',
               id: 2
            },
                
            {
               component: 'Third...',
               id: 3
            }
         ]
      }

   }

   render() {
      return (
         <div>
            <div>
               {this.state.data.map((dynamicComponent, i) => <Content 
                  key = {i} componentData = {dynamicComponent}/>)}
            </div>
         </div>
      );
   }
}
class Content extends React.Component {
   render() {
      return (
         <div>
            <div>{this.props.componentData.component}</div>
            <div>{this.props.componentData.id}</div>
         </div>
      );
   }
}

export default App;
```
```js
main.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.jsx';

ReactDOM.render(<App/>, document.getElementById('app'));
```

If we add or remove some elements in the future or change the order of the dynamically created elements, React will use the key values to keep track of each element.

## Exploration
- [Refs in React](https://hackernoon.com/refs-in-react-all-you-need-to-know-fb9c9e2aeb81)
- [Refs and the Dom](https://reactjs.org/docs/refs-and-the-dom.html)
- [Refs vs ID's](https://www.javascriptstuff.com/use-refs-not-ids/)
- [About Keys in react](https://reactjs.org/docs/lists-and-keys.html)
