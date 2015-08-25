##What is el

`el` is basically a reference to a DOM element, and all views must have one.

There are two ways to associate a DOM element with a view: a new element can be created for the view and subsequently added to the DOM, or a reference can be made to an element that already exists in the page.

`tagName` defaults to `div`.
```javascript
var TodosView = Backbone.View.extend({
tagName: 'ul', // required, but defaults to 'div' if not set
className: 'container', // optional, you can assign multiple classes to
// this property like so: 'container homepage'
id: 'todos', // optional
});
```

When declaring a view, we can define `options`, `el`, `tagName`, `id`, and `className` as functions.

####Understanding render()

`render()` is an option function that defines the logic for rendering a template.
