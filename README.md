# Redux

React context is good for low frequency update 

Redux is better for high frequency updates

```javascript
npm install redux
npm install react-redux
```

## 1. Create Redux store for react

```javascript
import {createStore} from 'redux';
 
const counterReducer = (state = {counter : 0}, action) => {
if (action.type === 'increment') {
    return {
        counter: state.counter +1,
    }
}
 
if (action.type ==='decrement') {
    return {
        counter: state.counter -1,
    }
}
 
return state;
 
}
 
const store = createStore(counterReducer)
 
export default store
```

## 2. Provide store

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import {Provider} from 'react-redux'
 
import './index.css';
import App from './App';
import store from './store/index'
 
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Provider store={store}><App /></Provider>);

```

## 3. Using Redux Data in React Components (get state)

```javascript
import classes from './Counter.module.css';
import {useSelector} from 'react-redux';
 
const Counter = () => {
 
  const counter = useSelector(state => state.counter);
 
  const toggleCounterHandler = () => {};
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;
```

## 4. Dispatch actions

```javascript
import classes from './Counter.module.css';
import {useSelector, useDispatch} from 'react-redux';
 
const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter);
 
  const incrementHandler = () => {
dispatch ({type:'increment'})
  }
 
  const decrementHandler = () => {
    dispatch ({type:'decrement'})
      }
 
  const toggleCounterHandler = () => {};
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div>
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;
```

## Class-based component equivalent


```javascript
class Counter extends Component {
  incrementHandler() {
    this.props.increment();
  }
 
  decrementHandler() {
    this.props.decrement();
  }
 
  toggleCounterHandler() {}
 
  render() {
    return (
      <main className={classes.counter}>
        <h1>Redux Counter</h1>
        <div className={classes.value}>{this.props.counter}</div>
        <div>
          <button onClick={this.incrementHandler.bind(this)}>Increment</button>
          <button onClick={this.decrementHandler.bind(this)}>Decrement</button>
        </div>
        <button onClick={this.toggleCounterHandler}>Toggle Counter</button>
      </main>
    );
  }
}
 
const mapStateToProps = state => {
  return {
    counter: state.counter
  };
}
 
const mapDispatchToProps = dispatch => {
  return {
    increment: () => dispatch({ type: 'increment' }),
    decrement: () => dispatch({ type: 'decrement' }),
  }
};
 
export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

## 5. Attaching Payloads to Actions


```javascript
import {createStore} from 'redux';
 
const counterReducer = (state = {counter : 0}, action) => {
if (action.type === 'increment') {
    return {
        counter: state.counter +1,
    }
}
 
if (action.type ==='increase') {
    return {
        counter: state.counter + action.amount,
    }
}
 
if (action.type ==='decrement') {
    return {
        counter: state.counter -1,
    }
}
 
return state;
 
}
 
const store = createStore(counterReducer)
 
export default store
```

```javascript
import classes from './Counter.module.css';
import {useSelector, useDispatch} from 'react-redux';
 
const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter);
 
  const incrementHandler = () => {
dispatch ({type:'increment'})
  }
 
  const decrementHandler = () => {
    dispatch ({type:'decrement'})
      }
 
const increaseHandler = () => {
  dispatch({type: 'increase', amount: 10})
}
 
  const toggleCounterHandler = () => {};
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      <div className={classes.value}>{counter}</div>
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 10</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;

```

# Work with Multiple State properties

```javascript
import {createStore} from 'redux';
 
const initialState = {counter: 0, showCounter: true};
 
const counterReducer = (state = initialState, action) => {
if (action.type === 'increment') {
    return {
        counter: state.counter +1,
        showCounter: state.showCounter
    }
}
 
if (action.type ==='increase') {
    return {
        counter: state.counter + action.amount,
        showCounter: state.showCounter
    }
}
 
if (action.type ==='decrement') {
    return {
        counter: state.counter -1,
        showCounter: state.showCounter
    }
}
 
if (action.type ==='toggle') {
    return {
        showCounter:!state.showCounter,
        counter: state.counter
    }
}
 
return state;
 
}
 
const store = createStore(counterReducer)
 
export default store
```

