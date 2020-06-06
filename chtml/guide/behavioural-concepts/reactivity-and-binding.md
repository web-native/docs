# Reactivity and Binding

Modern applications are often very dynamic! Representing the state of the application on the UI can be quite challenging as everything is constantly changing. To reduce the amount of code for the developer, modern UI libraries now come with some _reactive_ mechanism that enables them keep the UI in sync with dynamic application state/data.

Reactivity in CHTML is based on a form called _reflex actions_, and the general-purpose _Reflex API_ we used [earlier](../structural-concepts/the-component-api.md) is all we need! In this section, we will demonstrate _reflex actions_ and how to programmatically keep track of changes. In the next section, you will see how you can even forget about tracking changes with the help of directives!

## Reactivity

The component object itself, including its `tree` property, is a reactive container, and can, thus, be observed. We'll demonstrate this with the component instance below.

```markup
<div data-role="article" data-tree="author content" id="atcl">
  <div data-role="article-author"></div>
  <div data-role="article-content"></div>
</div>
```

```javascript
const article = new Chtml(document.querySelector('#atcl'));
```

Below, we want to observe when nodes land for the first time on the component's `tree` object. Remember that nodes are _lazy-loaded_. So, above, the _author_ node won't be loaded until the first time we try to access it.

```javascript
// First, let's construct an observer to catch this event
Reflex.observe(article.tree, 'author', author => {
    console.log(author ? author.el : 'Deleted!');
});

// Let's access the node for the first time.
// Our observer should be called.
article.tree.author.el.innerHTML = 'Hi';
```

If that node ever gets deleted from the `tree` object, some _reflex action_ will fire and our observer will be called.

```javascript
// Let's explicitly delete the node from the object.
// This won't, however, remove it from the DOM
Reflex.deleteProperty(article.tree, 'author');
```

Attempting to re-access the node should reload the node again from the DOM. Our observer should also be triggered by the _reflex action_.

```javascript
// Our observer should be called.
article.tree.author.el.innerHTML = 'Hi, again!';
```

Removing this node from the DOM will also delete it from the component's `tree` object. Our observer should be called again by means of _reflex actions_.

```javascript
// Let's directly remove the node from the DOM.
article.tree.author.el.remove();

// Re-accessing the node should now return undefined.
article.tree.author.el.innerHTML = 'Hi, yet again!'; // Reference error
```

### Binding

In the example above, we actually bound an observer to the `author` node. The observer ran each time the state of this node changed. This gives us something like a _leader_-&gt;_follower_ relationship - the node being the _leader_, while the observer block, the _follower_. To put it better, what we see is actually a _model_-&gt;_binding_ relationship - the node being the _model_, while the observer block, the _binding_. So a _binding_ is a block of code that follows the lead of a _model_. We can build on this idea to acheive greater things.

We will now introduce a similar relationship where the _model_ is some data from the _application_, while the _binding_ is a block of code that renders this data on the _view_, or UI. We want the rendering to be kept in sync with the state of this _model_.

_The View_

```javascript
// The model will be arriving on the component instance itself
// So we observe the component instance
Reflex.observe(article, 'model', modelValue => {
    article.tree.content.el.append(modelValue);
});
```

_The Application_

```javascript
// This should be rendered immediately
article.model = '<h1>Hello World!</h1>';

// This should also be rendered - appended
setTimeout(() => {
    article.model = '<div>Thanks for visiting!</div>';
}, 900);
```

And we can have more complex models.

_The View_

```javascript
// The model is now an object
Reflex.observe(article, 'model', model => {
    article.tree.author.el.append(model.author);
    article.tree.content.el.append(model.content);
    // Notice the options.observeDown setting here
    // We want the observer to be called also when the model itself is modified deeply
}, {observeDown:true});
```

_The Application_

```javascript
const model = {
    author:'John Doe',
    content:'<h1>Hello World!</h1>',
};
article.model = model;

// Now even when we update just the content property
// the view will still pick it up
setTimeout(() => {
    // Note that we need to use Reflex now
    Reflex.set(model, 'content', '<div>Thanks for visiting!</div>');
}, 900);
```

At this point, we have bound the _view_ to the _application_.

#### The `bind()` Instance Method

This method is just another way to set the component's `model` property. It simply takes a _model_ and sets it to the `model` property.

```javascript
article.bind(model);
```

But it also brings us more flexibility when we change the default _model_ property from `model` to something else, using the [`params.modelProperty`](../extras/configuration.md) option.

```javascript
// To set this globally...
Chtml.params.modelProperty = 'data';

// To change this per instance...
var article = new Chtml(el, {modelProperty:'data'});
```

Calls to `article.bind()` will now set the given _model_ to the component's `data` property. So this method serves as a more consistent public API to set a component's _model_ regardless of the implementation details.

### Optimizing DOM updates

As seen, CHTML does not intercept operations that update the DOM using some _virtual DOM_ technique. But it makes room for implementing DOM updates that are performant. One common problem with direct DOM-manipulation is _layout thrashing_. A good way to avoid this is to batch and separate actions that mutate the DOM from those that simply query the DOM/UI. You can leverage tools like [fastDOM](https://github.com/wilsonpage/fastdom) for this wonderful technique. Or the fully-featured, CHTML-based [CUI](https://github.com/web-native/cui) frontend framework may be all you need!

For more infromation on layout thrashing, see [this article](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) on Web Fundamentals.

