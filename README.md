# React18-Fundamentals-Codedamn

[Conditional Rendering](#Conditional-Rendering) 

[DOM Events](#dom-events)

[Stateful React Components](#stateful-react-components)

[useState Functional Argument](#usestate-functional-argument)

[Random Quote Generator](#random-quote-generator)

[useEffect Hook](#useeffect-hook)

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


# Random Quote Generator 

```jsx
import React, {useState} from "react";
import "./App.css";

function App() {
    const quotesArray = [
    'You are the shuckiest shuck faced shuck in the world!',
    'Her name badge read: Hello! My name is DIE, DEMIGOD SCUM!',
    'Insane means fewer cameras',
    'I"m about as intimidating as a butterfly.',
    'Some quotations," said Zellaby, "are greatly improved by lack of context',
    'What"s my age again?'
]
    const [quotes, setQuotes] = useState(quotesArray[0])
    function generateQuotes() {
        setQuotes(quotesArray[Math.floor(Math.random()*quotesArray.length)])
        console.log(quotes)
    }
  return (
    <div className="App">
        <h2>Random Quote Generator</h2>
        <button onClick={generateQuotes}>Click</button>
        <p>{quotes}</p>
    </div>
  );
}

export default App;
```
# useEffect Hook

useEffect hook has 2 parts:
    1. Anonymous Function
    2. Dependeancy Array
    
Syntax:
```jsx
useEffect(()=>{},[])
```

## useEffect hook with no value in dependancy array

- **useEffect** hooks run only once when no value is passed to dependancy array.

```jsx
  useEffect(()=>{
        console.log("I RAN")
    }, [])
```

- In above example, console is logged only when component is first rendered. It wont run even if the state changes.

## useEffect hook with value passed to dependancy array

```jsx
function App() {
    const [counter, setCounter] = useState(0)
    
    useEffect(()=>{
        console.log("I RAN", counter)
    }, [counter])

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

- Here useEffect hook calls on every render when the value or state passed to dependancy array changes. Here the state `counter` changes on button click, so useEffect hooks runs at that time. 

- **useEffect** hook runs after the component gets mounted. 

- Anonymous function inside useEffect is dependent on state passed inside the dependancy arary,

## use case of useEffect hook

If we have a todo list app, we want to save the order of todos on the backend. In such case we create a useEffect hook, create a state called `order` whcih defines the order of todo list. Write fetch request in useEffect hook and pass `order` state to dependancy array. So whenever state value changes useEffect hook runs on every such change. So here fetch request depends on state in dependancy array.


# Todo List App

