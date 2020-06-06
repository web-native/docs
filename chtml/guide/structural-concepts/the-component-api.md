# The Component API

The Chtml API is a simple library that translates component models from markup into an Object Model for programmatic use. In its basic form, a Chtml instance lets us access a component's nodes as properties and objects.

```javascript
import Chtml from ‘@web-native-js/chtml’;
```

If Chtml has been loaded via a script tag, it will be available in the global `WebNative` object.

```javascript
const Chtml = WebNative.Chtml;
```

A component instance is created via the Chtml constructor. The constructor takes a DOM element as root element, and optionally accepts a `params` object that provides additional parameters for the instance.

Syntax:

```javascript
const component = new Chtml(el[, params]);
```

This creates an object that lets us access the underlying component’s nodes as properties. The instance `get()` method is used to retrieve nodes.

Examples:

```javascript
// Lets create a component on the DOM documentElement itself
// and access its nodes. 
const doc = new Chtml(document.documentElement);

let head = doc.get('head');
let body = doc.get('body');
```

Nodes in the tree live as Chtml instances of their own. This makes it easy to access down the tree.

```javascript
// Here is the document title
let title = doc.get('head').get('title');
```

To access the component's underlying DOM element, the readonly `el` property is used.

```javascript
// Here is the component's underlying DOM element
let documentElement = doc.el;

// Here is the underlying DOM element for the title node
let titleElement = title.el;

// And here’s how we could write to the titleElement
titleElement.innerHTML = ‘Hello from CHTML!’;
```

In a component instance, nodes are _lazy-load_ and stored in the instance's `tree` property. In other words, the DOM is actually queried the first time a node is accessed. So requesting a node multiple times has no performance penalty. In fact, it is actually possible to do `doc.tree.head` after the first `doc.get('head')`.

## The Component's `tree` Property

It is possible to access nodes directly on the componet's `tree` property - without having to call the component's `get()` method. But because nodes are lazy-loaded, this property will be an empty object at first, so it requires some technique to directly access nodes here.

* **Using the Reflex API**: The general-purpose [Reflex API](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/chtml/reflex/README.md) can be used with CHTML components as CHTML is based on _reflex actions_.

  Reflex can be obtained separately, but CHTML also ships with it.

  ```javascript
    // Via import
    import {Reflex} from '@web-native-js/chtml';
    // Or if CHTML was loaded via a script tag...
    const Reflex = Chtml.Reflex;
  ```

  We can use `Reflex.get()` to _reflexively_ access nodes.

  ```javascript
    let head = Reflex.get(doc.tree, 'head');
    let body = Reflex.get(doc.tree, 'body');
  ```

  We can use `Reflex.init()` to _virtualize_ node access.

  ```javascript
    Reflex.init(doc.tree, ['head', 'body']);
    let head = doc.tree.head;
    let body = doc.tree.body;
  ```

* **Using Component Hints**: A component can prepare its `tree` object ahead of time if given a hint as to the nodes defined in its DOM. As seen [earlier](the-component-model.md#component-hint), the `data-tree` attribute is used for this purpose.

  Define a _self-aware_ component.

  ```markup
    <html data-tree="head body">
      <head></head>
      <body></body>
    </html>
  ```

  Access the tree.

  ```javascript
    let head = doc.tree.head;
    let body = doc.tree.body;
  ```

## Other Component Properties

A component instance also exposes a few other information via the following properties:

* `el (HTMLElement)` - This property is a reference to the component's underlying DOM element as supplied to the constructor.
* `params (object)` - This property is a copy of the instance's options as supplied to the constructor.
* `roles (array)` - This property is an array of the component's roles as specified on the `data-role` attribute.
* `directives (array)` - This property is an array of the component's parsed JSEN directives as specified on the component's data script \(`<script text/scoped-js></script>`\) element. \(Directives are covered in the section for [behavioural concepts](../behavioural-concepts/)\)
* `namespace (string)` - This property is the component's namespace as specified on the `data-namespace` attribute. \(Namespaces are covered in the section for [compositional concepts](../compositional-concepts/)\).

## The Component Anatomy

To better reason about a component, here's an anatomy of the humble component instance:

```markup
component
    |-- el (HTMLElement)
    |-- params (object)
    |-- tree (object)
    |-- roles (array)
    |-- directives (array)
    |-- namespace (string)
```

Later on, we will learn about the special _model_ property that can be added to the component from within an application:

```markup
component
    |-- model (any)
```

CHTML components are also very flexible; a different name can be configured, each, for many of these these properties. This is covered in the [configuration section](../extras/configuration.md).

## Extending The Component's Functionality

It is possible to add certain useful methods to the component's prototype to help with DOM manipulation. This is intentionally left out in the standard CHTML API to make it as generic as possible. But adding custom methods is as easy as extending the `Chtml` class.

```javascript
const customChtml = class extends Chtml {

    /**
     * This method will work like jQuery's html() method
     * With it, we can insert and retreive the component's HTML content.
     * 
     * @param string content     - The html content to insert
     * 
     * @return string            - The retreived HTML content, when called without arguments
     * /
    html(content = null) {
        if (!arguments.length) {
            return this.el.innerHTML;
        }
        this.el.innerHTML = content;
        return this;
    }
}
```

We should now use the `customChtml` class instead of `Chtml`.

```javascript
const doc = new customChtml(document.documentElement);
doc.get('body').html('<h1>Hello from my custom CHTML!</h1>');
```

For an API-rich CHTML, you may want to check out [CUI](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/chtml/cui/README.md) - a CHTML-based frontend framework for building Single Page Appications \(SPAs\).

