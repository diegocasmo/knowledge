# Simple BDD Backbone Views

[Backbone.js](http://backbonejs.org) is a small Javascript library which helps give structure to web applications. In contrast to the many other front end technologies out there, Backbone attempts to give just as little as possible in order to build an application. As a result, we need to be extra careful when designing and developing an application to make it easy to test and maintain in the future.

In this blog post, I will explain how to write a simple and maintainable Backbone view. I will be creating a simple cart item comment form view using [BDD](http://guide.agilealliance.org/guide/bdd.html) which will allow the user to enter a comment on a cart item. I will be using [Coffeescript](http://coffeescript.org/) and the [Jasmine](http://jasmine.github.io/) testing framework as tools to develop and test the view.

A Backbone view can be thought of as a controller in the MVC software design pattern. It handles user input, and it is in charge of updating the necessary DOM elements when appropriate. A view should know as little as possible about the application, in other words, it needs to be as dumb as it can be. It should not perform any complicated logic, instead, it should only be in charge of rendering and updating data in its associated model. The model related to this view will be the [Single Source of Truth](http://en.wikipedia.org/wiki/Single_Source_of_Truth); all the information the view is going to use is going to be stored and retrieved from a single place.

When creating a view, I like to divide it into four smaller segments:

- Initialization: Test whether this view exists and if it has the data it needs in order to work properly.
- Rendering: Test if the template is rendered and its values are correct.
- Events: Test if a view listens accurately to user interaction with it.
- Methods: Test whether the view delegates the action correctly.

We will start up by creating the first test at `cart_item_comment_form_view_spec.js.coffee` to make sure the view is defined and accessible:

```javascript
describe 'Cart Item Comment Form View', ->

  it 'is defined', ->
    expect(App.Views.CartItemCommentForm).toBeDefined()
```

In order to make this test pass, we simply declare the view at `cart_item_comment_form_view.js.coffee` as follows:

```javascript
class App.Views.CartItemCommentForm extends Backbone.View
```

Next, we need to test the view initialization by making sure it has the correct tagName, className, and the necessary options in order to work. Since this view will set or get the cart item comment according to user interaction, a cart item model will be required in order for it to work. Luckily, jasmine provides two helper methods to help creating [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) tests; `beforeEach`, and `afterEach.` We will setup the view to test on the `beforeEach` method by passing to it a cart item model, and then clean it up on the `afterEach` method.

```javascript
beforeEach ->
  cartItemModel = new Backbone.Model()
  options =
    cartItemModel: cartItemModel
  @view = new App.Views.CartItemCommentForm(options)

afterEach ->
  @view = null
```

Now, we can create the necessary tests to check if the view is being initialized correctly:

```javascript
describe 'Initialization', ->

  it 'has correct tagName', ->
    expect(@view.tagName).toEqual('div')

  it 'has correct className', ->
    expect(@view.className)
        .toEqual('cart-item-comment-form')

  it 'throws error if no “cartItemModel” is passed to it', ->
    # Save error on variable
    throwMeAnError = ->
      # Initialize view without a cart item model
      new App.Views.CartItemCommentForm()
      return
    expect(throwMeAnError).toThrow(new Error('A cart item model is required.'))

  it 'sets cart item model correctly', ->
    expect(@view.cartItemModel instanceof Backbone.Model)
      .toBeTruthy()
```

To make the tests pass, we need to add the following code:

```javascript
tagName: 'div'

className: 'cart-item-comment-form'

# Initialize view
initialize: ->
  # Check for view requirements
  if !@options.cartItemModel
    throw new Error('A cart item model is required.')

  # Initialize view requirements
  @cartItemModel = @options.cartItemModel
```

We have successfully tested our cart item comment form view is being initialized properly. It is now time to test whether it is being rendered as expected. As far as templates, I have found it is better to write inline templates even though they might seem more difficult to understand. But by using inline templates, we force ourselves as developers to write smaller, simpler, and logicless templates, which in turn will help us to create a more independent and maintainable application.

With the following tests, I want to make sure if the view renders a cart item comment when one is present, and if it renders an empty textarea if no cart item comment is present.

```javascript
describe 'Rendering', ->

  describe 'when cart item has comment', ->

    beforeEach ->
      # Override getComment stub
      @view.cartItemModel.getComment = -> 'foo'
      @view.render()

    afterEach ->
      @view.cartItemModel.getComment = -> return

    it 'renders cart item comment on textarea', ->
      expect(@view.$el.find('.comment-area').val()).toEqual('foo')

  describe 'when cart item doesn\'t have comment', ->

    it 'renders empty comment textarea', ->
      expect(@view.$el.find('.comment-area').val()).toEqual('')
```

In order for this tests to pass, we will first need to add stubs for the `getComment` and `setComment` methods of a cart item model because we are testing the view not the model. Therefore, our test setup will now look like this:

```javascript
beforeEach ->
  cartItemModel = new Backbone.Model()
  # Create stub for getComment
  cartItemModel.getComment = -> return
  # Create stub for setComment
  cartItemModel.setComment = -> return
  options =
    cartItemModel: cartItemModel
  @view = new App.Views.CartItemCommentForm(options)
  @view.render()
```

Notice I call `render` at the end of the `beforeEach` block. This will allow the view to be “cleansed” after every single test.

To make the tests above turn green, lets add the following code to the view:

```javascript
template: _.template(
  '<textarea cols="30" rows="10" class="comment-area">' +
    '<%= comment %>' +
  '</textarea>'
  )

# Render view on template
render: ->
  context =
    comment: @cartItemModel.getComment()
  @$el.html(@template(context))
  @
```

The next thing to test are events. The user needs to be able to write a comment on the textarea and the view must delegate this action to the cart item model in order for it to automatically set the comment. Therefore, the cart item comment form view has to listen to the `keyup`, and `change` events and then delegate this action to the cart item model. In order to test for this, I will use Jasmine [spies](http://jasmine.github.io/2.0/introduction.html#section-Spies) to check if the view calls correctly the desired method.

```javascript
describe 'Events', ->

  it 'it listens to "keyup" event on textarea', ->
    spyOn(@view, 'setComment')
    @view.delegateEvents()
    @view.$el.find('textarea')
      .trigger('keyup')
    expect(@view.setComment).toHaveBeenCalled()
    expect(@view.setComment.calls.count()).toEqual(1)

  it 'it listens to "change" event on textarea', ->
    spyOn(@view, 'setComment')
    @view.delegateEvents()
    @view.$el.find('textarea')
      .trigger('change')
    expect(@view.setComment).toHaveBeenCalled()
    expect(@view.setComment.calls.count()).toEqual(1)
```

Notice I call `delegateEvents` after each spy I create. The main reason for doing that is to refresh the view events to be able to call the spy later. To make the new tests pass, we will need to add the following code:

```javascript
# View events
events: ->
  'keyup textarea': 'setComment'
  'change textarea': 'setComment'

# Sets comment on cart item model
setComment: (event) ->
  event.preventDefault()
```

The last thing to test in the view are methods. View methods shouldn’t be cumbersome with complicated logic and business rules. As I said above, the view doesn’t need to know anything about the application. As a result of this, I will test if the view calls the appropriate model function (`setComment`), that is, test if it correctly delegates the action to the model, and if the it returns itself from the `render` method (to allow for chaining).

```javascript
describe 'Methods', ->

  describe '#render', ->

    it 'returns the view object', ->
      expect(@view.render()).toEqual(@view)

  describe '#setComment', ->

    it 'calls "setComment" on cart item model', ->
      spyOn(@view.cartItemModel, 'setComment')
      @view.delegateEvents()
      comment = 'foo'
      @view
        .$el.find('.comment-area').val(comment)
      # Fake preventDefault
      @view.setComment(preventDefault: -> )
      expect(@view.cartItemModel.setComment)
        .toHaveBeenCalled()
      expect(@view.cartItemModel.setComment.calls.count())
        .toEqual(1)

    # User might want to erase comment
    it 'calls "setComment" on cart item model if comment is empty', ->
      spyOn(@view.cartItemModel, 'setComment')
      @view.delegateEvents()
      comment = ''
      @view
        .$el.find('.comment-area').val(comment)
      # Fake preventDefault
      @view.setComment(preventDefault: -> )
      expect(@view.cartItemModel.setComment)
        .toHaveBeenCalled()
      expect(@view.cartItemModel.setComment.calls.count())
        .toEqual(1)
```

In order to make the tests turn green, all we need to do is to fill `setComment` in the view as follows:

```javascript
# Sets comment on cart item model
setComment: (event) ->
  event.preventDefault()
  comment = $.trim(@$el.find('.comment-area').val())
  @cartItemModel.setComment(comment)
```

## Conclusion

Writing small, modular, and well designed views is extremly important, and by defining the real purpose of a view, bad practices and patterns can be quickly avoided and thus help us to create a sane and maintainable application. Always keep in mind that view tests catch the most regression issues, therefore, even though creating tests for views can sometimes be a daunting task, at the long run, it will always be worthy.
