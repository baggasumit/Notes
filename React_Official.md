# React - Official Tutorial - Tic Tac Toe

### Setting Initial State

```jsx
class Square extends React.Component {
  constructor() {
    super();
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => this.setState({value: 'X'})}>
        {this.state.value}
    //</button> // jsx markdown support :(
    );
  }
}

```


### Lifting state up

When you want to aggregate data from multiple children or to have two child components communicate with each other, move the state upwards so that it lives in the parent component. The parent can then pass the state back down to the children via props, so that the child components are always in sync with each other and with the parent.

Square no longer keeps its own state; it receives its value from its parent Board and informs its parent when it's clicked. We call components like this **controlled components**.

### Data change with and without mutation

```js
// w/ mutation
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}

// w/o mutation
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```

### Functional Components

For component types like Square that only consist of a render method. Rather than define a class extending React.Component, simply write a function that takes props and returns what should be rendered.

```jsx
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    //</button> // jsx markdown support :(
  );
}
```
