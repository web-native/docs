# CSS/readStylesheet\(\)

This function returns one or more style properties associated with the given element accross the document's stylesheets. These are properties that have been defined statically and are different from the element's computed style.

## Import

```javascript
import readStylesheet from '@web-native-js/play-ui/src/css/readStylesheet.js';
```

## Syntax

```javascript
// Get a single inline property
let value = readStylesheet(el, name);

// Get a multiple inline properties
let values = readStylesheet(el, [name]);
```

### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `name` - `String|Array`: The CSS property or list of properties to read. When an array, values are returnd as an object.

### Return

* `String|Number` - The value for a single property.
* `Object` - The values for multiple properties.

## Examples

```markup
<style>
div {
    background-color: yellow;
}
</style>
<div id="el" style="color:red"></div>
```

```javascript
let el = document.querySelector('#el');

// Set attribute
let values = readStylesheet(el, ['background-color', 'color']);

// Show
console.log(values);
/**
{
    backgroundColor: "yellow",
    color: undefined,
}
*/
```

