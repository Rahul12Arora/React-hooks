# React-hooks
React hooks

<h2>Introduction</h2>

**React hooks should only be directly called in the top level of a function component, it doesn't work inside regular js functions, nested functions or loop**</br>


<h2>useState</h2>

1.)The useState hook allows us to create state variables in a React function component.</br>
2.)State allows us to access and update certain values in our components over time</br>
3.)When we create a state variable, we must provide it a default value (which can be any data type eg - like number, String or object).</br>
4.)We get the state variable as the first value in an array, which we can destructure and declare with const</br>
5.)The second value of the array is a function which we can use to change the value of our state variable, we pass the desired value as an argument to this function to update our state variable.</br>
6.)When ever our state variable changes/updates, our react UI also updates to reflect those changes.

```
function App() {
  const [counter, setcounter] = useState(0);
  function increasecounter(){
    setcounter(counter+1);
  }
  function decreasecounter(){
    setcounter(counter-1);
  }

  return (
    <div className="App">
      <h1>React counter is {counter}</h1>
      <button onClick={increasecounter}>increment</button>
      <button onClick={decreasecounter}>decrement</button>
    </div>
  );
}
```

<h2>useEffect</h2>

1.)useEffect accepts a callback function (called the 'effect' function), which will by default run every time the component re-renders.</br>
2.)It runs once when the component is mounted.
3.)useEffect lets us conditionally perform effects with the dependencies array, The dependencies array is the second argument passed to useEffect,If any one of the values in the array changes, the effect function runs again.
4.)If no values are included in the dependencies array i.e empty array, useEffect will only run on component mount and unmount.</br>

```
function App() {
  const [counter, setcounter] = useState(0);

  function increasecounter(){
    setcounter(counter+1);
  }
  function decreasecounter(){
    setcounter(counter-1);
  }

  useEffect(()=>{
    setcounter(counter+1);           //this will run on infinite loop(because it changes counter after rendering initially => componenent rerenders => useEffect runs 
  })                                 //again as it is on default mode & it refreshes the page i.e no second argument =>cycle repeats
  
  useEffect(()=>{
    setcounter(counter+1);          //this will run on infinite loop((because it changes counter after rendering initially => componenent rerenders => change is 
  },[counter])                      //detected in the counter variable from state array => this calls for useEffect execution again =>cycle repeats
  
  useEffect(()=>{
    setcounter(counter+1);          //this will run once when the component is first mounted/rendered
  },[])
  
  useEffect(()=>{
    setcounter(counter+1);                          //this will run once when the component is first mounted/rendered
    return ()=>{console.log("Goodbye component"}    //runs when component is removed from ui i.e unmounted
  },[])
  

  return (
    <div className="App">
      <h1>React counter is {counter}</h1>
      <button onClick={increasecounter}>increment</button>
      <button onClick={decreasecounter}>decrement</button>
    </div>
  );
}
```

<h2>useContext</h2>

Intro - useContext Helps Us Avoid Prop Drilling. In React, we want to avoid the following problem of creating multiple props to pass data down two or more levels from a parent component. In some cases, it is fine to pass props through multiple components, but it is redundant to pass props through components which do not need it.</br>

1.)We create & export a variable that is React.createContext();</br>
2.)We wrap our function in this variable component & assign the value attribute what we want to pass down.</br>
3.)We now import UserContext in the component we want to use the data in & pass it in useContext() Hook.</br>

```
import React from 'react';

export const UserContext = React.createContext();

export default function App() {
  return (
    <UserContext.Provider value="Reed">
      <User />
    </UserContext.Provider>
  )
}

function User() {
  const value = React.useContext(UserContext);  
    
  return <h1>{value}</h1>;
}
```

**custom example boilerplate**
**context created here**

```
import React from 'react';
import { useEffect, useState } from "react";
import Body from './Body';

export const counterData = React.createContext();
function App() {

  const [counter, setcounter] = useState(0);

  return (
    <counterData.Provider value= {[counter, setcounter]}>
    <div className="App">
      <Body></Body>
    </div>
    </counterData.Provider>
  );
}

export default App;
```

**context data used here**

```
import React, { useContext } from 'react'
import { counterData } from './App'
import './Body.css';

function Body() {
  
  const [count,setcount] = useContext(counterData);
  if(count==10) setcount(20);
  return (
    <div className='cn'>{count}</div>
  )
}

export default Body;
```