```javascript
import classes from './Counter.module.css';
import {useSelector, useDispatch} from 'react-redux';
 
const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter);
  const show = useSelector(state=> state.showCounter)
 
  const incrementHandler = () => {
dispatch ({type:'increment'})
  }
 
  const decrementHandler = () => {
    dispatch ({type:'decrement'})
      }
 
const increaseHandler = () => {
  dispatch({type: 'increase', amount: 10})
}
 
  const toggleCounterHandler = () => {
    dispatch({type:'toggle'})
  };
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 10</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;
```

# How to work with state correctly

Never change the state, the existing state.

Instead always override it by returning a brand new state object

# Redux Toolkit


```javascript
npm install @reduxjs/toolkit
```

## Adding state slices

```javascript
import {createStore} from 'redux';
import {createSlice} from '@reduxjs/toolkit';
const initialState = {counter: 0, showCounter: true};
 
createSlice({
    name: 'counter',
    initialState: initialState,
    reducers: {
        increment(state) {
            state.counter++;
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.amount;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
 
    }
});
```

Here, it seems we are changing the existing state but actually redux toolkit overrides it for us.

## Connect redux toolkit state

```javascript
import {createStore} from 'redux';
import {createSlice, configureStore} from '@reduxjs/toolkit';
const initialState = {counter: 0, showCounter: true};
 
const counterSlice = createSlice({
    name: 'counter',
    initialState: initialState,
    reducers: {
        increment(state) {
            state.counter++;
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.amount;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
 
    }
});
 
 
const store = configureStore({
    reducer : counterSlice.reducer
})
 
export default store
```

## Migrating Everything To Redux Toolkit

```javascript
import {createStore} from 'redux';
import {createSlice, configureStore} from '@reduxjs/toolkit';
const initialState = {counter: 0, showCounter: true};
 
const counterSlice = createSlice({
    name: 'counter',
    initialState: initialState,
    reducers: {
        increment(state) {
            state.counter++;
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.payload;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
 
    }
});
 
 
const store = configureStore({
    reducer : counterSlice.reducer
})
 
export const counterActions = counterSlice.actions;
 
export default store
```

```javascript
import classes from './Counter.module.css';
import {useSelector, useDispatch} from 'react-redux';
 
import {counterActions} from '../store/index'
 
const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter);
  const show = useSelector(state=> state.showCounter)
 
  const incrementHandler = () => {
dispatch (counterActions.increment())
  }
 
  const decrementHandler = () => {
    dispatch (counterActions.decrement())
      }
 
const increaseHandler = () => {
  dispatch(counterActions.increase(10))
}
 
  const toggleCounterHandler = () => {
    dispatch(counterActions.toggleCounter())
  };
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 10</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;
```

