# Backbone Required Attributes

[backbone-required-attributes](https://github.com/ubiqua/backbone-required-attributes) is a view helper to enforce specific view constructor parameters. Its goal is to help designing [Backbone.js views](http://backbonejs.org/#View) which do not perform complicated logic, but instead require to be initialized with certain parameters in order to properly work.

## Usage
``backbone-required-attributes`` allows to define in an object literal notation the necessary information a ``Backbone.View`` will need in order to properly work. Ideally, the information a ``Backbone.View`` needs will be passed to it in an object literal notation in its ``initialize`` method and then validated against its expected type or instance using the ``requiredAttrs`` object literal. ``backbone-required-attributes`` internally uses [underscore](http://underscorejs.org/) and if a value doesn't comply to its expected type or instance, a ``console.error`` will be logged.

After ``backbone-required-attributes`` has been loaded, it can be used as follows:

```javascript
  var MyExampleView = Backbone.View.extend({

    requiredAttrs: {
      'exampleModel': Backbone.Model,
      'age': 'number',
      'title': 'string'
    }

    initialize: function(options) {
      this.validateAttrs(options)
    }

  });
```

## Available rules
  - ``'string'``: Tests if object is a String.
  - ``'boolean'``: Tests if object is either true or false.
  - ``'array'``: Tests if object is an Array.
  - ``'number'``: Tests if object is a Number (including NaN).
  - If a function is provided as a rule to ``backbone-required-attributes``, it will attempt to test if the value has in its prototype chain the prototype property of the constructor ([see more](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof)). Therefore, you can also use rules such as:
    - ``Backbone.Router``
    - ``Backbone.Collection``
    - ``Backbone.Model``
    - ``Backbone.View``

## Road-map
  - [ ] Log errors and warnings with specific view name.
  - [ ] API to create custom rules.
  - [ ] Avoid running helper on tests since it will make it difficult to test views in an isolated manner.