<h3>Routes</h3>

install react router in your react app
```
npm i react-router-dom
```

1.)Firstly we wrap the index component in <BroswerRouter>
  
```
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from 'react-router-dom';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```
2.)Now we set all our components in different <route> tags, that are enclosed within a single <routes> tag.</br>
Note - only the component within the <routes> tag rerenders as we select & not the whole page.</br>
3.)Path = "/" tells us that this is the root component of this entity, element atrribute takes the actual jsx or component to be displayed.</br>
4.)Path= "/mycomponent" is the path we give to that component.</br>
5.)Linking - now to redirect we use <Link to={"/mycomponent2"}> tag, it renders that particular component inside <routes> tag in that whole component.(swaps things without refreshing our entire application)</br>

```
import React from 'react';
import { useEffect, useState } from "react";
import { Route, Routes, Link } from 'react-router-dom';
import Home from './pages/Home';
import Layout from './pages/Layout';
import Blogs from './pages/Blogs';
import Contact from './pages/Contact';

export const counterData = React.createContext();
function App() {

  const [counter, setcounter] = useState(0);

  return (
    <>
    <ul>
      <li><Link to={"/"}>Home page pe le chalo</Link></li>
      <li><Link to={"/blogs"}>Blogs pe chalo</Link></li>
      <li><Link to={"/contact"}>contact pe chalo</Link></li>
    </ul>
    <Routes>
      <Route path="/" element={<Home></Home>}></Route>  
      <Route path="/blogs" element={<Blogs></Blogs>}></Route>  
      <Route path="/contact" element={<Contact></Contact>}></Route>  
    </Routes>
    </>
  );
}

export default App;

```

6.)Routing & passing url data with useParams()</br>

```
//in app component
<Route path="/blogs/:id" element={<Blogs></Blogs>}></Route> 


import React from 'react'
import { useParams } from 'react-router-dom'

function Blogs() {
    const {id} = useParams()
  return (
    <div>Blogs {id}</div>
  )
}

export default Blogs
```

<h3>Dynamic Routing</h3>

**App**

```
return (
    <>
    <ul>
      <li><Link to={"/"}>Home page pe le chalo</Link></li>
      <li><Link to={"/blogs"}>BlogList pe chalo</Link></li>
      <li><Link to={"/contact"}>contact pe chalo</Link></li>
    </ul>
    <Routes>
      {/* Path = "/" tells us that this is the root component of this entity, element atrribute takes the actual jsx or component to be displayed*/}
      <Route path="/" element={<Home></Home>}></Route>  
      <Route path="/blogs/:id" element={<Blogs></Blogs>}></Route>  
      <Route path="/blogs" element={<BlogList></BlogList>}></Route>  
      <Route path="/contact" element={<Contact></Contact>}></Route>  
    </Routes>
    </>
  );
```

**BlogList**
```
import React from 'react'
import { Route, Routes, Link } from 'react-router-dom';
function BlogList() {
  return (
    <>
    <h1>
        BookList
    </h1>
    <Link to={"/blogs/1"}>Book1</Link>
    <Link to={"/blogs/2"}>Book2</Link>
    </>
  )
}

export default BlogList
```

**Book/Blog**

```
import React from 'react'
import { useParams } from 'react-router-dom'

function Blogs() {
    const {id} = useParams()
  return (
    <>
    <div>Actual Blog {id}</div>
    </>
  )
}

export default Blogs
```

<h2>useNavigate</h2>

```
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/some-route');
  };

  return (
    <button onClick={handleClick}>
      Go to Some Route
    </button>
  );
}
```


<h2>useReducer</h2>

```
function reducer(state, action){
  return { count : state.count + 1 }
}
function App() {

  const [state, dispatch] = useReducer(reducer, {count : 0})
  function increment(){
    dispatch()
  }
  return (
    <>
    <button>-</button>
    <span>{state.count}</span>
    <button onClick={increment}>+</button>
    </>
  );
}
```

<h2>Miscallaneous</h2>
nvm - node version manager</br>
To install node version - node install 18.15.0</br>
To use another node version - node use 19.15.0</br>
To see the versions avaialble on your machine - node ls</br>

