# Scoped HTML
Scoped HTML is a markup pattern that lets us break an HTML document into a hierarchy of smaller-sized structural boundaries called scopes. This form of scoping is for structural purposes and is designed to complement native HTML's implicit scopes established by semantic structural elements.

Thus in Scoped HTML, a scope may be explicitly designated or implicitly inferred from native HTML semantics where that satisfies the intended structure.

## On this page:
+ [Explicit Scopes](#explicit-scopes)
+ [Implicit Scopes](#implicit-scopes)
+ [The Scope API](#the-scope-api)
+ [ScopedHTML Configuration](#scopedhtml-configuration)

## Explicit Scopes
A scope may be explicitly designated on an element using a pre-agreed attribute. The `scope` attribute is the default choice, but this can be changed.

```html
<div scope=”article”>
  <div>
    <h1>COVID19 Situation Report</h1>
    <div>Many more countries have been affected!</div>
  </div>
  <div>John Doe</div>
</div>
```

Certain elements within a scope that have semantic relatonships with the scope may also be explicitly designated. A pre-agreed attribute is used to create this relationship. The `scope` attribute is the default choice, but this can be changed.

```html
<div scope=”article”>
  <div>
    <h1 scope="article-title">COVID19 Situation Report</h1>
    <div scope="article-content">Many more countries have been affected!</div>
  </div>
  <div scope="article-author">John Doe</div>
</div>
```

Now, regardless of how complicated the document tree eventually becomes, our semantic *scope-part* relationship can always produce a simple mental model, saving everyone the structural guesswork and mcuh cognitive load.

```html
article
  |-- title
  |-- content
  |-- author
```

### Scope Hint

To provide for a quick hint, the *scope root* may optionally maintain a list of its semantic parts on a pre-agreed attribute. The `scope-parts` attribute is the default choice, but this can be changed.

```html
<div scope=”article” scope-parts="title content author">
  <div>
    <h1 scope="article-title">COVID19 Situation Report</h1>
    <div scope="article-content">Many more countries have been affected!</div>
  </div>
  <div scope="article-author">John Doe</div>
</div>
```

### Nested Scopes

A *scope part* may constitute a new scope of its own, and have its own parts. An *article's* `author` part can be a good example.

```html
<div scope=”article” scope-parts="title content author">
  <div>
    <h1 scope="article-title">COVID19 Situation Report</h1>
    <div scope="article-content">Many more countries have been affected!</div>
  </div>
  <div scope="article-author user">
    <div scope="user-name"></div>
    <div scope="user-avatar"></div>
  </div>
</div>
```

This would produce the following model.

```html
article
  |-- author (user)
  |  |-- avatar
  |  |-- name
  |- content
```

Where scopes tend to be recursive, parts are associated with the scope that is closest to them up the hierarchy.

```html
<div scope=”article>
  <div>
    <div scope=”article-author”></div>
    <div scope=”article-content”></div>
    <div scope=”article-brief article”>
      <div scope=”article-date”></div>
      <div scope=”article-description”></div>
      <div scope=”anotherrole-othernode”></div>
    </div>
  </div>
</div>
```

The nested articles would produce the following model.

```html
article
  |-- author
  |-- content
  |-- othernode
  |-- brief (article)
    |-- date
    |-- description
```

## Implicit Scopes

The idea of scoping is inherent to many native HTML elements. These natural relationships, or implicit scopes, are automatically recognized in Scoped HTML and need not be explicitly redesigned. But do these scopes work?

The [HTML standard](https://html.spec.whatwg.org/multipage) maintains a design note or specification that dictates an element's **category** and **content model** (or permissible elements). Here an element’s *category*, if defined, represents a scope, and elements in its *content model* serve as semantic parts. This becomes clearer in a few examples.

The `<html>` element’s content model permits two elements that must also be direct children.

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

The `<head>` element’s content model permits only elements that have been categorized a *Metadata* elements.

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
  <meta name=”” content=”” />
  <meta name=”” content=”” />
  <meta name=”” content=”” />
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

Where two identical scopes are nested, parts are associated with the scope that is closest to them up the hierarchy. The `<body>` element and the `<blockquote>` element are a good example of recursive scopes.

The `<body>` element is categorized as a *Sectioning Root* element that permits *Flow Content* elements in its content model. The `<blockquote>` element is one of those *Flow Content* elements, but at the same time, a *Sectioning Root* element that could have its own *Flow Content* elements. This gives us a recursive *Flow Content* scope as seen below.

```html
<body>
  <h1></h1>
  <div>
    <p></p>
  </div>
  <blockquote>
    <h1></h1>
    <div></div>
  </blockquote>
  <div></div>
</body>
```

So above, every element is semantically associated with the body’s *Sectioning Root* scope except those within blockquote’s *Sectioning Root* scope. This gives us the following relationship.

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
  <article>
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
  <div role=”header”></div>
  <article>
    <div role=”header”></div>
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

The Scope API is a feature available on every element that lets us access an element's scoped parts. We can simply retreive an element from the DOM and access its scoped parts.

```js
// The article itself
let article = document.querySelector('[scope~="article"]');

// The article's title part
let articleTitle = article.scope.title;

// The article's content part
let articleContent = article.scope.content;

// The article's author parts
let articleAuthorName = article.scope.author.scope.name;
let articleAuthorAvatar = article.scope.author.scope.avatar;
```

In the code above, we have assumed that the *ScopedHTML* JavaScript library has been [loaded](/chtml/guide/installation.md) in the page.

### Implementation Notes
+ **Parts are lazy-loaded** - ScopedHTML follows a strategy of not querying the DOM until a specific *part* is requested from the scope object. So at first, an element's *scope* object is an empty object. This also calls for a few techniques to be able to access a *part* for the first time on this empty object.
  + Using `Reflex.get()` - By default, ScopedHTML is based on the general-purpose reflection API [Reflex](/reflex/), so one way we could retreive parts from a scope object is with `Reflex.get()`. This *get* request will automatically trigger the DOM query that loads the *part* element from the DOM.

      ```js
      let articleTitle = Reflex.get(article.scope, 'title');
      ```

  + Using Proxies - We can [configure](#configuration) ScopedHTML to automatically return scope objects as `Proxy` instances. Proxies will enable us use the `.` (dot) notation while actually forwarding the request to an underlying DOM query.

      ```js
      // Configure ScopedHTML to always return a Proxy for scope objects
      ENV.params.proxyScopedObjects = true;
      ```

      ```js
      // We can npw
      let articleTitle = article.scope.title;
      ```
  + Using Proxies and `Reflex.get()` per instance - We can actually proxy the scope object on our own. This is even preferred to globally configuring the `ENV.params.proxyScopedObjects` parameter.
      
      ```js
      // We can npw
      let articleScope = new Proxy(article.scope, {get: Reflex.get});
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

+ `ENV.params.proxyScopedObjects` - (Boolean): This tells ScopedHTML to always return a Proxy for scope objects. This is `false` by default.

+ `ENV.params.scopeAttribute` - (String): This specifies the attribute name for establishing a scope. This is `scope` by default.

+ `ENV.params.partAttribute` - (String): This specifies the attribute name for associating a *part* to a scope. This is `scope` by default.

+ `ENV.params.scopeNamePrefix` - (String): This lets ScopedHTML differentiate between random attribute values and real scope names. This is empty by default. This is useful when a generic attribute, like class, is being use as `ENV.params.scopeAttribute`.

    ```js
    // Lets use the class attribute for scoping
    ENV.params.scopeAttribute = 'class';
    ENV.params.partAttribute = 'class';
    // Lets also add that only classes that begin with an underscore would be valid scope names
    // This will make ScopedHTML ignore other classnames.
    ENV.params.scopeNamePrefix = '_';
    ```

    ```html
    <!-- Now in the markup below, the class "blue"
    wouldn't be regarded as a scope name. -->
    <div class=”_article blue”>
      <div>
        <h1 class="_article-title">COVID19 Situation Report</h1>
        <div class="_article-content">Many more countries have been affected!</div>
      </div>
      <div class="_article-author _user">
        <div class="_user-name"></div>
        <div class="_user-avatar"></div>
      </div>
    </div>
    ```

+ `ENV.params.partsHintAttribute` - (String): This specifies the attribute name that optionally lists the parts of a scope. This is `parts-hint` by default.

    ```html
    <div class="_article-author _user" parts-hint="name avatar">
      <div class="_user-name"></div>
      <div class="_user-avatar"></div>
    </div>
    ```

+ `ENV.params.scopePropertyName` - (String): This specifies the property name for accessing an element's scope. This is `scope` by default.
  
    ```js
    // Lets change how we access an element's scope
    ENV.params.scopePropertyName = 'parts';
    ```
      
    ```js
    // We should now be using .parts instead of .scope
    let article = document.querySelector('[scope~="article"]');
    let articleTitle = article.parts.title;
    ```