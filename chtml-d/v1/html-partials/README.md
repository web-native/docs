# HTML Partials

HTML Partials is a DOM feature that brings the ability to define, extend, import/export reusable HTML snippets using the *template*, *partials*, and *slots* paradigm.

## On this page:
+ [Templates, Partials and Slots](#templates-partials-and-slots)
+ [Slot Properties](#slot-properties)
+ [Nested Templates](#nested-templates)
+ [Remote Templates](#remote-templates)
+ [Partials API](#partials-api)
+ [Isomorphic Rendering](#isomorphic-rendering)

## Templates, Partials and Slots

A *template* is a collection of independent *partials* that can be consumed from anywhere in the main document.

```html
<head>

    <template name="template1">
        <div id="partial-1"></div>
        <div id="partial-2"></div>
    </template>

</head>
```

An element in the main document, called the *implementation block* or the *composition area*, can define *slots*, and then, point to a template to have the template's partials each mapped to a slot. 

```html
<html>

    <head>

        <template name="template1">
            <div id="partial-2" partials-slot="slot-1"></div>
            <div id="partial-2" partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <h2>I have slots</h2>
            <partials-slot name="slot-1"></partials-slot>
            <span>
                <partials-slot name="slot-2"></partials-slot>
            </span>
        </div>

    </body>

</html>
```

Composition takes place and the slots are replaced by the template's partials. The block is said to have *implemented* the template.

```html
<html>

    <head>

        <template name="template1">
            <div id="partial-2" partials-slot="slot-1"></div>
            <div id="partial-2" partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <h2>I have slots</h2>
            <div id="partial-2" partials-slot="slot-1"></div>
            <span>
                <div id="partial-2" partials-slot="slot-2"></div>
            </span>
        </div>

    </body>

</html>
```

An implementation block can implement another template by simply pointing to it; slots are disposed of their previous slotted contents and recomposed from the new template.

The `<partials-slot>` element, even though replaced, is never really destroyed. It returns to its exact position whenever the last of its slotted elements get deleted, or whenever the slot has no corresponding partial in the next implemented template.

A template is to the composition block what the Light DOM of a custom element is to the Shadow DOM - providing *slottable* contents for *slots*; called *slottables* in Web Components, *partials* in CHTML.

HTML Partials also supports *Default Slots*. A template's direct children without an explicit `partials-slot` attribute are slotted into the *Default Slot* in the implementation block.

**Unscoped Slots**

By default, slots are scoped to their containing implementation block. But the `<partials-slot>` element may also be used independent of an implementation block to point to its own template.

```html
<html>

    <head>

        <template name="template1">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

        <template name="template2">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <h2>I have slots</h2>
            <partials-slot name="slot-1"></partials-slot>
            <span>
                <partials-slot name="slot-1" template="template2"></partials-slot>
            </span>
        </div>

        <partials-slot name="slot-2" template="template1"></partials-slot>

    </body>

</html>
```

## Slot Properties

In HTML Partials, slots may be defined with extra properties that a slotted element can inherit. Every element slotted in its place will take on these properties.

Inherittable properties can be both attributes and content.

**Attributes**

A slot's attributes, other than the slot-exclusive `name` and `template` attributes, are inheritted by every slotted element.

On inheriting single-value attributes, like the `id` attribute, any such attribute is replaced on the slotted element. On inheriting space-delimitted attributes, like the `class` attribute, new and non-duplicate values are placed after any existing values. On inheriting key/value attributes, like the `style` attribute, new declarations are placed after any existing declarations, making CSS cascading work on the `style` attribute.

Below, we are using Slot Attributes to recompose the same partial differently for each usecase.

```html
<html>

    <head>

        <template name="template1">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <partials-slot name="slot-1" id="headline" style="color:red"></partials-slot>
        </div>

        <partials-slot name="slot-1" template="template1" style="color:blue"></partials-slot>

    </body>

</html>
```

**Content**

A slot can have default content that renders before slotting takes place. But this content can instead be defined as a new set of *partials* that can be *implemented* by slotted elements. So we have the slot element acting as the *template* and the slotted element as the *implementation block*. (In the light/shadow paradigm, this is the slot element acting as an element's *Light DOM* and the slotted element as its *Shadow DOM*.)

To implement a slot, a *partial* would set its `template` attribute to the keyword *slot* instead of pointing to an actual template.

```html
<html>

    <head>

        <template name="template2">

            <!-- I am a recomposable partial. My ideal slot provides the partials for me -->
            <div partials-slot="slot-1" template="slot">
                <partials-slot name="slot-1-1"></partials-slot>
            </div>
            
            <!-- I am a regular partial -->
            <div partials-slot="slot-2"></div>

        </template>

    </head>

    <body>

        <div template="template1">

            <!-- I am an implementable slot. My ideal partial defines slots -->
            <partials-slot name="slot-1">
                <div partials-slot="slot-1-1"></div>
            </partials-slot>

        </div>

    </body>

</html>
```

## Nested Templates

Templates may be nested for organizational purposes. 

```html
<template name="template1">

    <div slot="slot1"></div>
    <div slot="slot2"></div>

    <template name="nested1">
        <div slot="slot3"></div>
        <div slot="slot4"></div>
    </template>
    <template name="nested2">
        <div slot="slot5"></div>
        <div slot="slot6"></div>
    </template>

</template>
```

Nested templates are referenced using a path notation: 

```html
<div template="template1/nested1">
</div>
```

## Remote Templates

Templates may reference remote content using the `src` attribute.

**Remote file: http://localhost/templates.html**

```html
    <div slot="slot-1"></div>
    <div slot="slot-2"></div>

    <template name="extended">
        <div slot="slot-3"></div>
        <div slot="slot-4"></div>
    </template>
<p></p>
```

**Document: http://localhost**

```html
<head>
    <template name="template1" src="http://localhost/templates.html"></template>
</head>
```

Where remote templates are detected in a document, slots are resolved after all templates have loaded their content. The status of loading content is reflected on the `document.templatesReadyState` property. This property has the initial value of `loading`, and a final value of `complete`, with state change announced on the document object in a `templatesreadystatechange` event.

## Partials API

HTML Partials introduces a few new DOM properties for working with composition.

**For the document object:**

+ `.templatesReadyState` - This property reflects the status of loading templates:
    + `loading` - Templates currently loading.
    + `complete` - Templates done loading, or no remote templates at all. The `templatesreadystatechange` event is fired on the document object.
+ `.templates` - This property represents the list of templates in the document. Templates are exposed here by name. So `document.templates.template1` should return the template element used in the examples above.

**For the <template> element:**

+ `.partials` - This property represents the list of partials defined by the template. It is an object holding a reference to partials by name. Unnamed partials are treated as having the name *default*. So, for the template below,
    
    ```html
    <template name="template1">
        <div slot="one"></div>
        <div slot="two"></div>
        <div slot="default"></div>
        <p></p>
    </template>
    ```

    accessing `document.templates.template1.partials.one` should return an array containing the first `<div>`; while `document.templates.template1.partials.default` should return an array containing the last `<div>` and `<p>`.

+ `.templates` - This property represents the list of templates nested within the template. It is an object holding a reference to templates by name.
    
    ```html
    <template name="template1">
        <template name="nested1"></template>
        <template name="nested2">
            <div slot="one"></div>
        </template>
    </template>
    ```

    accessing `document.templates.template1.templates.nested1` should return the first nested template, while `document.templates.template1.templates.nested2` the second nested template. And the nesting can go on as much as code organization requires.

**For every element:**

+ `.template` - This property represents a copy of the `<template>` element referenced by an element. So if an element implements a template as in `<div template="html/temp"></div>`, then `element.template` should be a copy of the `<template>` at the `module/temp` namespace; `element.template.partials.default` should thus return an array like the above.

**For the <slot> element:**

+ `.slottedElements` - This property represents the list of partials slotted into a slot. (Much like the [`HTMLSlotElement.assignedElements()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSlotElement/assignedElements) method.)
+ `.resolve()` - This method, without arguments, is used to programatically resolve a slot from the appropriate partial in the template given in context.
+ `.empty([silently = false])` - This method is used to programatically empty the slot of its partials, thereby triggering the restoration of the slot element itself. To empty the slot silently without restoring the original slot element, provide `true` on the first parameter.

**For slotted elements:**

+ `.slotReference` - This property gives a reference to the slot element an element was assigned to. (Much like the [`Slottable.assignedSlot`](https://developer.mozilla.org/en-US/docs/Web/API/Slottable/assignedSlot) property.)

## Isomorphic Rendering

One thing with slots-based compostion is the promise of persistent slots - placeholders must never really lose their place. This promise is easy to keep on a live DOM, as slot positions can be easily maintained - even after a slot is replaced - with the use of empty text nodes, for example. Where the challenge lies is when rendering happens on the server and has to be serialized for the browser to take over; replaced slots must have to be *hydratable* by the browser. 

HTML Partials addresses this by serializing slot elements as *comment nodes* with a view to recreating the original slot elements from these comments on getting to the browser. This way, composition is able to continue. For example, deleting a server-slotted element now in the client should trigger the restoration of the original slot element; changing the `template` attribute of the composition block should dispose off all its slotted elements and recompose the block from the new referenced template.

**Before Rendering on the Server**

```html
<html>

    <head>

        <template name="template2">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <partials-slot name="slot-1" id="headline" style="color:red">Default Headline</partials-slot>
        </div>

        <partials-slot template="template1" name="slot-1" style="color:blue"></partials-slot>

    </body>

</html>
```

**After Rendering on the Server**

```html
<html>

    <head>

        <template name="template2">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <div partials-slot="slot-1" id="headline" style="color:red"></div>
            <!-- <partials-slot name="slot-1" id="headline" style="color:red">Default Headline</partials-slot> -->
        </div>

        <div partials-slot="slot-1" style="color:blue"></div>
        <!-- <partials-slot template="template1" name="slot-1" style="color:blue"></partials-slot> -->

    </body>

</html>
```

**Now on the Browser**

Find and delete the server-slotted element with ID `#headline`. The original slot element should now be restored and ready to recieve the next partial.

```html
<html>

    <head>

        <template name="template2">
            <div partials-slot="slot-1"></div>
            <div partials-slot="slot-2"></div>
        </template>

    </head>

    <body>

        <div template="template1">
            <partials-slot name="slot-1" id="headline" style="color:red">Default Headline</partials-slot>
            <!-- <partials-slot name="slot-1" id="headline" style="color:red">Default Headline</partials-slot> -->
        </div>

        <div partials-slot="slot-1" style="color:blue"></div>
        <!-- <partials-slot template="template1" name="slot-1" style="color:blue"></partials-slot> -->

    </body>

</html>
```

**Enabliing Slots Serialization**

Since slots serialization is only necessary for isomorphic pages, this feature is designed to be explicitly turned on with a META tag.

```html
<html>
    <head>
        <meta name="chtml" content="isomorphic=true;" />
    </head>
    <body></body>
</html>
```

\* The trailing semi-colon (;) in the CHTML META tag is optional.
