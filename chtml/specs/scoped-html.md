# Scoped HTML

Scoped HTML is an HTML document that is based on a hierarchy of smaller-sized structural scopes instead of one global document scope. It provides a more semantic document structure that is easier to think about and work with. Thinking and working in scopes is the modern key to effortlessly building and maintaining the UI.

This specification defines a standard way of explicitly declaring scopes and encapsulating structure, and also incorporates the implicit scopes that are inherent to semantic HTML. It further introduces _scopeTree_ API for traversing scoped structures.

## On this page:

* [Scopes](scoped-html.md#scopes)
  * [Explicit Scopes](scoped-html.md#explicit-scopes)
  * [Implicit Scopes](scoped-html.md#implicit-scopes)
* [The `scopeTree` API](scoped-html.md#the-scopetree-api)
* [ScopedHTML Configuration](scoped-html.md#scopedhtml-configuration)

## Scopes

A scope is a _virtual encapsulation_ of a subtree\*. It functions as a reference point for accessing semantic parts of the subtree.

### Explicit Scopes

A scope is explicitly designated on an element using the `root` _Boolean_ attribute.

```markup
<div root>
  <div>
    <h1>COVID19 Situation Report</h1>
    <div>Many more countries are in!</div>
  </div>
  <div>John Doe</div>
</div>
```

The context created by the `root` attribute allows us to use element IDs that apply only within the scope. Scoped IDs are based on the `scoped:id` attribute.

```markup
<div root id="article">
  <div>
    <h1 scoped:id="title">COVID19 Situation Report</h1>
    <div scoped:id="content">Many more countries are in!</div>
  </div>
  <div scoped:id="author">John Doe</div>
</div>
```

Scoped IDs are expecially important in maintaining a semantic structure that can be used as a scope's public structural API. Our structural API above would be:

```markup
#article
  |-- title
  |-- content
  |-- author
```

Now, regardless of how complex the document tree eventually becomes, there will always be a consistent structural API that an application can bank on. Much structural guesswork also gets out of the way for everyone else.

#### Nested Scopes

A _semantic node_ may establish a scope of its own, and have its own semantic nodes. An `article` scope, for example, could have an `author` node that functions as the _root_ of a new scope.

```markup
<div root id="article">
  <div>
    <h1 scoped:id="title">COVID19 Situation Report</h1>
    <div scoped:id="content">Many more countries are in!</div>
  </div>
  <div root scoped:id="author">
    <div scoped:id="name"></div>
    <div scoped:id="avatar"></div>
  </div>
</div>
```

This would produce the following API.

```markup
#article
  |-- title
  |-- content
  |-- author
     |-- avatar
     |-- name
```

### Implicit Scopes

The concept of scoping is inherent to many native HTML elements. These implicit scopes are automatically recognized in Scoped HTML and need not be explicitly redefined.

The [HTML standard](https://html.spec.whatwg.org/multipage) maintains a design specification that dictates an element's **category** and its **content model** \(or permissible elements\). Here, an element’s permissible elements are implicitly-scoped to its _category_.

For example, the `<html>` element has two direct elements in its content model that are implicitly-scoped.

```markup
<html>
  <head></head>
  <body></body>
</html>
```

Its strucutral API would now be:

```markup
html
  |-- head
  |-- body
```

As another example, the `<head>` element defines _Metadata_ elements in its content model; that is, elements in the _Metadata_ category. So below, the following _Metadata_ elements are soped to the `<head>` element and can only be used once in the `<head>` element.

```markup
<head>
  <title></title>
  <base />
</head>
```

The strucutral API would now be:

```markup
head
  |-- title
  |-- base
```

Certain elements are also permitted to appear any number of times within their permissible scope. The `<head>` element, again, permits multiple `<meta>` elements.

```markup
<head>
  <title></title>
  <meta name="" content="" />
  <meta name="" content="" />
  <meta name="" content="" />
  <base />
</head>
```

This would give us the following strucutral API. \(Notice the list notation on the _meta_ node.\)

```markup
head
  |-- title
  |-- meta (3)
  |-- base
```

#### Recursive Scopes

Where two identical scopes are nested, elements are associated with the scope that is closest to them up the hierarchy. The `<body>` element and the `<blockquote>` element are a good example of recursive scopes.

The `<body>` element is categorized as a _Sectioning Root_ element that permits _Flow Content_ elements in its content model. The `<blockquote>` element is one of those _Flow Content_ elements, but also happens to be a _Sectioning Root_ element that could have its own _Flow Content_ elements. This gives us a recursive _Flow Content_ scope as seen below.

```markup
<body> <!-- Sectioning Root -->
  <h1></h1> <!-- Flow Content -->
  <div> <!-- Flow Content -->
    <p></p> <!-- Flow Content -->
  </div> <!-- Flow Content -->
  <blockquote> <!-- Flow Content, also Sectioning Root -->
    <h1></h1> <!-- Flow Content -->
    <div></div> <!-- Flow Content -->
  </blockquote>
  <div></div> <!-- Flow Content -->
</body>
```

So above, every _Flow Content_ element is semantically associated with the body's _Sectioning Root_ scope except those within the blockquote's _Sectioning Root_ scope. This gives us the following strucutral API.

```markup
body
  |-- div (2)
  |  |-- #0
  |  |  |-- p (1)
  |  |-- #1
  |-- h1 (1)
  |-- blockquote (1)
  |  |-- #0
  |     |-- h1 (1)
  |     |-- div (1)
  |- p (1)
```

Another form of the _Sectioning Root_ category is _Sectioning Content_. Elements in this category are `<article>`, `<aside>`, `<nav>`, and `<section>`. Notice how the `<header>` element, being a _Flow Content_ element, is semantically associated below. Also note that there cannot be more than one within its permissible scope.

```markup
<body>
  <header></header>
  <article> <!-- Sectioning Content -->
    <header></header>
    <div></div>
  </article>
</body>
```

This would give us the following strucutral API.

```markup
body
  |-- header
  |-- article (1)
    |--#0
      |-- header
      |-- div (1)
```

#### ARIA Roles

The scopes created by the roles defined in the ARIA specification are automatically recognized in Scoped HTML. Elements that have an explicit or implicit ARIA role can be accessed by both their _rolename_ and their _tagname_.

```markup
<body>
  <div role="header"></div>
  <article>
    <div role="header"></div>
    <div></div>
  </article>
</body>
```

This would give us the following strucutral API.

```markup
body
  |-- div (1)
  |-- header
  |-- article (1)
    |-- #0
      |-- div (1)
      |-- header
```

The `<section>` element has the implicit ARIA role _region_. So both the names _section_ and _region_ should point to the `<section>` element.

```markup
<body>
  <section>
  </section>
  <section>
  </section>
</body>
```

This would produce the following strucutral API.

```markup
body
  |-- section/region (2)
```

The `<ul>` element has the implicit ARIA role _list_. The `<li>` element has the implicit ARIA role _listitem_. Here’s how this would look.

```markup
<body>
  <ul>
    <li></li>
    <li></li>
  </ul>
</body>
```

```markup
body
  |-- ul/list (1)
    |-- #0
      |-- li/listitem (2)
```

Notice above that the `<li>` element does not reflect in the `<body>` as their permissible scope ends with the `<ul>` element.

## The `scopeTree` API

To make it easier to work with scoped documents, Scoped HTML introduces the `scopeTree` property as part of the DOM API. This property can be used to directly access elements by `scoped:id`.

This API is made available by the [_ScopedHTML_ JavaScript module](../chtml-guide/installation.md).

```javascript
// Let's retrieve the #article element itself using document.querySelector()
let article = document.querySelector('#article');

// Let's access elements by "scoped-id"
let articleTitle = article.scopeTree.title;
let articleContent = article.scopeTree.content;

// Chained access would just take us into nested scopes
let articleAuthorName = article.scopeTree.author.scopeTree.name;
```

The `scopeTree` property is however a live object and not a static one as ScopedHTML follows a strategy of not querying the DOM until a specific _node_ is requested. In other words, nodes are lazily-loaded into the _scopeTree_ object as a reaction to a query; so something has to trigger this query the first time. But this is easy to do.

ScopedHTML allows us to use the `Reflex.get()` function \(from the general-purpose [Reflex API](../../reflex/)\) to eagerly read the `scopeTree` object. Reflex-based access would automatically trigger the appropriate DOM query.

```javascript
let articleTitle = Reflex.get(article.scopeTree, 'title');
```

Being based on Reflex also gives us the ability to observe when a node arrives or exits the `scopeTree` object. We could bind an observer to give us this insight, which might be useful for debugging purposes.

```javascript
Reflex.observe(article.scopeTree, nodes => {
  console.log(nodes);
});

let articleTitle = Reflex.get(article.scopeTree, 'title');
```

Although we have explicitly used `Reflex.get()` above, this may not even be necessary when using the `scopeTree` property within reflex-driven APIs as these naturally follow the above methodology under the hood in working with objects.

[Scoped JS](scoped-js.md), for example, provides a reflex-based runtime that fires reflex actions as it evaluates statements that read or modify an object.

```markup
<div root id="article">
  <div>
    <h1 scoped:id="title">COVID19 Situation Report</h1>
    <div scoped:id="content">Many more countries are in!</div>
  </div>
  <div scoped:id="author"></div>
  <script type="text/scoped-js">
    // Below, this.scopeTree.author is read reflexively
    this.scopeTree.author.innerHTML = 'John Doe';
  </script>
</div>
```

Outside of Reflex-driven code, we could [configure](scoped-html.md#configuration) ScopedHTML to automatically return the `scopeTree` object as a `Proxy` instance. Returned Proxies are configured to actively react to read operations.

```javascript
// Configure ScopedHTML to always return a Proxy for scope objects
ENV.params.proxyScopedObjects = true;
```

```javascript
// We can npw
let articleTitle = article.scopeTree.title;
```

Better yet, using Proxies and `Reflex.get()`, we could even do a _per-instance_ implementation instead of globally configuring the `ENV.params.proxyScopedObjects` parameter.

```javascript
// We can npw
let articleScope = new Proxy(article.scopeTree, {get: Reflex.get});
let articleTitle = articleScope.title;
```

## ScopedHTML Configuration

ScopedHTML has been designd to be fully customizable. Simply obtain ScopedHTML's `ENV` object and configure its `params` property.

```javascript
// If ScopedHTML has been loaded via a script tag
let ENV = window.WebNative.ScopedHTML.ENV;

// To use the import method
import {ENV} from '@web-native-js/chtml/src/scoped-html/index.js';
// To access the same ENV from the Chtml's ENV
import {ENV as CHTML_ENV} from '@web-native-js/chtml';
let ENV = CHTML_ENV.ScopedHTML;
```

Here are the configuration options.

* `ENV.params.proxyScopedObjects` - \(Boolean\): This tells ScopedHTML to always return a Proxy for `scopeTree` objects. This is `false` by default.
* `ENV.params.rootAttribute` - \(String\): This specifies the attribute name for establishing a scope. This is `root` by default.
* `ENV.params.scopedIdAttribute` - \(String\): This specifies the attribute name for creating scoped-IDs. This is `scoped:id` by default.
* `ENV.params.scopeTreePropertyName` - \(String\): This specifies the property name for accessing an element's scope tree. This is `scopeTree` by default.

  ```javascript
    // Lets change how we access an element's scope
    ENV.params.scopeTreePropertyName = 'scopedIDs';
  ```

  ```javascript
    // We should now be using .scopedIDs instead of .scopeTree
    let article = document.querySelector('#article');
    let articleTitle = article.scopedIDs.title;
  ```

