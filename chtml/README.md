---
description: 'Put up amazing UIs in one go - complete with styling and automatic functionality without leaving HTML!'
---

# CHTML
CHTML is a suite of short specifications and technologies that bring a component-based architecture to the HTML language itself. This lets us create the UI and its components entirely in HTML. A pure-HTML approach is new and requires no build tools, nor compilers, nor even a template syntax!

This project is opensource and available on [GitHub](https://github.com/web-native/chtml).

## Overview
CHTML is centered on using the web platform itself to put up elegant user interfaces. A few specifications make this possible by easing out platform limitions that have made this difficult to do.
+ [**Scoped HTML**](/chtml/scoped-html/) - Scope-based markup pattern that lets us break an HTML document into smaller-sized structural boundaries.
+ [**Scoped JS**](/chtml/scoped-js/) - JavaScript-based presentational logic that works within the realms of its containing element.
+ [**Scoped CSS**](/chtml/scoped-css/) - CSS styling that is only applicable to its containing element and its descendants.
+ [**HTML Transport**](/chtml/html-transport/) - An import/export system that lets us define, extend, and distribute HTML components.

While all four technologies fit together seamlessly, they can also be used independently.

### Scoped HTML
Scoped HTML is a semantic markup pattern that translates to an intuitve structural API for applications. Here, you simply establish a scope with a name of your choice, and optionally associate key elements from within its subtree to the scope.

```html
<div id="el" scope="article">
    <div class="wrapper">
        <div scope="article-title"></div>
        <div scope="article-content"></div>
    </div>
</div>
```

This gives us a *scope-node* relationship or model that an application can always bank on.

```html
article
  |- title
  |- content
```

We also have a *scoped-HTML* API that makes it easy to traverse the model.

```js
let article = document.querySelector('#el');
let title = article.scope.title;
let content = article.scope.content;
// And if we had nested a child scope into "content", we could also do...
let nested = content.scope.nested;
```

Semantic markup with intuitive API access can save an application (or bot) a lot of structural guesswork and inefficient DOM queries.

Visit the [Scoped HTML Specs](/chtml/scoped-html/) to learn more.

### Scoped JS
Scoped JS is a breakthrough technology that lets us place JavaScript functionality anywhere in a page, just right where they are needed. Instead of retreiving elements into one huge global scope to imperatively manipulate them, we would simply take the functionality to the elements.

```html
<div scope="alert">
    <div scope="alert-message">This task is now complete!</div>
    <div scope="alert-exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      this.scope.exit.addEventListener('click', () => {
          this.remove();
      });
    </script>
</div>
```

Noteworthy is the script's isolation from the global scope, with the *this* reference pointing to the script's containing element.

With an explicit setting, certain global variables could however be accessed from scoped scripts.

```js
// Lets make jQuery available to scoped scripts
window.WebNative.ScopedJS.ENV.globals.$ = window.jQuery;
```

```html
<div scope="alert">
    <div scope="alert-message">This task is now complete!</div>
    <div scope="alert-exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      $(this.scope.exit).on('click', () => {
          $(this).remove();
      });
    </script>
</div>
```

A scope may also receive application data, and further distribute received data to child scopes. Automatic *reactivity* steps in to do the dirty work of keeping the UI in sync with application state.

Visit the [Scoped JS Specs](/chtml/scoped-js/) to learn more.

### Scoped CSS
Scoped CSS allows us to define styles that only apply to the containing element and its descendants. Scoped styles were once promised in the CSS standards and are now even more demanded than ever across the web.

Scoped CSS is currently being reconceived in CHTML.

### HTML Transport
HTML Transport is an export/import distribution system for HTML. Here, reusable HTML fragments are defined as *exports* within a `<template>` element.

```html
<template is="chtml-bundle">

    <div namespace="export/one"></div>
    <div namespace="export/two"></div>

</template>
```

These *exports* can be easily placed anywhere in the main document.

```html
<body>
    <micro-import namespace="export/one"></micro-import>
    <micro-import namespace="export/two"></micro-import>
</body>
```

And we can programmatically import a micro module.

```js
let import1 = MicroModules.import('export/one');
```

Taking things further, micro modules can be dynamically bundled on the server as regular HTML files. An `src` attribute is used on a `<template>` element to import this remote bundle.

**file://bundle.html**

```html
<div namespace="export/one"></div>
<div namespace="export/two"></div>
```

**file://index.html**

```html
<template is="chtml-bundle" src="bundle.html"></template>
```

Code organization, extensibility and composability are HTML Transport' powerful features that lets us build an entire app with much fewer components.

Visit the [HTML Transport Specs](/chtml/html-transport/) to learn more.

## Guide
+ [Installation Guide](/chtml/guide/installation-guide.md)
+ [Server-Side Rendering](/chtml/guide/server-side-rendering.md)