# React-hooks
React hooks

<h2>Introduction</h2>

**React hooks should only be directly called in the top level of a function component, it doesn't work inside regular js functions, nested functions or loop**</br>


<h3>useState</h3>

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
