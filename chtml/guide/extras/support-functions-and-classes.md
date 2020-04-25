# Support Functions

Chtml offers certain methods for working with modules and bundles.

## The `Chtml.import()` Static Method

This method simply retrieves a module by namespace from across loaded bundles. We will demonstrate these methods against the simple template below.

```markup
<template is=”chtml-bundle”>

  <div data-role=”user” data-namespace=”html/badge/user“>
    <div data-role=”user-avatar”></div>
    <div data-role=”user-name”></div>
  </div>

  <div data-role=”article” data-namespace=”html/content/article“>
    <div>
      <chtml-import data-role=”article-author” data-namespace=”html/badge/user”></chtml-import>
        <div>
          <div data-role=”article-content”></div>
        </div>
    </div>
  </div>

  <chtml-import data-namespace=”html/content/article/readonly“></chtml-import>

</template>
```

```javascript
var articleEl = Chtml.import(‘html/content/article’);
```

The result is the HTML element when found, or undefined, when not found. At this point, all applicable recomposition would have been made.

Below, importing the `html/content/article/readonly` module will apply inheritance-based composition from its `html/content/article` super namespace. And the element returned remains the `<chtml-import>` element.

```javascript
var readonlyArticleEl = Chtml.import(‘html/content/article/readonly’);
```

## The `Chtml.from()` Static Method

In addition to creating instances from the Chtml constructor, the `Chtml.from()` static method may be used. This method accepts the same augments as with the constructor, but also agrees to accept the root element as a CSS selector or even a HTML markup.

Syntax:

```javascript
const component = Chtml.from(input[, params]);
```

Examples:

```javascript
// Create an instance from a DOM object as usual
const doc = Chtml.from(document.documentElement);

// Create an instance from a selector
const body = Chtml.from(‘body’);
const component = Chtml.from(‘#some-element’);

// Create an instance from markup;
// the from() method will first automatically resolve this to an element.
let markup = ‘<div data-role=”comp”><span data-role=”node”></span></div>’;
const component = Chtml.from(markup);
```

This method is also able to find elements by namespace, like the `Chtml.import()` method. But while the `Chtml.import()` method searches the document's bundles with a gievn namespace, this method searches the document body with the gievn namespace.

Below, we retrieve a namespaced element from within the document body.

```markup
<body>
  <chtml-import ondemand data-namespace=”html/content/article/editable“></chtml-import>
</body>
```

```javascript
var editableArticleEl = Chtml.from(‘html/content/article/editable’);
```

The result is an HTML element that’s fully resolved to the `html/content/article` module. At this point, the temporary element will be automatically replaced by the imported article module.

Note:

When extending the `Chtml` class into a custom class, the `Chtml.from()` method will need a special attention. Even though this method is inherited normally by the custom class, calling the method from the custom class will return an instance of `Chtml` instead of the expected instance of the custom class.

```javascript
const customClass = class extends Chtml {};
let component = customClass.from(input); /// instance of Chtml, not customClass
```

This is easy to fix by overriding the `from()` in the custom class and binding the custom class to this inherited method.

```javascript
const customClass = class extends Chtml {

    /**
     * @inheritdoc
     */
    from(imput, params = {}) {
        // Notice the third argument here, being a reference to the customClass.
        return super.from(input, params, customClass);
    }
};

let component = customClass.from(input); // instance of customClass
```

## The `Chtml.ready()` Static Method

This method is used to keep code execution in sync with the document’s “ready” state. It accepts a callback that will run when the DOM announces readiness – an event that indicates that the document tree has been initialized and safe to access.

```javascript
Chtml.ready(() => {
    // Put code here
});
```

This method also extends to wait for CHTML bundles to load. This is useful when running code that relies on external CHTML bundles. As it is, bundles have to be loaded in order to access their modules. In fact, a warning is issued on attempting to import modules while bundles are still loading.

```javascript
Chtml.ready(() => {
    var remoteModule = Chtml.import(‘html/content/article/readonly’);
});
```

To prevent waiting for bundles, pass false as the second argument to this method.

```javascript
Chtml.ready(callback, /*waitForBundles*/false);
```

