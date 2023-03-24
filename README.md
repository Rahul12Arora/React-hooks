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
4.)If no values are included in the dependencies array, useEffect will only run on component mount and unmount.</br>
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