<h2>Pass Data as props</h2>

```
1.)Pass data as an object
function parent(){
const data = {
    name: "Rahul",
    age: 22
  }

  return (
    <Header person={data}></Header>
  );
}

2.) Acess in two ways

function Child(props) {

  const {name, age} = props.person;
  return (
    <>
    <div>{name}</div>
    <div>{props.person.name}</div>  // 2nd way
    </>
  )
}
```

```
1.) Parent side
return (
    <Header {...data}></Header>
  );

2.) Child side
function Child({name, age}) {
  return (
    <div>{name}</div>
  )
}
or

function Child(props){
const {name, age} = props;          //props.data is not needed because it was never sent as data in the first place
   return(
   <div>{name}</div>
   )
}
```

<h1>How to render list of components inside react return</h1>

```
<select>
{genres.map((item)=> { return <option>{item.title}</option> })}
</select>

------------------------------------------------------------------

{[1,2,3,4,5,6,7,8,9,10].map((song,i)=>{return <SongCard key={song.key} song={song} i={i}></SongCard>})}
```

<h2>useLocation</h2>

```


import { Link ,useNavigate} from "react-router-dom";
import MyOrder from "./MyOrder";

function Login() {
  const [credentials, setCredentials] = useState({ email: "", password: "" });

  const navigate=useNavigate();
  
  let loginflag=false;

    if(!json.success){
      loginflag=false;
      alert("Enter Valid Credantials")
    }
    if(json.success){
      loginflag =true;
      localStorage.setItem("userEmail",credentials.email);
      localStorage.setItem("authToken",json.authToken);
      // console.log(localStorage.getItem("authToken"));
      navigate("/" ,{state:{loginflag}});                          //yahan se bhej diya
    }

  }

  const onchange=(e)=>{
    setCredentials({...credentials,[e.target.name]:e.target.value})
  }

  return (
    <>
        <Form onSubmit={handleSubmit}>
    </>
  );
}

export default Login;
  
  
 ```

<p> where i want to use properties</p>

```
  import { useLocation } from "react-router-dom";
  
   const location = useLocation();

  const flag =location.state.loginflag;                          //yahan pe recieve kar liya; (state is sent with navigate("/" ,{state:{loginflag}});
  // flag we can use here
  
```
  


<h2>useMemo</h2>


# REDUX

```
import { createStore } from 'redux'

/**
 * This is a reducer - a function that takes a current state value and an
 * action object describing "what happened", and returns a new state value.
 * A reducer's function signature is: (state, action) => newState
 *
 * The Redux state should contain only plain JS objects, arrays, and primitives.
 * The root state value is usually an object. It's important that you should
 * not mutate the state object, but return a new object if the state changes.
 *
 * You can use any conditional logic you want in a reducer. In this example,
 * we use a switch statement, but it's not required.
 */
function counterReducer(state = { value: 0 }, action) {
  switch (action.type) {
    case 'counter/incremented':
      return { value: state.value + 1 }
    case 'counter/decremented':
      return { value: state.value - 1 }
    default:
      return state
  }
}

// Create a Redux store holding the state of your app.
// Its API is { subscribe, dispatch, getState }.
let store = createStore(counterReducer)

// You can use subscribe() to update the UI in response to state changes.
// Normally you'd use a view binding library (e.g. React Redux) rather than subscribe() directly.
// There may be additional use cases where it's helpful to subscribe as well.

store.subscribe(() => console.log(store.getState()))

// The only way to mutate the internal state is to dispatch an action.
// The actions can be serialized, logged or stored and later replayed.
store.dispatch({ type: 'counter/incremented' })
// {value: 1}
store.dispatch({ type: 'counter/incremented' })
// {value: 2}
store.dispatch({ type: 'counter/decremented' })
// {value: 1}
```

<h2>Formsubmit</h2>

```
const handleSubmit=async(e)=>{
    e.preventDefault();

    // console.log(JSON.stringify({email:credentials.email,password:credentials.password}))
    const response=await fetch(`${process.env.REACT_APP_BACK_URL}/api/login-user`,{
      method:'POST',
      headers:{
        'Content-Type':'application/json'
      },
      body:JSON.stringify({email:credentials.email,password:credentials.password})
    })
    const json =await response.json();
    console.log(json)
   
```
