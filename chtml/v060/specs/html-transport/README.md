# HTML Transport
HTML Transport is an import/export system that lets us define, extend, and distribute HTML components. Its two most-important concepts are *exports* and *imports*. Inbetween is the concept of *dynamic recomposition*.

The combination of exports, imports, and dynamic recomposition makes it possible for us to build entire applications with much fewer components.

HTML Transport also natively supports [ScopedHTML](/chtml/v060/specs/scoped-html/) and [ScopedJS](/chtml/v060/specs/scoped-js/) where used in the same document.

## On this page:
+ [Exports](#exports)
  + [Bundles](#bundles)
+ [Imports](#imports)
  + [Imports Ondemand](#imports-ondemand)
  + [Shadow Imports](#shadow-imports)
+ [Dynamic Compositions](#dynamic-compositions)
  + [Import-Based Recomposition](#import-based-recomposition)
  + [Inheritance-Based Recomposition](#inheritance-based-recomposition)
  + [Managing Recomposition](#managing-recomposition)
+ [List Composition](#list-composition)
  + [Updating a List](#updating-a-list)
  + [Dynamic Item Namespaces](#dynamic-item-namespaces)
+ [The HTMLTransport API](#the-htmltransport-api)
+ [HTMLTransport Configuration](#htmltransport-configuration)

## Exports
HTML exports are fragments of HTML markup defined for later use, bundled together in a special `<template>` element. Each export is assigned a unique namespace for reference purposes.

Below, we've chosen to use the `namespace` attribute, but this can be changed.

```html
<template is="html-bundle">
  <div namespace="html/content/article"></div>
</template>
```

Namespaces are like file paths. They organize exports into virtual categories. So above, we just placed *article* under a category named *content*. We could go to add other types under this category. And we can have subcategories to as much as organization requires.

```html
<template is="html-bundle">

  <!-- Displays article -->
  <div namespace="html/content/article/readonly"></div>

  <!-- Allows editing -->
  <div namespace="html/content/article/editable"></div>

</template>
```

Note that a namespace is required to be of, at least, two parts. So a single word like `content` wouldn't be valid.

### Bundles
While exports can be statically defined in a bundle - a `<template>` element, they could also be defined as standalone files on the server while being able to dynamically combine them back into a single file. The process that brings them together is called *bundling* and can even be automated using an included [little Bundler](/chtml/v060/specs/html-transport/bundling.md). The dynamically-created file is called a *remote bundle*.

Remote bundles can be easily loaded into a `<template>` element using a custom `src` attribute.

```html
<!-- file: bundle.html -->
<div namespace="html/content/article/readonly"></div>
<div namespace="html/content/article/editable"></div>
```

```html
<!-- index.html -->
<html>
  <head>
    <template is="html-bundle" src="/bundle.html"></template>
    <!-- The src attribute demands that the <template> element be empty -->
  </head>
  <body></body>
</html>
```

Now multiple bundles can be used on the same document - whether defined statically or loaded remotely. They will all work together in a cascaded manner, as we will see [shortly](#cross-bundle-inheritance).

```html
<!-- index.html -->
<html>
  <head>

    <template is="html-bundle" src="/bundle.html"></template>
    <template is="html-bundle">

        <!-- components -->
        <div namespace="html/badge/user"></div>

        <!-- Media: images, videos, etc -->
        <img src="/assets/img/brand/logo.png" namespace="img/brand/logo" />

        <!--  Media with data-URLs. -->
        <!-- Good for preloading resources -->
        <img src="data:image/png,%89PNG%0D%0A=" namespace="img/brand/logo2" />

        <!-- SVG, possibly SVG icons -->
        <svg namespace="svg/brand/logo/24x24" >
        <path />
        </svg>

        <!-- Stylesheets -->
        <style type="text/css" namespace="css/badge" >
        /* rules */
        </style>

    </template>

  </head>
  <body></body>
</html>
```

## Imports
HTML imports are special elements that let us retreive and place *exports* into any location in an HTML document. Import go by the `html-import` tag.

```html
<body>
  <html-import namespace="html/content/article/readonly"></html-import>
</body>
```

Imports use the `namespace` attribute to reference exports. They function as mere placeholders as they are automatically replaced by the componet they import.

### Imports Ondemand
By default, import elements are resolved as soon as they get connected to the DOM. This makes sense most of the times. At other times, we may want imports to resolve *on-demand* - just at the time they are queried by an application. This is achieved with the `ondemand` *Boolean* attribute.

```html
<div root>
  <html-import ondemand namespace="html/badge/user"></html-import>
</div>
```

### Shadow Imports
It is possible to import components directly into an element's shadow DOM. *Shadow imports* are qualified with the `shadow` *Boolean* attribute. An import's *Shadow Host* is its immediate parent.

```html
<div id="host">
  <html-import shadow namespace=" html/badge/user"></html-import>
</div>
```

We could even send some `<style>` element into the shadow DOM.

```html
<div id="host">
  <html-import shadow namespace=" html/badge/user"></html-import>
  <html-import shadow namespace="css/badge"></html-import>
</div>
```

## Dynamic Compositions
In addition to its *define once, use everywhere* paradigm, HTML Transport further empowers us with other dynamic compositional features:
+ [Import-Based Recomposition](#import-based-recomposition)
+ [Inheritance-Based Recomposition](#inheritance-based-recomposition)

### Import-Based Recomposition
Import elements can do more than just place a component. They can be empowered to recompose the component they import. We do this by predefining additional properties on the import element for the imported element to inherit. The import element is replaced as usual, but this time, with a richly composed component.

There are three aspects of a component that can be recomposed on each import we make of it:
+ [Attribute Recomposition](#attribute-recomposition)
+ [Functional Recomposition](#functional-recomposition)
+ [Structural Recomposition](#structural-recomposition)

#### Attribute Recomposition
Attributes can be predefined on an import element for the imported element to inherit. For example, if we wanted an incoming component to have an ID, we would set this on the import element.

```html
<html-import namespace="html/badge/user" id="some-id"></html-import>
```

We could import the same component in another place to take on a different ID.

```html
<html-import namespace="html/badge/user" id="some-other-id"></html-import>
```

Using this same approach, we could import components to inherit *scoded-IDs* to form part of a scope's structural API. Below, we are importing a *user* component to take on the `author` role for an *article* component.

```html
<template is="html-bundle">
  <div namespace="html/badge/user" root>...</div>
</template>
```

```html
<div root>
  <html-import namespace="html/badge/user" scoped:id="author"></html-import>
</div>
```

This is how the final composition would look:

```html
<div root>
  <div namespace="html/badge/user" scoped:id="author">...</div>
</div>
```

As seen, inherited single-value attributes like ID replace any existing value that the component may already have. For other types of attributes, inherited values are merged with any existing values.

**List-Type Attributes:** Where list-type attributes, like the `class` and `role` attributes, are predefined on an import element, they will be transfered to the imported component, with newer values placed after the component's existing list, if any.

Below, we're importing a component to take on additional classes.

```html
<template is="html-bundle">
  <div namespace="html/badge/user" class="class0">...</div>
</template>
```

```html
<div root>
  <html-import namespace="html/badge/user" scoped:id="author" class="class1 class2"></html-import>
</div>
```

This is how the final composition would look; notice the recomposition on the `class` attribute:

```html
<div root>
  <div namespace="html/badge/user" scoped:id="author" class="class0 class1 class2">...</div>
</div>
```

It is possible to configure additional *list-type* attributes. Simply [obtain HTML Transport's `ENV` object](#configuration) and add to the `ENV.params.listTypeAttributes` array.

**Key-Value Attributes:** Where key-value attributes, like the `style` attribute, are predefined on an import element, they will be transfered to the imported component, with newer declarations placed after the component's existing declarations, if any. For the `style` attribute, CSS cascading automatically takes effect this way, where newer rules get to override existing rules.

Below, we're importing a component to take on a blue color by overriding its default red color.

```html
<template is="html-bundle">
  <div namespace="html/badge/user" style="color: red"></div>
</template>
```

```html
<html-import namespace="html/badge/user" style="color: blue"></html-import>
```

This is how the final composition would look; notice how the `style` attribute was recomposed on the imported component:

```html
<div namespace="html/badge/user" style="color: red; color: blue;"></div>
```

It is possible to configure additional *key-value* attributes. Simply [obtain HTML Transport's `ENV` object](#configuration) and add to the `ENV.params.keyValAttributes` array.

#### Functional Recomposition
Although HTML Transport does not require ScopedJS, it does provide native support for scoped scripts where used in the same document. Here, a ScopedJS script can be defined on an import element for the incoming component to inherit. The new ScopedJS *statements* will be composed into the imported component; placed after the component's existing *statements*, if any.

This is what happens below.

```html
<template is="html-bundle">
  <div namespace="html/alert/success" root>
    <div scoped:id="message">...</div>
    <script type="text/scoped-js">
      this.innerHTML = 'Hello World!';
    </script>
  </div>
</template>
```

```html
<html-import namespace="html/alert/success">
  <script type="text/scoped-js">
    this.querySelector('.exit').addEventListener('click', () => {
        this.remove();
    });
  </script>
</html-import>
```

The final composition would give us:

```html
<div namespace="html/alert/success" root>
  <div scoped:id="message">...</div>
  <script type="text/scoped-js">
    this.innerHTML = 'Hello World!';
    this.querySelector('.exit').addEventListener('click', () => {
        this.remove();
    });
  </script>
</div>
```

#### Structural Recomposition
Although HTML Transport does not require Scoped HTML, it does provide native support for scoped-IDs where used in the same document. Here, components that define a scope may have their scoped *nodes* replaced on importing them. To replace nodes of an incoming scoped component, we would simply create the replacement nodes inside the import element itself.

This is what happens below where we import an *#alert* component, but with the default *message* node completely replaced with a richer one.

```html
<template is="html-bundle">
  <div namespace="html/alert/success" root>
    <div scoped:id="message" style="color: blue;">...</div>
  </div>
</template>
```

```html
<html-import namespace="html/alert/success">
  <div scoped:id="message" root>
    <div scoped:id="title" style="text-transform: uppercase; font-weight: bold;"></div>
    <div scoped:id="content"></div>
  </div>
</html-import>
```

The final composition would give us:

```html
<div namespace="html/alert/success" root>
  <div scoped:id="message" root style="color: blue;">
    <div scoped:id="title" style="text-transform: uppercase; font-weight: bold;"></div>
    <div scoped:id="content"></div>
  </div>
</div>
```

Notice that *replacement nodes* also inherit properties of their *original nodes*. So before a replacement occurs, any attributes and scoped scripts that may have been defined on the *original node* are composed into the *replacement node*. Thus, our final *message* node above has now rightly inherited a blue color.

**Now a few notes apply:**
* Replacement nodes must be at the root of the import element if they must be discovered.
* The scoped ID of the *replacement node* is what determines the *original node* to be replaced. To replace an *message* node, for example, a replacement node must also define the *message* scoped ID.
* Root-level elements without a `scoped:id` attribute are simply copied to the root of the imported component.

##### Recursive Structural Recomposition
In our code above, we statically typed a *message* replacement node. But it is also possible for a replacement node to be an import of its own.

```html
<template is="html-bundle">

  <div namespace="html/alert/success" root>
    <div scoped:id="message" style="color: blue;">...</div>
  </div>

  <div namespace="html/message" root>
    <div scoped:id="title" style="text-transform: uppercase; font-weight: bold;"></div>
    <div scoped:id="content"></div>
  </div>

</template>
```

```html
<html-import namespace="html/alert/success">
  <html-import namespace="html/message" scoped:id="message"></html-import>
</html-import>
```

The final composition would give us the same result as before:

```html
<div namespace="html/alert/success" root>
  <div scoped:id="message" root style="color: blue;">
    <div scoped:id="title" style="text-transform: uppercase; font-weight: bold;"></div>
    <div scoped:id="content"></div>
  </div>
</div>
```

We have just performed a recursive import as we replaced the *message* node. But with the *message* replacement node being an import of its own, we now also have the power of the import element. For example, We could replace the *title* node on this new level of import.

```html
<html-import namespace="html/alert/success">
  <html-import namespace="html/message" scoped:id="message">
    <div scoped:id="title" style="font-size: larger;"></div>
  </html-import>
</html-import>
```

The final composition would give us the same result as before, but with a replaced *message-title* part:

```html
<div namespace="html/alert/success" root>
  <div scoped:id="message" root style="color: blue;">
    <div scoped:id="title" style="text-transform: uppercase; font-weight: bold; font-size: larger;"></div>
    <div scoped:id="content"></div>
  </div>
</div>
```

We could go even deeper to a third level, and a fourth, and as far as composition requires!

### Inheritance-Based Recomposition
Namespaces in HTML Transport are based on inheritance. Exports in a namespace are believed to be built off their supernamespace. So, a component at `html/content/article/readonly/dark-mode` would be implicitly inheriting properties from the component at `html/content/article/readonly`, which in itself, would be inheriting from `html/content/article`.

By default, only attributes are implicitly inherited. Where ScopedJS is used in the same document, scoped scripts are also automatically inherited. It is also possible for *structural nodes*, to be inherited, where ScopedHTML is used in the same document. But structural inheritance is done explicitly.

Below, we have two *article* components. The first one is the base article component. The second is a derivation of this base component; we simply extended the namespace into a *dark-mode* version.

```html
<template is="html-bundle">

  <!-- Standard, readonly article -->
  <div namespace="html/content/article/readonly" root>

    <div scoped:id="title"></div>
    <div scoped:id="content"></div>

    <script type="text/scoped-js">
      this.scopeTree.content.append('Thanks for reading!');
    </script>

  </div>

  <!-- Dark-mode, readonly article -->
  <div namespace="html/content/article/readonly/dark-mode" style="color: white; background-color: black;">

    <div scoped:id="title"></div>
    <div scoped:id="content"></div>

  </div>

</template>
```

Above, our second component would be implicitly inheriting the *root* attribute and the ScopedJS script. Importing this *dark-mode* component would give us a richly-composed result.

```html
<div namespace="html/content/article/readonly/dark-mode" root style="color: black;">

    <div scoped:id="title"></div>
    <div scoped:id="content"></div>

    <script type="text/scoped-js">
      this.scopeTree.content.append('Thanks for reading!');
    </script>

  </div>
```

Inheritance has, indeed, saved us much repetition! The only aspects we repeated from the base component are the structural parts - *title* and *content*. Even these could be inherited; this time, using an explicit approach. To inherit structural nodes, the namespace extension would be defined as an import.

```html
<template is="html-bundle">

  <html-import namespace="html/content/article/readonly/dark-mode" style="color: white; background-color: black;"></html-import>

</template>
```

Using an `html-import` element now also gves us the power of import-based composition. If we so desired, the inherited structural node could be overridden!

```html
<template is="html-bundle">

  <!-- Dark-mode, readonly article, with a new "content" node  -->
  <html-import namespace="html/content/article/readonly/dark-mode" root style="color: white; background-color: black;">
    <div scoped:id="content" style="border-color:white"></div>
  </html-import>

</template>
```

#### Inheritance and Fallbacks
Since properties are implicitly inherited down a subnamespace path, it would be safe to query a descendant namespace this is really yet to exist. The query would fallback to the closest implemented namespace up the hierarchy. In other words, a non-existent subnamespace also implicitly inherits from the closest implemented supernamespace.

Right below, we're importing a component that is assumed to be built off an *alert* component. But since we're yet to actually implement this subnamespace, the query will be falling back to the standard alert component.

```html
<html-import namespace="html/alert/success/animated"></html-import>
```

Now this really allows us to progressively build features into an app while using a real layout plan from the start.

#### Cross-Bundle Inheritance
When multiple bundles are defined on a document, they are all used in a cascaded manner; the same way multiple CSS stylesheets are used. Namespaces in latter bundles are able to extend namespaces in bundles before.

The derived *dark-mode* article in our code earlier could be in a diffrent bundle. Inheritance would apply as expected, this time, cross-bundle.

```html
<template is="html-bundle">

  <!-- Standard, readonly article -->
  <div namespace="html/content/article/readonly" root>

    <div scoped:id="title"></div>
    <div scoped:id="content"></div>

    <script type="text/scoped-js">
      this.scope.content.append('Thanks for reading!');
    </script>

  </div>

</template>

<template is="html-bundle">

  <!-- Dark-mode, readonly article -->
  <div namespace="html/content/article/readonly/dark-mode" style="color: white; background-color: black;">

    <div scoped:id="title"></div>
    <div scoped:id="content"></div>

  </div>

</template>
```

### Managing Recomposition
As a general rule, everything found on an import element are stripped off and composed into the imported component. Properties are always inherited along a namespace path. But all of this can be tuned. There are two ways.

**Using the Norecompose Attribute:** To object to recomposition on an element, the *norecompose* attribute can be used. If without a value, this attribute disables recomposition completely. Setting its value to `*` achieves the same thing. A list of attribute names may, however, be set to specify the attributes that should be excluded from composition.

```html
<template is="html-bundle">
  <div namespace="html/badge/user" style="color: blue;" norecompose="style"></div>
</template>
```

To exclude scoped scripts from recomposition, the `--scoped-js` expression should be added to the *norecompose* list.

```html
<div namespace="html/badge/user" norecompose="style --scoped-js">

  <script type="text/scoped-js">
    // code
  </script>

</div>
```

The *norecompose* directive may also be set generally for all exports in a bundle.

```html
<template is="html-bundle" norecompose="style --scoped-js">

  <div namespace="component/badge/user" style="color: blue;">

    <script type="text/scoped-js">
      // code
    </script>

  </div>

</template>
```

With a bundle-level *norecompose* directive in place, exports will now be imported without having the listed properties recomposed. An export-level *norecompose* directive could still be set to list additional export-specific properties for exclusion.

## List Composition
With the HTML Transport system and the presence of ScopedHTML and ScopedJS, it is intuitive to make a list! Here, we able to define list containers that can auto-generate their items depending on the given data object or array. Items are automatically imported into, or removed from a list container, to keep in sync with the given data.

First, we would define one reusable item component, then the list container having a two-part namespace that references the reusable item component, with both parts of the namespace separated by two forward slashes.

```html
<template is="html-bundle">

  <li namespace="html/item"></li>

  <ul namespace="html/list"></ul>

</template>
```

Now, we can simply import the list container into the page.

```html
<body>
  <html-import namespace="html/list//html/item"></html-import>
</body>
```

Now when we send in a data array into the list container the ScopedJS way, the list will be auto-populated with items corresponding to the number of entries in the bound data object.

```js
let listContainer = document.querySelector('ul');
listContainer.bind([
    {content: 'item-1'},
    {content: 'item-2'},
]);
```

The auto-populated list should look like this:

```html
<body>
  <ul namespace="html/list//html/item">
    <li namespace="html/item"></li>
    <li namespace="html/item"></li>
  </ul>
</body>
```

A data object would also produce the same result:

```js
listContainer.bind({
    item1: {content: 'item-1'},
    item2: {content: 'item-2'},
});
```

To render individual data entry into the generated items, we would simply add a scoped script to the item definition to do the job.

```html
<template is="html-bundle">

  <li namespace="html/item">
    <script type="text/scoped-js">
      if (content) {
        this.innerHTML = content;
      }
    </script>
  </li>

  <ul namespace="html/list"></ul>

</template>
```

Our list should now have each item rendered:

```html
<body>
  <ul namespace="html/list//html/item">
    <li namespace="html/item">item-1</li>
    <li namespace="html/item">item-2</li>
  </ul>
</body>
```

### Updating a List
As noted earlier, items are automatically imported into, or removed from a list container, to keep in sync with the given data object or array.

To render additional items to our list above, we would simply add extra entries to our data object. Below, we've added two more entries. Since two item elements already exists in the list container, only two new imports will now be made.

```js
listContainer.bind({
    item1: {content: 'item-1'},
    item2: {content: 'item-2'},
    item3: {content: 'item-3'},
    item4: {content: 'item-4'},
});
```

```html
<body>
  <ul namespace="html/list//html/item">
    <li namespace="html/item">item-1</li>
    <li namespace="html/item">item-2</li>
    <li namespace="html/item">item-3</li>
    <li namespace="html/item">item-4</li>
  </ul>
</body>
```

Now, what happens if we reduced the list data to a single entry after having rendered four? The number of item elements would also be reduced!

```js
listContainer.bind({
    item3: {content: 'item-3'},
});
```

```html
<body>
  <ul namespace="html/list//html/item">
    <li namespace="html/item">item-3</li>
  </ul>
</body>
```

And setting an empty array or object will as well empty the list container. Remember, this is a binding contract!

While we have a working list, we have, so far, been reassigning the data object in whole and making the list render from the beginning. To optimize the rendering efficiency, we could assign our data object to the list container once and perform subsequent modifications to the data object in-place.

```js
let dataArray = [
    {content: 'item-1'},
];
// We would assign only once
listContainer.bind(dataArray);

// Then do subsequent modifications in-place
Reflex.set(dataArray, 1, {content: 'item-2'});
Reflex.set(dataArray, 2, {content: 'item-3'});
```

Additionally, with the help of Reflex, we can directly call the data array's prototype methods to achieve the same result.

```javascript
// Charge the array's push(), splice() and unshift() methods with Reflex actions
Reflex.init(dataArray, ['push', 'splice', 'unshift']);
// Add a fourth item
dataArray.push({content: 'item-4'});
// Empty the array
dataArray.splice(0);
// Add a new first item
dataArray.unshift({content: 'new item-1'});
```

To make auto-generated list even cooler, subsequent entries in a data array are rendered in an ordered manner. So even when we modify the array in a random order, imported item elements will be correctly placed on the right index.

```js
Reflex.set(dataArray, 10, {content: 'item-11'});
Reflex.set(dataArray, 8, {content: 'item-9'});
listData.unshift({content: 'newest item-1'});
```

The last item `newest item-1` should still come first, and `item-8` should still be placed before `item-10`:

```html
<ul namespace="html/list">
  <li namespace="html/item">newest item-1</li>
  <li namespace="html/item">new item-1</li>
  <li namespace="html/item">item-8</li>
  <li namespace="html/item">item-10</li>
</ul>
```

### Dynamic Item Namespaces
To import an item in an auto-generated list, the *item* namespace given in the container's two-part namespace is used. But it is possible to dynamically suffix each import's namespace with the current item *index*. So in our list above, the first item could be imported using the namespace `html/item/0`, while the second item could also be imported using the namespace `html/item/1`, and so on. This is achieved by appending an empty pair of square brackets to the namespace template.

```html
<body>
  <ul namespace="html/list//html/item/[]">
    <li namespace="html/item">item-3</li>
  </ul>
</body>
```

The same pattern holds true for data objects. For an object like `{key1: {content: 'item-1'}, key2: {content: 'item-2'}}`, the first item would be imported with `html/item/key1`, and the second, `html/item/key2`.

This makes it possible to create components that are unique to specific indexes, if we so desire. For example, while every item would be imported with the regular `html/item`, we could have a unique implementation for the first item.

```html
<template is="html-bundle">

  <li namespace="html/item"></li>
  <li namespace="html/item/0" style="font-weight: bold;"></li>

  <ul namespace="html/list"></ul>

</template>
```

```html
<body>

  <html-import namespace="html/list//html/item/[]"></html-import>

</body>
```

Item-specific imports can be even more dynamic using dynamic namespace expressions that contain placeholders. Placeholders are references to properties of the given data entry. They are enclosed in square brackets `[]`.

```html
<body>

  <html-import namespace="html/list//html/item/[lang]"></html-import>

</body>
```

The `lang` property in each item below will be used to resolve the namespace for each item imported.

```js
listContainer.bind([
    {lang: 'en', value: 'Hello World!'},
    {lang: 'fr', value: 'Bonjour!'},
]);
```

```html
<template is="html-bundle">

  <li namespace="html/item"></li>
  <li namespace="html/item/en" style="color: blue;"></li>
  <li namespace="html/item/fr" style="color: magenta;"></li>

  <ul namespace="html/list"></ul>

</template>
```

Placeholders could also use the dot (.) notation to reference deep in a the given data entry.

```html
<body>

  <html-import namespace="html/list//html/item/[lang.code]"></html-import>

</body>
```

```js
listContainer.bind([
    {lang: {code: 'en', display: 'English'}, value: 'Hello World!'},
    {lang: {code: 'fr', display: 'French'}, value: 'Bonjour!'},
]);
```

## The HTMLTransport API
HTMLTransport offers certain methods for working with bundles and exports.

### The `HTMLTransport.import()` Static Method
This method simply provides a programmatic way to retrieve an export.

```js
let user = HTMLTransport.import('html/badge/user');
```

### The `HTMLTransport.ready()` Static Method
This method is used to keep code execution in sync with the document’s “ready” state. It accepts a callback that will run when the DOM announces readiness – an event that indicates that the document tree has been initialized and safe to access.

```js
HTMLTransport.ready(() => {
    // Put code here
});
```

This method also extends to wait for remote HTML bundles to load. This is useful when running code that relies on remote bundles. As it is, bundles have to be loaded in order to access their exports. In fact, a mild warning is raised on attempting to import while bundles are still loading.

```js
HTMLTransport.ready(() => {
    let user = HTMLTransport.import('html/badge/user');
});
```

To prevent waiting for bundles, pass `false` as a second argument to this method.

```js
HTMLTransport.ready(() => {
    //...
}, /*waitForBundles*/false);
```

## HTMLTransport Configuration
HTML Transport has been designd to be fully customizable. Simply obtain HTML Transport's `ENV` object and configure its `params` property.

```js
// If HTMLTransport has been loaded via a script tag
let ENV = window.WebNative.HTMLTransport.ENV;

// To use the import method
import {ENV} from '@web-native-js/chtml/src/html-transport/index.js';
// To access the same ENV from the Chtml's ENV
import {ENV as CHTML_ENV} from '@web-native-js/chtml';
let ENV = CHTML_ENV.HTMLTransport;
```

Here are the configuration options.

+ `ENV.params.namespaceAttribute` - (String): The namespace attribute for import and exports. This is `namespace` by default.

+ `ENV.params.bundleElement` - (String): The name of the bundle element, implemented as a *Customized Built-In*. This is `html-bundle` by default.

+ `ENV.params.importElement` - (String): The name of the import element, implemented as a *Custom Element*. This is `html-import` by default.

+ `ENV.params.keyValAttributes` - (Array): Attributes to be treated as *key-value* attributes, in addtion to the `style` attribute. This is empty by default.

+ `ENV.params.listAttributes` - (Array): Attributes to be treated as *list-type* attributes, in addtion to the `class`, `role` attributes (and the `scope`, `parts-hints` attributes in a ScopedHTML-based document). This is empty by default.

+ `ENV.params.norecomposeAttributes` - (Array): Attributes to be excluded during recomposition. This is the `nocompose` attribute itself and the `shadow` attribute, by default.
