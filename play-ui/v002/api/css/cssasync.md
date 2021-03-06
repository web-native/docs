# CSS/cssAsync\(\)

This function sets or returns one or more style properties for the given element. It is a convenient alternative to [`window.getComputedStyle`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle) and [`ElementCSSInlineStyle.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style). It also has special support for vendor-prefixed properties.

The suffix *Async* differentiates this method from its *Sync* counterpart - [`cssSync()`](/play-ui/v002/api/css/csssync.md). Unlike the *Sync* counterpart, `cssAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import cssAsync from '@web-native-js/play-ui/src/css/cssAsync.js';
```

## Syntax

```javascript
// Method signature
let promise = cssAsync(el, ...args);
```

### &gt; Set/Unset Inline Styles

```javascript
// Set a single inline property
let promise = cssAsync(el, name, value);
// Unset a single inline property
let promise = cssAsync(el, name, '');

// Set multiple single inline properties
let promise = cssAsync(el, {
    name: value,
});
// Unset multiple single inline properties
let promise = cssAsync(el, {
    name: '',
});
```

**Parameters**
+ `el` - `HTMLElement`: The target DOM element.
+ `name` - `String`: The CSS property to set or unset.
+ `value` - `String|Number`: The property value to set. When an empty string `''`, the property is unset from the element's inline CSS.

**Return**
+ `Promise` - A *Promise* that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Get Computed Properties

```javascript
// Get a single computed property
cssSync(el, name[, pseudo = null]).then(value => {
    // Do something with value
});

// Get a multiple computed properties
cssSync(el, [name][, pseudo = null]).then(values => {
    // Do something with values
});
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `name` - `String|Array`: The CSS property or list of properties to read. When an array, values are returnd as an object.
+ `pseudo` - `String`: An optional specifier to read from the element's `before` or `after` pseudo elements.

**Return**
+ `Promise` - A *Promise* that resolves when the operation finally gets executed. The computed CSS values are returned when the promise resolves. The resolved return type is the same return type at [`cssSync() - Return`](/play-ui/v002/api/css/csssync.md#return-1).

## Usage

```markup
<div id="el" style="transform:translate(30, 40); color:red"></div>
```

```javascript
let el = document.querySelector('#el');

// Set attribute
cssSync(el, ['transform', 'color']).then(values => {
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
});
```

## Implementation Note
Technically, DOM operations initiated with `cssAsync()` are internally batched to an appropriate queue using the [Reflow](/play-ui/v002/api/reflow.md) utility. *Read* operations run first, then *write* operations. This works to eliminate *layout thrashing* as discussed in *Reflow*'s documentation.

Notice the order of execution in the following example.

```javascript
// Change color from red to blue
cssAsync(article, 'color', 'blue').then(() => {
    console.log('Color is now blue');
});

// Read the current color
cssAsync(article, 'color').then(value => {
    console.log('Current color: ' + value);
});

// ------------
// console
// ------------
Current color: red
Color is now blue
```

The proper way to synchronize with an async function is to move code into its `then()` block as seen below.

```javascript
// Change color from red to blue
cssAsync(article, 'color', 'blue').then(() => {
    console.log('Color is now blue');
    // Read the current color
    cssAsync(article, 'color').then(value => {
        console.log('Current color: ' + value);
    });
});
// ------------
// console
// ------------
Color is now blue
Current color: blue
```

