# Namespaces and Templates, Layout and Bundling

In CHTML, components intended for later use are defined together in a `<template>` element. They may, however, be defined individually in separate files on the server with a view to bundling them into a `<template>` element - whatever makes them live comfortably and easy to maintain.

## Namespaces and Templates

Components defined together in a `<template>` element are each assigned a unique namespace. Namespaces organize them in virtual categories and help us reference them and reuse them.

Namespaces are like file paths. They are assigned on the `data-namespace` attribute.

```markup
<template>
  <div data-role="article" data-namespace="html/content/article"></div>
</template>
```

Here we've placed article under a category named _content_. There could be other types in this category. And we can have subcategories to as much as organization requires.

```markup
<template>

  <!-- Displays article -->
  <div data-role="article" data-namespace="html/content/article/readonly"></div>

  <!-- Allows editing -->
  <div data-role="article" data-namespace="html/content/article/editable"></div>

</template>
```

Note that the way namespaces are used in CHTML demands that a namespace be of, at least, two parts. So a single word like `content` isn't a valid namespace; a valid one would be `html/content`.

Templates can be defined either _internally_ in the `<head>` section of a document or as _external_ HTML files that can be linked to from the document. For CHTML to automatically pick these up, the `chtml-bundle` type of `<template>` must be used.

```markup
<html>
  <head>

    <template is="chtml-bundle">
      <div data-namespace="html/content/article"></div>
    </template>

  </head>
  <body></body>
</html>
```

To link a bundle from the server, the `src` attribute is used. This attribute isn't defined as part of the `<template>` element and is entirely a CHTML's way of loading template content as the standard `<link type="import">` element is no longer supported.

```markup
<!-- file: bundle.html -->
<div data-namespace="html/content/article/readonly"></div>
<div data-namespace="html/content/article/editable"></div>

<!-- app -->
<html>
  <head>
    <!-- The src attribute demands that the <template> element be empty -->
    <template is="chtml-bundle" src="/bundle.html"></template>
  </head>
  <body></body>
</html>
```

Now multiple bundles can be used on the same document - whether defined _internally_ or _externally_. They will all work together in a cascaded manner, as covered in a subsequent section for bundle cascading. This opens the door to building rich applications with bundles from multiple sources.

## Layout and Bundling

The folder-based layout approach takes things from _virtual_ to _actual_ namespacing. Here, components are defined in stand-alone HTML files placed in a hierarchy of folders on the server.

Here is the equivalent layout of the namespaced article components above:

```markup
content
    |-- article
        |-- readonly.html
        |-- editable.html
```

Now, a component's namespace naturally becomes the combination of its path and filename; and the `data-namespace` attribute isn't required anymore.

In a straightforward process called _bundling_, it is easy to put all our components back into one file. This time, namespaces are automatically derived and assigned to each component. Check out the little _node.js_-based [bundler](../extras/the-bundler-utility.md) that ships with CHTML.

