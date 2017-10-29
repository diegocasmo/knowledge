# Using React’s Context to Pass Variables From the Server to the Client

I recently got hired by I client to create a new functionality for a single page application. The client wanted the marketing team of the company to be able to easily change the landing page content. The landing page was already built with ``React`` using [isomorphic/universal rendering](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/) and the ability to edit and save the page content was also already in place and saved in a database.

The problem I had to solve was how to pass variables (containing the data necessary for rendering the page) from the server to the client in an application using [react-router](https://github.com/ReactTraining/react-router) without loosing the benefits of isomorphic rendering. Initially, this seemed to me as a very easy task, but as I will later explain, the fact that the application was built with isomorphic rendering made this task an opportunity to learn something new.

The goal of this blog post is to explain how I used React's context feature to pass variables from the server to the client without loosing the benefits of isomorphic rendering.

### The Challenge
I found a couple of solutions to similar problems online, but none of them worked for the same reason: the HTML rendered by the server wasn't the same as the one rendered by the client. In an isomorphic application, the HTML rendered by the server must be the same as the one rendered by the client, otherwise you would loose the benefits of it and get a [React check-sum warning](http://stackoverflow.com/a/34315767/6373590).

### The Solution
I had already worked with the well known state container ``Redux``, and I figured I could use something similar to the ``react-redux`` [Provider](https://github.com/reactjs/react-redux/blob/master/src/components/Provider.js) component. So I went ahead and took a look at the source code and found out this component internally uses React's context feature (unknown to me at this time) to make the state of the application seemingly available to all container components.

React's context feature allows to pass data through the component tree without having to pass the props down manually at every level. It's actually  similar to using global variables to pass state through the application, thus it must be used sparingly (also its API is an experimental feature which is likely to change in the future).

Let me now explain all the components I used to implement the desired feature. I hope this gives you a better understanding of how I was able to use React's context feature to solve the problem described above:

- ``Provider``: This component implements the context feature, and exposes the ``prop`` called ``appData`` to other components. The whole application is going to be wrapped by it:

``` javascript
class Provider extends React.Component {
  getChildContext() {
    return {appData: this.props.appData};
  }

  render() {
    return(
      <div>
        {this.props.children}
      </div>
    );
  }
}

Provider.childContextTypes = {
  appData: React.PropTypes.object.isRequired
};
```

- ``server.jsx``: This file has an ``Express`` handler to catch all routes and isomorphically serve the application using React's ``renderToString`` method. Notice ``appData`` is passed to both the ``Provider`` and the ``index`` template:

``` javascript
  res.render('index', {
    renderedRoot: ReactDOMServer.renderToString(
      <Provider appData={appData}>
        <RouterContext {...props}/>
      </Provider>
    ),
    appData: appData
  });
```

- ``index.ejs``: This file bootstraps the whole application. It receives ``appData`` and then initializes the application:

``` javascript
<html>
  <body>
    <div id="root"><%- renderedRoot %></div>
    <script src="/app.js"></script>
    <script>
      var config = {
        appData: <%-JSON.stringify(appData)%>
      };
      MY_APP.initialize(config);
    </script>
  </body>
</html>

```

- ``client.jsx``: This file starts the application like you would normally do in the client side. It is wrapped by the ``Provider`` component and passes to it the ``appData`` received from the ``index.ejs`` file:

``` javascript
window['MY_APP'] = window['MY_APP'] || (() => {
  return {
    initialize: (config) => {
      ReactDOM.render(
        <Provider appData={config.appData}>
          <Router history={browserHistory}>
            { routes }
          </Router>
        </Provider>,
        document.getElementById('root')
      );
    }
  };
})();
```

- ``App.jsx`` (optional): The ``App`` component is the top level component of the application and all other components are wrapped by it as such (using ``react-router``):

``` javascript
  <Route component={App}>
    <Route path="/" component={HomeContainer}/>
  </Route>
```

This component is optional because ``appData`` is already available to all components of the application through React's ``context``. But as mentioned above, ``context`` must be used sparingly, so I decided to map the ``context`` as ``props`` for all container components instead since this is the common approach when using React:

``` javascript
class App extends React.Component {

  // Encapsulate 'appData' context as props for all children
  renderChildrenWithContextAsProps() {
      return React.Children.map(this.props.children, (child) => {
          return React.cloneElement(child, {
              appData: this.context.appData
          });
      });
  };

  render() {
    return (
      <div>
        {this.renderChildrenWithContextAsProps()}
      </div>
    );
  }
}

App.contextTypes = {
  appData: React.PropTypes.object.isRequired
};
```

Now all container components such as ``HomeContainer`` (see ``/`` route above) can access ``appData`` through ``props`` as you would normally do in React.

Implementing React’s context feature was a fun learning experience and I hope this post serves as a guide of how to do so. Have you ever had to solve a similar issue? How did you manage to do it?
