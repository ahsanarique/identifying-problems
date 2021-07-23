# Given the code defined below, can you identify two problems?

```js
class MyComponent extends React.Component {
  constructor(props) {
    // set the default internal state
    this.state = {
      clicks: 0,
    };
  }
  componentDidMount() {
    this.refs.myComponentDiv.addEventListener("click", this.clickHandler);
  }
  componentWillUnmount() {
    this.refs.myComponentDiv.removeEventListener("click", this.clickHandler);
  }
  clickHandler() {
    this.setState({
      clicks: this.clicks + 1,
    });
  }
  render() {
    let children = this.props.children;
    return (
      <div className="my-component" ref="myComponentDiv">
        <h2>My Component ({this.state.clicks} clicks})</h2>
        <h3>{this.props.headerText}</h3>
        {children}
      </div>
    );
  }
}
```

## Ans:

### Problem 01:

The constructor in react class component refers to a method used to initialize an object's state in class. This method is called before the component is mounted. Which is why, when implementing a constructor in react class component, it is required to call `super(props)` before any other statement. Else, `this.props` will be `undefined` in constructor.

### Problem 02:

In Javascript, method in classes are not bound to a class by default. In the above code, `this.clickHandler()` will return `undefined` because it is not bound to the corresponding `constructor` method.

### Problem 03:

To get access to the state value, we need to use `this.state.clicks`. But in the `clickHandler` method, it is written as `this.clicks` which will return `undefined`. So, it should be `this.state.clicks`.

### Problem 04:

To start using `ref`, we need to initialize it first by using `React.createRef()` inside the `constructor`;

### Problem 05:

Inside `render -> return`, this line: `<h2>My Component ({this.state.clicks} clicks})</h2>` includes an unnecessary curly braces. It would not exactly break the code, but it seems like a typo.

### Improvement?

Unless we are adding multiple events to an element, `onClick` is a simpler option than adding click event listeners through life cycle methods and ref.

### Solution:

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    // set the default internal state
    this.state = {
      clicks: 0,
    };
    this.refs = React.createRef();
    this.clickHandler = this.clickHandler.bind(this);
  }

  componentDidMount() {
    this.refs.myComponentDiv.addEventListener("click", this.clickHandler);
  }

  componentWillUnmount() {
    this.refs.myComponentDiv.removeEventListener("click", this.clickHandler);
  }

  clickHandler() {
    this.setState({
      clicks: this.state.clicks + 1,
    });
  }

  render() {
    let children = this.props.children;
    return (
      <div className="my-component" ref="myComponentDiv">
        <h2>My Component ({this.state.clicks} clicks)</h2>
        <h3>{this.props.headerText}</h3>
        {children}
      </div>
    );
  }
}
```
