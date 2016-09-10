# redux-async-actions

A simple interface for working with asynchronous actions in Redux.

## Motivation
Dispatching a function instead of an object like in [redux-thunk](https://github.com/gaearon/redux-thunk) feels weird and can make debugging difficult. This is an alternative, yet still simple, interface for dispatching actions asynchronously in Redux. The goal is to provide a more intuitive interface as well as simplify debugging by only ever dispatching plain objects.

## Installation

```bash
npm install --save redux-async-actions
```

Then apply the `asyncActionsMiddleware` using [`applyMiddleware()`](http://redux.js.org/docs/api/applyMiddleware.html):
```js
import { createStore, applyMiddleware } from 'redux';
import { asyncActionsMiddleware } from 'redux-async-actions';
...
const store = createStore(
  rootReducer,
  applyMiddleware(asyncActionsMiddleware)
);

```

## Usage

###`createAsyncAction(?startAction, asyncFunc)`

Creating an async action is simple. Simply pass an asynchronous function to `createAsyncAction`:
```js
import { createAsyncAction } from 'redux-async-actions';
...
createAsyncAction((dispatch, getState) => {
  setTimeout(() => {
    dispatch(someActionCreator());
  }, 1000);
});
```

Since many asynchronous use cases require dispatching a 'starting action', `createAsyncAction` allows you to specify one of these upfront, instead of dispatching an action first thing in your asyncFunc. This `startAction` is dispatched in the same dispatch loop as the async action, saving the extra dispatch call.
```js
import { createAsyncAction } from 'redux-async-actions';
...
createAsyncAction(someStartActionCreator(), (dispatch, getState) => {
  setTimeout(() => {
    dispatch(someCompletionActionCreator());
  }, 1000);
});
```
