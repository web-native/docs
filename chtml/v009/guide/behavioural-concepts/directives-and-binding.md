# Directives and Bindings

While the CHTML Component API helps us work with components programmatically, it also allows us to factor behaviour into a component declaratively using directives.

This section discusses how to declare directives in CHTML, and how they do the heavy-lifting.

## Directives

Directives are executable statements declared right within a component's markup and are based on the general [JavaScript Expression Notation](https://github.com/web-native/jsen) \(JSEN\).

CHTML employs [data blocks](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) to factor these directives into a component.

```markup
<body data-role=”app”>

 <script type="text/scoped-js">
    html('Hello from CHTML!');
  </script>

</body>
```

The directive above would have the following programmatic equivalent - where `component` is a reference to the component's instance:

```javascript
component.html('Hello from CHTML!');
```

And here's how we would address a component's node:

```markup
<body data-role=”app”>

  <div data-role="app-content"></div>

  <script type="text/scoped-js">
    tree.content.html('Hello from CHTML!');
  </script>

</body>
```

Again, the directive above would have the following programmatic equivalent - where we need to have [_hinted_ the component's `tree`](../structural-concepts/the-component-api.md#the-components-tree-property) property:

```javascript
component.tree.content.html('Hello from CHTML!');
```

Apparently, the _declarative_ syntax is just like the _programmatic_ syntax. And as expected, a directive expression can be a chain of method calls.

```markup
<body data-role=”app”>

  <div data-role="app-content"></div>

  <script type="text/scoped-js">
    // Assuming we've also implemented some "shake" method...
    tree.content.html('Hello from CHTML!').shake();
  </script>

</body>
```

### Usage

The best way to use directives is to see a directive as excatly what it sounds like: an _instruction_ that runs on, or _consumes_, the component's API; not a means to declare new variables, functions or classes. You build functionalities in the _application layer_, then, you consume them in the _presentation layer_. So the extent to what's possible with a directive expression is determined by the capabilities of the component API and the application at large. This successfully acheives the ideal separation between _application code_ and _presentation code_.

In the following examples, we will be accessing a component's underlying DOM element. So the method names below are those of a standard DOM element.

_Math/concatenation expression._

```javascript
el.append(‘Summing 2 and 3 gives us ‘ + (2 + 3));
```

_\(Deep\) references to properties of the component._

```javascript
el.append(‘The color of this text is ‘ + el.style.color);
```

_Method calls._

```javascript
el.append(‘My name is ‘ + el.tagName.toLowerCase());
```

_Assertions, Comparison and Conditionals._

```javascript
el.append(el.hasAttribute(‘is‘) || el.tagName.indexOf(‘-‘) > -1 ? ‘I am a custom element.’ : ‘I am a standard element.’);
```

_Array and Object constructs._

```javascript
el.animate([{color:’red’}, {color:’blue’},], {
    duration:600,
});
```

_ES6 Function constructs._

```javascript
tree.content.el.addEventListener(‘click’, event => {
    event.target.animate([{color:’red’}, {color:’blue’},], {
        duration:600,
    });
});
```

_ES6 Function constructs for a loop._

```javascript
tree.content.el.children.forEach((child, index) => {
    child.append(index);
});
```

### Conditionals

The standard `if () ... else` construct is fully supported in directives. But there are other ways to write conditional directives.

**Using Dynamic Node Paths.** By nature, JSEN path expressions like `tree.content.el.append()` can be rephrased as dynamic expressions using the bracket notation.

For example, to dynamically decide between `append` and `prepend` based on some _condition_, we could say:

```javascript
// If the condition passes, append, otherwise, prepend.
tree.content.el[condition ? ‘append’ : ‘prepend’](’Thanks for visiting!’);
```

**Using Assertions.** Just before terminating a directive in a JSEN _Call_ expression like `append()`, we could make certain assertions that should either validate or fail the rest of the expression. These expressions use the logical `&&` \(AND\) and `||` \(OR\) operators.

For example, if we wanted to validate an _append_ operation on the presence of the `content` node, we could say:

```javascript
// This validates only if the content node is available.
// Fails if not
tree.content && tree.content.el.append(’Thanks for visiting!’);

// Conversely, this fails if the content node is available.
// Validates if not.
tree.content || tree.content.el.append(’Thanks for visiting!’);
```

**Using Ternary Operators.** Parenthesized expressions can form the basis of a _Call_ expression. So we can place a conditional expression in here.

For example, to dynamically choose the node on which to finally execute a directive, we could say:

```javascript
// Append to the content node, if available.
// Otherwise, to the root element.
(tree.content ? tree.content.el : el).append(’Thanks for visiting!’);
```

## Reactivity

In CHTML, directives are executed _reactively_ internally. Under the hood, after initially executing a directive, CHTML begins to observe all the variables \(or references\) in the expression with a view to re-executing the directive in the event of a change.

In the previous section, we saw how we could programmatically bind the UI, or _view_, to the state of an application. Now we will be able to implement the same _view_ declaratively, using directives and leveraging the entire reactivity system.

Here's that _view_ once more, and the application:

_The View_

```markup
<div data-role="article" data-tree="author content" id="atcl">
  <div data-role="article-author"></div>
  <div data-role="article-content"></div>
</div>
```

```javascript
// The model will be arriving on the component instance itself
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

Here's the same _view_ but without the binding logic:

```markup
<div data-role="article" data-tree="author content" id="atcl">

  <div data-role="article-author"></div>
  <div data-role="article-content"></div>

  <script type="text/scoped-js">
    tree.content.el.append(model);
  </script>

</div>
```

