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

Intro - useContext Helps Us Avoid Prop Drilling. In React, we want to avoid the following problem of creating multiple props to pass data down two or more levels from a parent component. In some cases, it is fine to pass props through multiple components, but it is redundant to pass props through components which do not need it.

1.)We create & export a variable that is React.createContext();</br>
2.)We wrap our function in this variable component & assign the value attribute what we want to pass down.</br>
3.)We now import UserContext in the component we want to use the data in & pass it in useContext() Hook.

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
