# Scoped HTML
Scoped HTML is a markup pattern that lets us break an HTML document into a hierarchy of smaller-sized structural scopes. A scope may be explicitly designated or implicitly inferred from HTML's semantic scoping.

## On this page:
+ [Explicit Scopes](#explicit-scopes)
+ [Implicit Scopes](#implicit-scopes)
+ [The Scope API](#the-scope-api)
+ [ScopedHTML Configuration](#scopedhtml-configuration)

## Explicit Scopes
A scope is explicitly designated on an element using the `root` *Boolean* attribute.

```html
<div root>
  <div>
    <h1>COVID19 Situation Report</h1>
    <div>Many more countries are in!</div>
  </div>
  <div>John Doe</div>
</div>
```

The `root` attribute creates a context for naming elements within a subtree. The special `scoped:id` attribute is used to name elements that are uniquely associated with this context.

```html
<div id="article" root>
  <div>
    <h1 scoped:id="title">COVID19 Situation Report</h1>
    <div scoped:id="content">Many more countries are in!</div>
  </div>
  <div scoped:id="author">John Doe</div>
</div>
```

Scoped-IDs are expecially important in exposing a scope's structural parts called nodes. We could use them to implement structural APIs over the DOM tree. So above, our structural API would be:

```html
#article
  |-- title
  |-- content
  |-- author
```

Now, regardless of how complex the document tree eventually becomes, there will always be a consistent model for an application to bank on, saving everyone much structural guesswork.

### Nested Scopes
A *node* may constitute a new scope of its own, and have its own nodes. An *article* scope, for example, could have an `author` node that is itself a new root.

```html
<div id="article" root>
  <div>
    <h1 scoped:id="title">COVID19 Situation Report</h1>
    <div scoped:id="content">Many more countries are in!</div>
  </div>
  <div scoped:id="author" root>
    <div scoped:id="name"></div>
    <div scoped:id="avatar"></div>
  </div>
</div>
```

This would produce the following model.

```html
#article
  |-- author
  |  |-- avatar
  |  |-- name
  |- content
```

## Implicit Scopes
The idea of scoping is inherent to many native HTML elements. These implicit scopes are automatically recognized in Scoped HTML and need not be explicitly redesigned.

The [HTML standard](https://html.spec.whatwg.org/multipage) maintains a design specification that dictates an element's **category** and its **content model** (or permissible elements). Where an element’s *category* is defined, its permissible elements are implicitly-scoped to this category. Here are a few examples.

The `<html>` element has two direct elements in its content model.

```html
<html>
  <head></head>
  <body></body>
</html>
```

Its model would look like this:

```html
html
  |-- head
  |-- body
```

The `<head>` element defines *Metadata* elements in its content model; that is, elements in the *Metadata* category.

Below, the following *Metadata* elements can only be used once in the `<head>` element.

```html
<head>
  <title></title>
  <base />
</head>
```

The model would look like this:

```html
head
  |-- title
  |-- base
```

But certain elements are also permitted to appear any number of times within their permissible scope. The `<head>` element, again, permits multiple `<meta>` elements.

```html
<head>
  <title></title>
  <meta name="" content="" />
  <meta name="" content="" />
  <meta name="" content="" />
  <base />
</head>
```

This would give us the following model. \(Notice the list notation on the meta node.\)

```html
head
  |-- title
  |-- meta (3)
  |-- base
```

### Recursive Scopes
Where two identical scopes are nested, elements are associated with the scope that is closest to them up the hierarchy. The `<body>` element and the `<blockquote>` element are a good example of recursive scopes.

The `<body>` element is categorized as a *Sectioning Root* element that permits *Flow Content* elements in its content model. The `<blockquote>` element is one of those *Flow Content* elements, but at the same time, a *Sectioning Root* element that could have its own *Flow Content* elements. This gives us a recursive *Flow Content* scope as seen below.

```html
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

So above, every *Flow Content* element is semantically associated with the body's *Sectioning Root* scope except those within blockquote's *Sectioning Root* scope. This gives us the following relationship.

```html
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

Another form of the *Sectioning Root* category is *Sectioning Content*. Elements in this category are `<article>`, `<aside>`, `<nav>`, and `<section>`. Notice how the `<header>` element, being a *Flow Content* element, is semantically associated below. Also note that there cannot be more than one within its permissible scope.

```html
<body>
  <header></header>
  <article> <!-- Sectioning Content -->
    <header></header>
    <div></div>
  </article>
</body>
```

This would give us the following model.

```html
body
  |-- header
  |-- article (1)
    |--#0
      |-- header
      |-- div (1)
```

### ARIA Roles
The implicit and explicit roles defined in the ARIA specification are automatically recognized in Scoped HTML. Elements that have an explicit or implicit ARIA role can be accessed by both their *rolename* and their *tagname*.

```html
<body>
  <div role="header"></div>
  <article>
    <div role="header"></div>
    <div></div>
  </article>
</body>
```

This would give us the following model.

```html
body
  |-- div (1)
  |-- header
  |-- article (1)
    |-- #0
      |-- div (1)
      |-- header
```

The `<section>` element has the implicit ARIA role *region*. So both the names *section* and *region* should point to the `<section>` element.

```html
<body>
  <section>
  </section>
  <section>
  </section>
</body>
```

This would produce the following model.

```html
body
  |-- section/region (2)
```

The `<ul>` element has the implicit ARIA role *list*. The `<li>` element has the implicit ARIA role *listitem*. Here’s how this would look.

```html
<body>
  <ul>
    <li></li>
    <li></li>
  </ul>
</body>
```


```html
body
  |-- ul/list (1)
    |-- #0
      |-- li/listitem (2)
```

Notice above that the `<li>` element does not reflect in the `<body>` as their permissible scope ends with the `<ul>` element.

## The Scope API
The Scope API is a set of properties and methods that lets us access scopes on any element. This is made possible by the [*ScopedHTML* JavaScript library](/chtml/guide/installation.md).

### The `scopeSelector()` Instance Method
This method is used to query a scope using CSS selectors, much like the `querySelector()` method. The distinguishing factor is that only elements within the boundaries of the scope are matched. Additionally, the ID `#` selector is mapped to the `scoped:id` attribute instead of the `id` attribute.

```js
// Let's retrieve the #article element itself using document.querySelector()
let article = document.querySelector('#article');

// Let's query elements by "scoped-id"
let articleTitle = article.scopeSelector('#title');
let articleContent = article.scopeSelector('#content');

// Chained queries would just take us into nested scopes
let articleAuthorName = article.scopeSelector('#author').scopeSelector('#name');
```

### The `scopeTree` Property
This property can be used to directly access named elements.

```js
// Let's retrieve the #article element itself using document.querySelector()
let article = document.querySelector('#article');

// The article's title part
let articleTitle = article.scopeTree.title;
let articleContent = article.scopeTree.content;

// Chained access would just take us into nested scopes
let articleAuthorName = article.scopeTree.author.scopeTree.name;
```

To directly use the `scopeTree` property as seen above, a technique is however be necessary. This is because the `scopeTree` property is implemented as a *live* object instead of a static one.

ScopedHTML follows a strategy of not querying the DOM until a specific *node* is requested from the root element. So nodes arrive in the *scopeTree* property after the first eager query.

```js
// At first, the article's title node would be undefined in scopeTree
let articleTitle = article.scopeTree.title; // undefined

// It should now be available after the first eager query
let articleTitle = article.scopeSelector('#title');
let articleTitle = article.scopeTree.title; // works
```

But we do not always have to initially call the `scopeSelector()` method either. ScopedHTML allows us to use the `Reflex.get()` function (from the general-purpose [Reflex API](/reflex/)) to eagerly read the `scopeTree` object.

```js
let articleTitle = Reflex.get(article.scopeTree, 'title');
```

We could also [configure](#configuration) ScopedHTML to automatically return the `scopeTree` object as a `Proxy` instance. Accessing the returned Proxy will automatically eagerly query the DOM for us.

```js
// Configure ScopedHTML to always return a Proxy for scope objects
ENV.params.proxyScopedObjects = true;
```

```js
// We can npw
let articleTitle = article.scopeTree.title;
```

Using Proxies and `Reflex.get()`, we could even do a *per-instance* implementation. This is even preferred to globally configuring the `ENV.params.proxyScopedObjects` parameter.

```js
// We can npw
let articleScope = new Proxy(article.scopeTree, {get: Reflex.get});
let articleTitle = articleScope.title;
```

## ScopedHTML Configuration
ScopedHTML has been designd to be fully customizable. Simply obtain ScopedHTML's `ENV` object and configure its `params` property.

```js
// If ScopedHTML has been loaded via a script tag
let ENV = window.WebNative.ScopedHTML.ENV;

// To use the import method
import {ENV} from '@web-native-js/chtml/src/scoped-html/index.js';
// To access the same ENV from the Chtml's ENV
import {ENV as CHTML_ENV} from '@web-native-js/chtml';
let ENV = CHTML_ENV.ScopedHTML;
```

Here are the configuration options.

+ `ENV.params.proxyScopedObjects` - (Boolean): This tells ScopedHTML to always return a Proxy for `scopeTree` objects. This is `false` by default.

+ `ENV.params.rootAttribute` - (String): This specifies the attribute name for establishing a scope. This is `root` by default.

+ `ENV.params.scopedIdAttribute` - (String): This specifies the attribute name for creating scoped-IDs. This is `scoped:id` by default.

+ `ENV.params.scopeTreePropertyName` - (String): This specifies the property name for accessing an element's scope tree. This is `scopeTree` by default.
  
    ```js
    // Lets change how we access an element's scope
    ENV.params.scopeTreePropertyName = 'scopeParts';
    ```
      
    ```js
    // We should now be using .scopeParts instead of .scopeTree
    let article = document.querySelector('#article');
    let articleTitle = article.scopeParts.title;
    ```