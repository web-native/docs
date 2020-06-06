# CHTML Specs

Here is the suite of specifications and tooling that lets you build elegant user interfaces using the web platform itself.

* [**Scoped HTML**](scoped-html.md) - Scope-based markup pattern that lets us break an HTML document into smaller-sized structural scopes.
* [**Scoped JS**](scoped-js.md) - Scripts that are scoped to their containing elements.
* [**Scoped CSS**](scoped-css.md) - Stylesheets that are scoped to their containing elements.
* [**HTML Transport**](html-transport/) - An import/export system that lets us define, extend, and distribute HTML components.

While all four technologies fit together seamlessly, they can also be used independently.

## Scoped HTML

Scoped HTML follows a semantic markup pattern that lets us create an HTML document as a hierachy of smaller-sized scopes. Each scope establishes a new naming context for elements within its subtree.

A scope is designated on an element by simply adding the _Boolean_ `root` attribute. This creates a context for implementing scoped IDs.

```markup
<div root id="article">
    <div class="wrapper">
        <div scoped:id="title"></div>
        <div scoped:id="content"></div>
    </div>
</div>
```

Scoped IDs are especially useful in maintaining a structural API for applications. Above, our structural API would be:

```markup
#article
  |- title
  |- content
```

We also have the _ScopedHTML_ API that makes it easy to traverse these structural models.

```javascript
// The regular querySelector() function would give us the #article element
let article = document.querySelector('#article');

// The new scopeTree DOM property would give us the structural parts
let title = article.scopeTree.title;
let content = article.scopeTree.content;
```

Scopes, can also be nested to form a tree.

```markup
<div root id="article">
    <div class="wrapper">
        <div scoped:id="title"></div>
        <div scoped:id="content"></div>
    </div>
    <div root scoped:id="author">
        <div class="wrapper">
            <div scoped:id="avatar"></div>
            <div scoped:id="name"></div>
        </div>
    </div>
</div>
```

```markup
#article
  |- title
  |- content
  |- author
    |- avatar
    |- name
```

```javascript
// We could chain the acccess
let authorName = article.scopeTree.author.scopeTree.name;
```

Now, much structural guesswork and inefficient DOM queries have been avoided with a simple semantic markup pattern.

Visit the [Scoped HTML docs](scoped-html.md) to learn more.

## Scoped CSS

Scoped CSS is a styling approach that lets us couple smaller-sized stylesheets with elements in an HTML document. Defined rules now get scoped to these containing elements.

You define a scoped CSS with the _Boolean_ `scoped` attribute. The special `:root` pseudo-class is used to address its implicit root element.

```markup
<div id="article">
    <style scoped>
        :root {
            color: red;
        }
        .wrapper {}
    </style>
    <div class="wrapper">...</div>
</div>
```

Scoped CSS fits right in with Scoped HTML. If we designated the `#article` element as _root_, then we would be able to style its structural parts by ID. The ID selector `#` would now map to the `scoped:id` attribbute instead of the normal `id` attribbute.

```markup
<div root id="article">
    <style scoped>
        :root {
            color: red;
        }
        .wrapper {}
        #title {
            font-weight: bold;
        }
        #content {
            font-weight: normal;
        }
    </style>
    <div class="wrapper">
        <div scoped:id="title"></div>
        <div scoped:id="content"></div>
    </div>
</div>
```

If we wanted to style the structural parts of deeply nested scopes, a path notation would be used.

```markup
<div root id="article">
    <style scoped>
        :root {
            color: red;
        }
        .wrapper {}
        #title {
            font-weight: bold;
        }
        #content {
            font-weight: normal;
        }
        #user:hover / #name {
            font-weight: bold;
        }
    </style>
    <div class="wrapper">
        <div scoped:id="title"></div>
        <div scoped:id="content"></div>
    </div>
    <div root scoped:id="author">
        <div class="wrapper">
            <div scoped:id="avatar"></div>
            <div scoped:id="name"></div>
        </div>
    </div>
</div>
```

Scoped styles save us the specificity wars that come with document-level stylesheets. We also gain the ease of maintenace and progressive development.

Visit the [Scoped CSS docs](scoped-css.md) to learn more.

## Scoped JS

Scoped JS is a special technology that lets us couple JavaScript functionality with any element in a page. While we traditionally maintain elements and their presentational logic separately, Scoped JS lets us place functionality just right where they are needed.

We define a _scoped script_ with the special `text/scoped-js` MIME type.

```markup
<div id="alert">
    <div class="message">This task is now complete!</div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      this.querySelector('.exit').addEventListener('click', () => {
          this.remove();
      });
    </script>
</div>
```

Noteworthy is the script's isolation from the global scope, with its _this_ reference pointing to the script's containing element.

With an explicit setting, certain global variables could also be accessible to _scoped scripts_.

```javascript
// Lets make jQuery available to scoped scripts
window.WebNative.ScopedJS.ENV.globals.$ = window.jQuery;
```

```markup
<div id="alert">
    <div class="message">This task is now complete!</div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      $('.exit', this).on('click', () => {
          $(this).remove();
      });
    </script>
</div>
```

If we employed Scoped HTML in our markup, we could simply traverse the _scope tree_ and do without _selectors_ altogether. Notice below that we are directly accessing the root element's [`scopeTree` property](scoped-html.md#the-scopetree-property).

```markup
<div root id="alert">
    <div scoped:id="message">This task is now complete!</div>
    <div scoped:id="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      this.scopeTree.exit.addEventListener('click', () => {
          this.remove();
      });
    </script>
</div>
```

A scope may also receive application data, and further distribute received data to child scopes. Automatic _observability_ steps in to do the dirty work of keeping the UI in sync with application state.

Visit the [Scoped JS docs](scoped-js.md) to learn more.

## HTML Transport

HTML Transport is an export/import system for HTML. Here, reusable HTML fragments are defined as _exports_ within a `<template>` element.

```markup
<template is="html-bundle">

    <div namespace="export/one"></div>
    <div namespace="export/two"></div>

</template>
```

These _exports_ could be easily placed anywhere in the main document.

```markup
<body>
    <html-import namespace="export/one"></html-import>
    <html-import namespace="export/two"></html-import>
</body>
```

And in a script, we could programmatically import an _export_.

```javascript
let import1 = HTMLTransport.import('export/one');
```

Now, while we have statically defined exports in a `<template>` element above, we could also have defined them as standalone HTML files on the server with a view to dynamically bundling them together as a remote _HTML bundle_.

Here is how a remote _HTML bundle_ file on the server could look:

**file://bundle.html**

```markup
<!-- the file at some-path/export/one.html -->
<div namespace="export/one"></div>

<!-- the file at some-path/export/two.html -->
<div namespace="export/two"></div>
```

This remote bundle is easily connected to a page by using the `src` attribute on a `<template>` element.

**file://index.html**

```markup
<html>
    <head>
        <template is="html-bundle" src="bundle.html"></template>
    </head>
    </body>...</body>
</html>
```

Code organization, extensibility and composability are HTML Transport's distinguishing features that let us build an entire app with much fewer components.

Visit the [HTML Transport docs](html-transport/) to learn more.

