# `readInline()`
This function returns one or more style properties from the given element's style attribute. These are properties that might have been defined statically or dynamically via JavaScript and are different from the element's computed style.

## Import

```js
import readInline from '@web-native-js/play-ui/src/css/readInline.js';
```

## Syntax

```js
// Get a single inline property
let value = readInline(el, name);

// Get a multiple inline properties
let values = readInline(el, [name]);
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `name` - `String|Array`: The CSS property or list of properties to read. When an array, values are returnd as an object.

**Return**
+ `String|Number` - The value for a single property.
+ `Object` - The values for multiple properties.

## Usage

```html
<style>
div {
    background-color: yellow;
}
</style>
<div id="el" style="color:red"></div>
```

```js
let el = document.querySelector('#el');

// Set attribute
let values = readInline(el, ['background-color', 'color']);

// Show
console.log(values);
/**
{
    backgroundColor: undefined,
    color: "red",
}
*/
```