# React18-Fundamentals-Codedamn

[Conditional Rendering](#Conditional-Rendering) 

[DOM Events](#dom-events)

[Stateful React Components](#stateful-react-components)

[useState Functional Argument](#usestate-functional-argument)

[Random Quote Generator](#random-quote-generator)

[useEffect Hook](#useeffect-hook)

[Todo List App](#todo-list-app)

[Accordion Project SetUp](#accordion-project-setup)

[React Quiz App](#react-quiz-app)

[React Router Introduction](#react-router-introduction)

[Fetching Data Using useEffect](#fetching-data-using-useEffect)

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

## 1. Building a Todo List App

**App.jsx**

```jsx
import React, { useState } from "react";
import "./App.css";

function App() {
     const [todos, setTodos] = useState(['jissmon', 'jomon'])

    //  usestate to get current todo
     const [task, setTask] = useState('')
     function addTodos() {
      // since current todos depends on previous todo values, we append current input with previous todo values. use spread operator
         setTodos(oldTodoValues => {
             return [...oldTodoValues, task]
         })
     }
  return (
    <div className="App">
        <h1>Todo App</h1>
        <input 
            type="text" 
            value={task} 
            // onchange event to get current input value
            onChange={event => setTask(event.target.value)}    
        />
        <button onClick={addTodos}>Create Todos</button>
        <ul>
            {
                todos.map((todo) => 
                    {
                        return (
                            <li>{todo}</li>
                        )
                    }
                )
            }
        </ul>
    </div>
  );
}

export default App;
```

## 2. Running Function on Enter Key

### 2.1 Clear Input Field on Enter

- Append null or empty string to *setTask()* above the *setTodos()* state function.

- We can provide *setTask()* to empty string under *setTodos()* it also work, but since javascript is asynchronous it may might not clear the field as we xpect.

```jsx
function addTodos() {
        setTask('')
        setTodos(oldTodoValues => {
            return [...oldTodoValues, task]
        })
    }
```
### 2.2 Add todo on pressing Enter key on Input field.

Notes:

1. Button element should be type `submit`, else form
does not know what should happen when user clicks on input field.

```jsx
 <button
      type='submit'
      >
      Create Todos
  </button>
```

### 2.3 Page Reloads on clicking Enter key

Because it is the default behavious of page. To resolve this,

1. Add *onSubmit* event listener to form element.
2. Remove *onClick* event listener from button element.

```jsx
<form onSubmit={addTodos}>
  <input 
      type="text" 
      value={task} 
      onChange={event => setTask(event.target.value)}    
  />
  <button
      type='submit'
      >
      Create Todos
  </button>
</form>
```

3. Set *event.preventDefault()* method to the function that create the todos.

```jsx
function addTodos(event) {
  event.preventDefault()
  setTask('')
    setTodos(oldTodoValues => {
        return [...oldTodoValues, task]
    })
}
```

### 2.4. Complete Code

**App.jsx**

```jsx
import React, { useState } from "react";
import "./App.css";

function App() {
     const [todos, setTodos] = useState(['jissmon', 'jomon'])
     const [task, setTask] = useState('')
     function addTodos(event) {
        event.preventDefault()
        setTask('')
         setTodos(oldTodoValues => {
             return [...oldTodoValues, task]
         })
     }
  return (
    <div className="App">
        <h1>Todo App</h1>
        <form onSubmit={addTodos}>
            <input 
                type="text" 
                value={task} 
                onChange={event => setTask(event.target.value)}    
            />
            <button
                type='submit'
                >
                Create Todos
            </button>
        </form>
        <ul>
            {
                todos.map((todo) => 
                    {
                        return (
                            <li>{todo}</li>
                        )
                    }
                )
            }
        </ul>
    </div>
  );
}

export default App;
```

## 3. Delete Item

**App.jsx**

```jsx
import React, { useState } from "react";
import "./App.css";

// set a global id 
let globalID = 0

function App() {
     const [todos, setTodos] = useState([])
     const [task, setTask] = useState('')
     function addTodos(event) {
        event.preventDefault()
         setTodos(oldTodoValues => {
            setTask('')

            // return an array of objects with todo and an id.
            return [...oldTodoValues, {todo: task, id: globalID++}]
         })
         console.log(todos)
     }

     function deleteTodo(id) {
        // udpate the todos array by filtering out the todo we deleted.
        setTodos(prevTodos => 
            prevTodos.filter((todo) => todo.id !== id)
        )
     }

  return (
    <div className="App">
        <h1>Todo App</h1>
        <form onSubmit={addTodos}>
            <input 
                type="text" 
                value={task} 
                onChange={event => setTask(event.target.value)}    
            />
            <button
                type='submit'
                >
                Create Todos
            </button>
        </form>
        <ul>
            {
                todos.map((item, index) => 
                    {
                        return (
                            <div key={item.id}>
                                <li>{item.todo}</li>                      
                                // pass todo id as aargument to deletTodo() function
                                <button 
                                    onClick={()=> deleteTodo(item.id)}>
                                    Delete
                                </button>
                            </div>
                        )
                    }
                )
            }
        </ul>
    </div>
  );
}

export default App;
```

# Accordion Project SetUp

## 1. Creating UI for first question

```txt
1. There should be a main tag on the page.
2. There should be a div.container tag inside the main element
3. There should be a section.info tag inside the div.container
4. "FAQ" heading in h3 should exist in the div.container element
```


# React Quiz App

**App.jsx**

```jsx
import React, { useState } from 'react'

export default function App() {
	// Define a state variable here to track question status
	const [currentIndex, setCurrentIndex] = useState(0)

	const questions = [
		{
			questionText: 'What is the capital of France?',
			answerOptions: [
				{ answerText: 'New York', isCorrect: false },
				{ answerText: 'London', isCorrect: false },
				{ answerText: 'Paris', isCorrect: true },
				{ answerText: 'Dublin', isCorrect: false },
			],
		},
		{
			questionText: 'Who is CEO of Tesla?',
			answerOptions: [
				{ answerText: 'Jeff Bezos', isCorrect: false },
				{ answerText: 'Elon Musk', isCorrect: true },
				{ answerText: 'Bill Gates', isCorrect: false },
				{ answerText: 'Tony Stark', isCorrect: false },
			],
		},
		{
			questionText: 'The iPhone was created by which company?',
			answerOptions: [
				{ answerText: 'Apple', isCorrect: true },
				{ answerText: 'Intel', isCorrect: false },
				{ answerText: 'Amazon', isCorrect: false },
				{ answerText: 'Microsoft', isCorrect: false },
			],
		},
		{
			questionText: 'How many Harry Potter books are there?',
			answerOptions: [
				{ answerText: '1', isCorrect: false },
				{ answerText: '4', isCorrect: false },
				{ answerText: '6', isCorrect: false },
				{ answerText: '7', isCorrect: true },
			],
		},
	]

	function handleAnswerClick(isCorrectAnswer) {
		// Check if correct answer is pressed. (See the hint on the left)
        // if (questions[currentIndex].answerOptions[currentIndex].isCorrect) {
        //     setScore((prevScore)=>prevScore+1)
        // }
        const answersArray = questions[currentIndex].answerOptions
        if (isCorrectAnswer) {
            setScore((prevScore)=>prevScore+1)
        }
		if (currentIndex === questions.length - 1) {
			// quiz over
			setQuizFinished(true)
		} else {
			setCurrentIndex((value) => value + 1)
		}
	}

	const [quizFinished, setQuizFinished] = useState(false)

	// Create a state variable here [score, setScore]
    const [score, setScore] = useState(0)

	return (
		<div className="app">
			{quizFinished ? (
				/* Change this hardcoded 1 to state variable score else */
				<div className="score-section">
					You scored {score} out of {questions.length}
				</div>
			) : (
				<>
					<div className="question-section">
						<div className="question-count">
							<span>Question {currentIndex
                            +1}</span>/{questions.length}
						</div>
						<div className="question-text">
							{questions[currentIndex].questionText}
						</div>
					</div>
					<div className="answer-section">
						{questions[currentIndex].answerOptions.map((answer) => {
							// Add onClick listener to this button
							return (
								<button
									onClick={(()=>handleAnswerClick(answer.isCorrect))}
									key={answer.answerText}
								>
									{answer.answerText}
								</button>
							)
						})}
					</div>
				</>
			)}
		</div>
	)
}
```

**App.css**

```css
.App {
	text-align: center;
}

.App-logo {
	height: 40vmin;
	pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
	.App-logo {
		animation: App-logo-spin infinite 20s linear;
	}
}

.App-header {
	background-color: #282c34;
	min-height: 100vh;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	font-size: calc(10px + 2vmin);
	color: white;
}

.App-link {
	color: #61dafb;
}

@keyframes App-logo-spin {
	from {
		transform: rotate(0deg);
	}
	to {
		transform: rotate(360deg);
	}
}
```

**index.css**

```css
body {
	margin: 0;
	font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu',
		'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}
* {
	font-family: 'Verdana', cursive, sans-serif;
	color: #ffffff;
}

body {
	background-color: #7cc6fe;
	display: flex;
	justify-content: center;
	align-items: center;
	min-height: 100vh;
}

.app {
	background-color: #252d4a;
	width: 450px;
	min-height: 200px;
	height: min-content;
	border-radius: 15px;
	padding: 20px;
	box-shadow: 10px 10px 42px 0px rgba(0, 0, 0, 0.75);
	display: flex;
	justify-content: space-evenly;
}

.score-section {
	display: flex;
	font-size: 24px;
	align-items: center;
}

/* QUESTION/TIMER/LEFT SECTION */
.question-section {
	width: 100%;
	position: relative;
}

.question-count {
	margin-bottom: 20px;
}

.question-count span {
	font-size: 28px;
}

.question-text {
	margin-bottom: 12px;
}

.timer-text {
	background: rgb(230, 153, 12);
	padding: 15px;
	margin-top: 20px;
	margin-right: 20px;
	border: 5px solid rgb(255, 189, 67);
	border-radius: 15px;
	text-align: center;
}

/* ANSWERS/RIGHT SECTION */
.answer-section {
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

button {
	width: 100%;
	font-size: 16px;
	color: #ffffff;
	background-color: #252d4a;
	border-radius: 15px;
	display: flex;
	padding: 5px;
	justify-content: flex-start;
	align-items: center;
	border: 5px solid #234668;
	cursor: pointer;
}

.correct {
	background-color: #2f922f;
}

.incorrect {
	background-color: #ff3333;
}

button:hover {
	background-color: #555e7d;
}

button:focus {
	outline: none;
}

button svg {
	margin-right: 5px;
}
```

**index.jsx**

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)

// Hot Module Replacement (HMR) - Remove this snippet to remove HMR.
// Learn more: https://www.snowpack.dev/#hot-module-replacement
if (import.meta.hot) {
	import.meta.hot.accept()
}
```

# React Router Introduction

https://blog.webdevsimplified.com/2022-07/react-router/

## Dynamic Routing

**App.jsx**

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BookList />} />
  <Route path="/books/:id" element={<Book />} />
</Routes>
```

**Book.jsx**
```jsx
import { useParams } from "react-router-dom"

export function Book() {
  const { id } = useParams()

  return (
    <h1>Book {id}</h1>
  )
}
```

## Routing Priority

**App.jsx**

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BookList />} />
  <Route path="/books/:id" element={<Book />} />
  <Route path="/books/new" element={<NewBook />} />
</Routes>
```
If we have the URL /books/new which route would this match? Technically, we have two routes that match. Both /books/:id and /books/new will match since the dynamic route will just assume that new is the :id portion of the URL so React Router needs another way to determine which route to render.

In older versions of React Router whichever route was defined first would be the one that is rendered so in our case the /books/:id route would be rendered which is obviously not what we want. Luckily, version 6 of React Router changed this so now React Router will use an algorithm to determine which route is most likely the one you want. In our case we obviously want to render the /books/new route so React Router will select that route for us. The actual way this algorithm works is very similar to CSS specificity since it will try to determine which route that matches our URL is the most specific (has the least amount of dynamic elements) and it will select that route.

While we are on the topic of routing priority I also want to talk about how to create a route that matches anything.


*App.jsx**

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BookList />} />
  <Route path="/books/:id" element={<Book />} />
  <Route path="/books/new" element={<NewBook />} />
  <Route path="*" element={<NotFound />} />
</Routes>
```

A * will match anything at all which makes it perfect for things like a 404 page. A route that contains a * will also be less specific than anything else so you will never accidentally match a * route when another route would have also matched.

## Nested Routes

**App.js**
```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books">
    <Route index element={<BookList />} />
    <Route path=":id" element={<Book />} />
    <Route path="new" element={<NewBook />} />
  </Route>
  <Route path="*" element={<NotFound />} />
</Routes>
```

This nesting is pretty simple to do. All you need to do is make a parent Route that has the path prop set to the shared path for all your child Route components. Then inside the parent Route you can put all the child Route components. The only difference is that the path prop of the child Route components no longer includes the shared /books route. Also, the route for /books is replaced with a Route component that has no path prop, but instead has an index prop. All this is saying is that the path of the index Route is the same as the parent Route.

## Shared Layouts

Let's imagine that we want to render a nav section with links to each book as well the new book form from any of our book pages. To do this normally we would need to make a shared component to store this navigation and then import that into every single book related component. This is a bit of a pain, though, so React Router created its own solution to solve this problem. If you pass an element prop to a parent route it will render that component for every single child Route which means you can put a shared nav or other shared components on every child page with ease.

**App.jsx**
```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/books" element={<BooksLayout />}>
    <Route index element={<BookList />} />
    <Route path=":id" element={<Book />} />
    <Route path="new" element={<NewBook />} />
  </Route>
  <Route path="*" element={<NotFound />} />
</Routes>
```

```jsx
import { Link, Outlet } from "react-router-dom"

export function BooksLayout() {
  return (
    <>
      <nav>
        <ul>
          <li><Link to="/books/1">Book 1</Link></li>
          <li><Link to="/books/2">Book 2</Link></li>
          <li><Link to="/books/new">New Book</Link></li>
        </ul>
      </nav>

      <Outlet />
    </>
  )
}
```

The way our new code will work is whenever we match a route inside the /book parent Route it will render the BooksLayout component which contains our shared navigation. Then whichever child Route is matched will be rendered wherever the Outlet component is placed inside our layout component. The Outlet component is essentially a placeholder component that will render whatever our current page's content is. This structure is incredibly useful and makes sharing code between routes incredibly easy.

# Fetching Data Using useEffect

**App.jsx**

```jsx
const [tours, setTours] = useState([])
const [loading, setLoading] = useState(true)

async function fetchTours() { 
	const data = await fetch(url)
	const jsonData = await data.json()
	setTours(jsonData)
}

useEffect(() => {
	fetchTours()
}, [])

return tours.map((tour) => {
	return <h2>{tour.name}</h2>
})

```
