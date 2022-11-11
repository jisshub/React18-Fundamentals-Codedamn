# React18-Fundamentals-Codedamn

[Conditional Rendering](#Conditional-Rendering) 

[DOM Events](#dom-events)

[Stateful React Components](#stateful-react-components)

[useState Functional Argument](#usestate-functional-argument)

# Conditional Rendering

**App.jsx**

- Using ternary operator and AND operator to render component conditionaly.

```jsx
import React from "react";
import "./App.css";

console.log("jissmon" && "jose" && undefined)

function App() {
    const score = 45
    const presentOrNot = true
    const skills = ['react', 'node', 'javascript']
  return (
    <div className="App">
        <h1>hello from app</h1>
        {score > 40 && <p>You are qualified</p>}
        {presentOrNot && <p>Student is present today</p>}
        {skills.includes('react') ? <p>shortlist the candidate</p>: null}
    </div>
  );
}

export default App;
```

# DOM Events

```jsx
const handleClick =()=>{
        console.log('Clicked')
}

<button onClick={handleClick}>Click Me</button>
```

# Stateful React Components

 - React considers anything starting with `use` as hook.
 - useState hook
 - State variables cannot be updated directly, it can be updated with their corresponding function or setters.


```jsx
function App() {
    const [counter, setCounter] = useState(100) # [100, function]
    const increaseCounterFn = () => {
        setCounter(250) # [250, function]
    }
    function decreaseCounterFn(){
        setCounter(50) # [50, function]
    }
  return (
    <div className="App">
      <h2>Counter: {counter}</h2>
      <button onClick={increaseCounterFn}>Increase the counter</button>
      <button onClick={decreaseCounterFn}>Decrease the counter</button>
    </div>
  );
}
```

 - When setCounter() functin is called App component re-renders with new value for state counter.


# useState Functional Argument

 - Updating the counter with setter function.

## Two ways:


### Variable Assignment Approach

```jsx
function App() {
    const [counter, setCounter] = useState(0)
    const increaseCounterFn = () => {
        setCounter(counter + 1)
    }
    function decreaseCounterFn(){
        setCounter(counter - 1)
    }
  return (
    <div className="App">
      <h2>Counter: {counter}</h2>
      <button onClick={increaseCounterFn}>Increase the counter</button>
      <button onClick={decreaseCounterFn}>Decrease the counter</button>
    </div>
  );
}
```

### Functional Approach

```jsx
function App() {
    const [counter, setCounter] = useState(0)
    const increaseCounterFn = () => {
        setCounter(olderCounterVal => olderCounterVal + 1)
    }
    function decreaseCounterFn(){
        setCounter(olderCounterVal => olderCounterVal - 1)
    }
  return (
    <div className="App">
      <h2>Counter: {counter}</h2>
      <button onClick={increaseCounterFn}>Increase the counter</button>
      <button onClick={decreaseCounterFn}>Decrease the counter</button>
    </div>
  );
}
```


**Note:**

 1. As long as newer value depndes on older value, use functional update approach instead of variable approach.
 2. If newer values doesnt depends on older one, use variable approach.



