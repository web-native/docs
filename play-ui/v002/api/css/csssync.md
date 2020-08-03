# CSS/cssSync\(\)

This function sets or returns one or more style properties for the given element. It is a convenient alternative to [`window.getComputedStyle`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle) and [`ElementCSSInlineStyle.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style). It also has special support for vendor-prefixed properties.

<<<<<<< HEAD:play-ui/v002/api/css/csssync.md
The suffix *Sync* differentiates this method from its *Async* counterpart - [`cssAsync()`](/play-ui/v002/api/css/cssasync.md). Unlike the *Async* counterpart, `cssSync()` is a normal function that runs in the same flow with that of the calling code.
=======
The suffix _Sync_ differentiates this method from its _Async_ counterpart - [`cssAsync()`](cssasync.md). Unlike the _Async_ counterpart, `cssSync()` is a normal function that runs in the same flow with that of the calling code.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/csssync.md

## Import

```javascript
import cssSync from '@web-native-js/play-ui/src/css/cssSync.js';
```

## Syntax

```javascript
// Method signature
cssSync(el, ...args);
```

### &gt; Set/Unset Inline Styles

```javascript
// Set a single inline property
let el = cssSync(el, name, value);
// Unset a single inline property
let el = cssSync(el, name, '');

// Set multiple inline properties
let el = cssSync(el, {
    name: value,
});
// Unset multiple inline properties
let el = cssSync(el, {
    name: '',
});
```

<<<<<<< HEAD:play-ui/v002/api/css/csssync.md
**Parameters**
+ `el` - `HTMLElement`: The target DOM element.
+ `name` - `String`: The CSS property to set or unset.
+ `value` - `String|Number`: The property value to set. When an empty string `''`, the property is unset from the element's inline CSS.

**Return**
+ `HTMLElement` - The target DOM element.
=======
#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `name` - `String`: The CSS property to set or unset.
* `value` - `String|Number`: The property value to set. When an empty string `''`, the property is unset from the element's inline CSS.

#### Return

* `HTMLElement` - The target DOM element.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/csssync.md

### &gt; Get Computed Properties

```javascript
// Get a single computed property
let value = cssSync(el, name[, pseudo = null]);

// Get a multiple computed properties
let values = cssSync(el, [name][, pseudo = null]);
```

<<<<<<< HEAD:play-ui/v002/api/css/csssync.md
**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `name` - `String|Array`: The CSS property or list of properties to read. When an array, values are returnd as an object.
+ `pseudo` - `String`: An optional specifier to read from the element's `before` or `after` pseudo elements.

**Return**
+ `String|Number|Object` - The value for a single property. An object is returned specially for the `transform` rule. This object is easily stringifiable with its `toString()` method.
+ `Object` - The values for multiple properties.
=======
#### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `name` - `String|Array`: The CSS property or list of properties to read. When an array, values are returnd as an object.
* `pseudo` - `String`: An optional specifier to read from the element's `before` or `after` pseudo elements.

#### Return

* `String|Number|Object` - The value for a single property. An object is returned specially for the `transform` rule. This object is easily stringifiable with its `toString()` method.
* `Object` - The values for multiple properties.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/csssync.md

## Usage

```markup
<div id="el" style="transform:translate(30, 40); color:red"></div>
```

```javascript
let el = document.querySelector('#el');

// Set attribute
let values = cssSync(el, ['transform', 'color']);

// Show
console.log(values);
/**
{
    transform: {
        translate: [30, 40],
    },
    color: "red",
}
*/

// Stringify transform
console.log(values.transform + '');
// translate(30, 40)
```

