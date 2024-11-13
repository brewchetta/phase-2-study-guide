// Phase 2 Assessment Study Guide

// 1. What is declarative vs. imperative programming? //

// imperative is when you go step by step
// declarative is when you tell it what you want to happen and it figures out the steps
// vanilla js and the DOM is imperative
// react is declarative (uses jsx to put elements on the page)

// 2. What is the virtual DOM? //

```
function GreetingHeader() {
    const [greeting, setGreeting] = useState('Hello')
    
    const handleChangeGreeting = () => setGreeting('Hola')
    
    return (
        <div>
            <h1 onClick={handleChangeGreeting}>{greeting}</h1>
        </div>
    )
}
```

// VIRTUAL DOM - our little translator blackbox - the real time processor that helps us change what is rendered (what the user the sees)
```
<div>
    <h1>Hola</h1>
</div>
```

// and adds it to the real DOM

// REAL DOM
```
<div>
    <h1>Hola</h1>
</div>
```

// 3. What is a component? What does it mean that we can make a component tree? //

// a component returns JSX
// it can take in props --> a property is passed down from a parent to a child for that child to use
// components need to start with a capital letter
// components do not need to have their own file but a lot of times they do
// a portion of our entire JSX code that allows us to make a more readable project instead of one large file / component
// parent child relationships that determine the hierarchy and which components can get props from which ones


// 4. What are props and how are they passed and received? What is destructuring? //

// a component can take in props --> a property is passed down from a parent to a child for that child to use
// similar to an argument in a function

```
function Parent() {
    const someProp = "I am a prop"

    return (
        <Child someProp={someProp} secondProp={'second prop'} /> {/* SENDING THE PROP */}
    )
}

function Child({ someProp, secondProp }) { /* RECEIVING THE PROP */
    // const someProp = props.someProp
    // const secondProp = props.secondProp
    
    return (
        <p>{someProp}</p>
    )
}
```

```
// props = { someProp: 'I am a prop', secondProp: 'second prop' }
function Child(props) { /* RECEIVING THE PROP */

    // PROPS IS AN PLAIN OLD JS OBJECT

    return (
        <p>{props.someProp}</p>
    )
}
```


// 5. What is state? Why do we use it? What two things does `useState` create? //

// useState is a hook that creates the GETTER VARIABLE and the SETTER FUNCTION
`const [hamburger, setHamburger] = useState('initial value')`
// useState takes in an argument for the initial value

// VERY BIG TOOL FOR REACT DECLARATIVE PROGRAMMING
// change the values triggers a RERENDER <----- THIS IS REALLY IMPORTANT
```
setHamburger('bbq burger')
hamburger => 'bbq burger'
```

// the SETTER can take in a new value OR it can take in a function
```
setHamburger('bbq burger') //>>> changes the value to 'bbq burger'
setHamburger(previousHamburgerState => previousHamburgerState.toUpperCase())
```
// once the setter triggers the getter becomes the value returned by the setter's callback function

setArray(previousArray => [...previousArray, 'cheeseburger'])
// making a new array with the spread operator (clones the previous array) and adds a new value at the end
// BUT WHY????
// useState has to rerender and we need a NEW ARRAY to trigger the rerender
// don't mutate state, instead copy state and put it in the setState function


// 6. What is a side effect? What is `useEffect` used for? What is a dependency array? //

// useEffect --> triggers on certain rerenders
// often used to stop loops with useState from happening (for example fetching data)

// useful for managing any activities outside the scope of the component function
// can be helpful for managing effects / events that happen as part of a rerender (or initial render)

```
useEffect(() => {
    const response = await fetch('localhost:3000')
    const data = await response.json()
    setState(data)
}, [])

useEffect(() => {
    console.log("HELLO!")
}, [hamburger]) // THIS USEEFFECT WILL TRIGGER ON THE FIRST RENDER AND EVERY TIME HAMBURGER CHANGES
```


// 7. What is a controlled form (or controlled component)? Why do we build controlled forms / components? //

// the component "puppets" the form
// this is nice because the value of state is easily accessible 
// and we can do things like make POST requests with it

// when controlling...
// add a use state for the input
// add an onChange function for the input to use the SETTER
// value is the state GETTER

```
function ControlledForm() {

    const [hamburger, setHamburger] = useState('') // tied to the form input

    const handleChangeHamburger = event => setHamburger(event.target.value)

    return (
        <form>
            <input type="text" onChange={handleChangeHamburger} value={hamburger} />
            <input type="submit" value="Submit" />
        </form>
    )

}
```


// 8. How do we create routes with react-router-dom? What is `<Outlet />` used for? What are route params? What does `useNavigate` allow us to do? //

// routes is an array of objects --> each object is a path & element
```
const routes = [
    {
        path: '/',
        element: <App />,
        children: [ // an array of sub-objects they will specifically appear where the <Outlet /> is
           {
                path: 'about',
                element: <About />
            },
            {
                path: 'hamburger/:id',
                element: <Lunch />
            }
        ]
    },
    {
        path: '/login',
        element: <Login />
    }
]

function App() {

    return (
        <div>
            <h1>HAMBURGERS ARE COOL</h1>
            <Outlet />
            {/* the components that are sub-routes for App ('/') will appear where the Outlet is */}
        </div>
    )
}

function Lunch() {

    const params = useParams()
    const navigate = useNavigate() // a tool to programmatically navigate to different routes

    const goToHome = () => navigate('/') // this triggers the application to go to the '/' route

    return (
        <p onClick={goToHome}>MY ID IS {params.id}</p>
    )

}
```

// example of useNavigate is using it to navigate away from a page after successfully submitting a form on that page