![image](https://user-images.githubusercontent.com/104289891/205249163-43e450e1-8293-4f37-a342-eefbbd98a05e.png)


## 1. Add State Slice

```javascript
import {createStore} from 'redux';
import {createSlice, configureStore} from '@reduxjs/toolkit';
const initialCounterState = {counter: 0, showCounter: true};
 
const counterSlice = createSlice({
    name: 'counter',
    initialState: initialCounterState,
    reducers: {
        increment(state) {
            state.counter++;
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.payload;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
 
    }
});
 
const initialAuthState = {
    isAuthenticated: false
}
 
const authSlice = createSlice({
    name: 'authentication',
    initialState: initialAuthState,
    reducers: {
        login(state) {
            state.isAuthenticated = true;
        },
        logout(state) {
            state.isAuthenticated = false
        }
    }
})
 
const store = configureStore({
    reducer : counterSlice.reducer
})
 
export const counterActions = counterSlice.actions;
 
export default store
```

## 2. Add reducer


```javascript
const store = configureStore({
    reducer : {counter: counterSlice.reducer, auth:authSlice.reducer}
})
 
export const counterActions = counterSlice.actions;
export const authActions = authSlice.actions;

```

## 3. Get state with useSelector


```javascript
import classes from './Counter.module.css';
import {useSelector, useDispatch} from 'react-redux';
 
import {counterActions} from '../store/index'
 
const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter.counter);
  const show = useSelector(state=> state.counter.showCounter)
 
  const incrementHandler = () => {
dispatch (counterActions.increment())
  }
 
  const decrementHandler = () => {
    dispatch (counterActions.decrement())
      }
 
const increaseHandler = () => {
  dispatch(counterActions.increase(10))
}
 
  const toggleCounterHandler = () => {
    dispatch(counterActions.toggleCounter())
  };
 
  return (
    <main className={classes.counter}>
      <h1>Redux Counter</h1>
      {show && <div className={classes.value}>{counter}</div>}
      <div>
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={increaseHandler}>Increase by 10</button>
        <button onClick={decrementHandler}>Decrement</button>
      </div>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </main>
  );
};
 
export default Counter;
```

```javascript
import { Fragment } from 'react';
import Counter from './components/Counter';
import Header from './components/Header';
import Auth from './components/Auth'
import {useSelector } from 'react-redux'
import UserProfile from './components/UserProfile'
 
function App() {
 
  const isAuth = useSelector(state => state.auth.isAuthenticated)
  return (
    <Fragment>
    <Header/>
    {!isAuth && <Auth/>}
    {isAuth && <UserProfile/>}
    <Counter/>
    </Fragment>
  );
}
 
export default App;
```

```javascript
import classes from './Header.module.css';
import {useSelector} from 'react-redux';
 
const Header = () => {
  const isAuth = useSelector(state => state.auth.isAuthenticated)
  return (
    <header className={classes.header}>
      <h1>Redux Auth</h1>
      {isAuth && (
        <nav>
        <ul>
          <li>
            <a href='/'>My Products</a>
          </li>
          <li>
            <a href='/'>My Sales</a>
          </li>
          <li>
            <button>Logout</button>
          </li>
        </ul>
      </nav>
 
      )}
     
    </header>
  );
};
 
export default Header;
```

## 4. Dispatch Action


```javascript
import classes from './Auth.module.css';
import { useDispatch } from 'react-redux';
import { authActions } from '../store';
 
const Auth = () => {
 
  const dispatch = useDispatch();
 
  const loginHandler = (event) => {
    event.preventDefault();
    dispatch(authActions.login())
 
  }
 
  return (
    <main className={classes.auth}>
      <section>
        <form onSubmit={loginHandler}>
          <div className={classes.control}>
            <label htmlFor='email'>Email</label>
            <input type='email' id='email' />
          </div>
          <div className={classes.control}>
            <label htmlFor='password'>Password</label>
            <input type='password' id='password' />
          </div>
          <button>Login</button>
        </form>
      </section>
    </main>
  );
};
 
export default Auth;
```

```javascript
import classes from './Header.module.css';
import {useSelector, useDispatch} from 'react-redux';
import {authActions} from '../store/index'
 
const Header = () => {
 
  const dispatch = useDispatch();
 
  const isAuth = useSelector(state => state.auth.isAuthenticated)
 
  const logoutHandler = () => {
    dispatch(authActions.logout())
  }
 
  return (
    <header className={classes.header}>
      <h1>Redux Auth</h1>
      {isAuth && (
        <nav>
        <ul>
          <li>
            <a href='/'>My Products</a>
          </li>
          <li>
            <a href='/'>My Sales</a>
          </li>
          <li>
            <button onClick={logoutHandler}>Logout</button>
          </li>
        </ul>
      </nav>
 
      )}
     
    </header>
  );
};
 
export default Header;
```

## Separate reducer and store 


```javascript
import {createSlice} from '@reduxjs/toolkit'
 
const initialCounterState = {counter: 0, showCounter: true};
 
const counterSlice = createSlice({
    name: 'counter',
    initialState: initialCounterState,
    reducers: {
        increment(state) {
            state.counter++;
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.payload;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
 
    }
});
 
export const counterActions = counterSlice.actions;
export default counterSlice.reducer
```

```javascript
import {createSlice} from '@reduxjs/toolkit'
 
const initialAuthState = {
    isAuthenticated: false
}
 
const authSlice = createSlice({
    name: 'authentication',
    initialState: initialAuthState,
    reducers: {
        login(state) {
            state.isAuthenticated = true;
        },
        logout(state) {
            state.isAuthenticated = false
        }
    }
})
 
export const authActions = authSlice.actions;
export default authSlice.reducer
```

```javascript
import {configureStore} from '@reduxjs/toolkit';
import counterReducer from './counter';
import authReducer from './auth'
 
 
const store = configureStore({
    reducer : {counter: counterReducer, auth:authReducer}
})
 
 
 
 
export default store

```
