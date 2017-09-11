# Fruit Stand App Demo 

* Use React to build components: buttons, list of fruits. 
* Use Redux to manage application state. 

### Redux overview:
![Redux diagram](https://github.com/rachelmoore/ReduxNotes/blob/master/Redux1.png "Redux 1")

* When we invoke the `dispatch()` method, we are telling our store to change our application state. 
* The way we tell the store to change the state is by dispatching an action object: `dispatch( {Action} )`. 
* After we dispatch `( {Action} )` the store will use the reducer to generate the next application state. 

### Write the reducer

```javascript 
// frontend/reducer.js
import { ADD_FRUIT, CLEAR } from "./actions";

const _defaultState = {
  fruits: []
}

const reducer = (oldState = _defaultState, action) => {
  switch(action.type) {
    case ADD_FRUIT:
      return {
        fruits: [
          ...oldState.fruits,
          action.fruit
        ]
      };
    case CLEAR:
      return _defaultState;
    default:
      return oldState;
  }
}

export default reducer;
```

* To test, add this reducer to a redux store. Do this by creating a store object in store.js:
```
import { createStore } from 'redux';
import reducer from './reducer.js';

const Store = createStore(reducer);

export default Store;
```

* Then import store and add store to window in entry.js. The entry.js file now looks like this: 
```
import React from 'react';
import ReactDOM from 'react-dom';
import store from './store';

// TODO just for testing!
window.store = store;


const App = () => (
	<div>Hello, World!</div>
);

document.addEventListener("DOMContentLoaded", () => {
	ReactDOM.render(
		<App />,
		document.getElementById('root')
	);
});
```

* Finally, in the console, we can manually test that `store.dispatch()` is working: 
```
window.store
store.getState()                                            // [] (default application state)
store.dispatch({ type: "ADD_FRUIT", fruit: "Apple" })     
store.getState()                                            // ["Apple"]
store.dispatch({ type: "ADD_FRUIT", fruit: "Orange" }) 
store.getState()                                            // ["Orange"]
store.dispatch({ type: "CLEAR" }) 
store.getState()                                            // [] 
```

### Write action creators

* User action creators(functions) to create things like the fruits without having to manually type the action in. 
* In frontend/actions.js: 
``` 
export const ADD_FRUIT = "ADD_FRUIT";
export const CLEAR = "CLEAR";

export const addOrange = () => ({
  type: ADD_FRUIT,
  fruit: "Orange"
})

export const addApple = () => ({
  type: ADD_FRUIT,
  fruit: 'Apple'
});

export const clearFruit = () => ({
  type: CLEAR
});
```
* Note that constants `ADD_FRUIT` and `CLEAR` were created on the first two lines to replace the type's string version with a constant.
* Now we need to add some code to actions.js for testing purposes. Add these three lines under `window.store = store`:
```
window.addOrange = addOrange;
window.addApple = addApple;
window.clearFruit = clearFruit;
```
* It is also necessary to add `import { addOrange, addApple, clearFruit } from './actions';` under the `import store` line.
* In the console,  we can now test: 
```
store.dispatch(addApple())
store.dispatch(addOrange())
store.getState()                // ["Apple", "Orange"]
```
* We've done everything on the Redux side. Now we have to connect our Redux cycle(action creators and store) to a React component. 

### Connecting the Redux cycle with React components

* React components are made up of two layers: 




