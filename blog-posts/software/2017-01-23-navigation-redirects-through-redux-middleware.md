# Navigation Redirects Through Redux Middleware

### Client-Side Routing
Routing in client-side applications usually starts off as a small number of routes with a few redirects or custom code here and there. But as soon as the application starts to grow and the number of features on it increases, routing commonly becomes a hard to maintain and complex domain. Ever more interactive client-side applications require an intuitive and consistent routing solution to provide a better UX, and we as developers must make sure we use equally intuitive and consistent design paradigms to deal with complex routing requirements.

The goal of this blog post is to explain how to use [Redux](https://github.com/reactjs/redux) middleware to create a solution for client-side navigation redirects which is intuitive, scalable, and easy to maintain.

### A Redux Walk-through
[Redux is a predictable state container for JavaScript applications](http://redux.js.org/). It offers a simple solution for storing and handling state changes. Redux can be used with any view library, though it's very commonly used with [React](https://facebook.github.io/react/).

In Redux, the whole application state is stored in a single place: the ``store``. The state of the application can only be modified through an ``action``. When an action is dispatched, it's handled by a ``reducer`` which proceeds to update the store as needed.

A Redux ``middleware`` provides and extension to the Redux work flow. It allows the developer to capture an action and execute some custom code before or after it's dispatched.

### Redux Middleware In Action
Let's now assume we are working on a simple TODO application using React and Redux. The TODO application allows a user to see all TODO's at ``/`` (the index route) and click a single TODO at ``/:todoId`` to view its details.  We want to add a new feature to our TODO application: give the user the ability to delete a single TODO when viewing its details at ``/:todoId``. If the TODO is successfully deleted, the user should be redirected to the index page.

In  order to delete a TODO, we create a couple of new actions in the application:

```
TODO__DELETE__INIT // Dispatched when user clicks the delete button
TODO__DELETE__SUCCESS // Dispatched when a TODO has been successfully deleted
TODO__DELETE__FAILURE // Dispatched if there was a problem while attempting to delete a TODO
```

Our goal is to create a Redux middleware that is executed right after the ``TODO__DELETE__SUCCESS`` is dispatched and redirect the user to the index page. Here's how we are going to do it:

- First, let's create a utility method which will allows us to create a Redux middleware and execute custom code before or after the action is dispatched:

``` javascript
import * as Immutable from 'immutable';
// Helper method for creating a middleware that handles the given set of actions
export function createMiddleware(handlers) {
  return storeAPI => next => action => {
    const actionHandler = Immutable.List(handlers).find(h => h.action.type === action.type);
    // Execute custom middleware handler before the action is dispatched
    if (actionHandler && actionHandler.beforeHandler) {
      actionHandler.beforeHandler(storeAPI, action);
    }
    // Dispatch the action
    const result = next(action);
    // Execute custom middleware handler after the action is dispatched
    if (actionHandler && actionHandler.afterHandler) {
      actionHandler.afterHandler(storeAPI, action);
    }
    return result;
  }
}
```

- Next, we'll use the created utility method to define the redirect code. Notice I have defined an ``afterHandler``, which will effectively execute the code after the action has been dispatched:

``` javascript
import {browserHistory} from 'react-router'

export const redirectMiddleware = createMiddleware([
  {
    // Redirect user to index page on successful TODO delete
    action: TODO__DELETE__SUCCESS,
    afterHandler: (storeAPI, action) => {
      browserHistory.replace({pathname: '/'});
    }
  }
]);
```

- Last thing needed is to apply the Redux middleware to the store, here's how to do it:

``` javascript
import {createStore, combineReducers, applyMiddleware} from 'redux'
import {redirectMiddleware} from '../middleware/redirect_middleware';

let todoApp = combineReducers(reducers);
let store = createStore(
  todoApp,
  applyMiddleware(redirectMiddleware) // Apply redirect middleware
);
```

### Conclusion
I have been using Redux middleware for handling navigation redirects for quite a while now, and this solution has proven to be intuitive, scalable, and easy to maintain. Redux middleware are also commonly used for logging, crash reporting, and talking to an asynchronous API.

Do you have a different solution for dealing with navigation redirects in client-side applications? Leave a comment below :).
