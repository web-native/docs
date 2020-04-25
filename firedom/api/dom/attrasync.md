# `DOM/attrAsync()`

This function sets or gets an element's attribute. It is the shorter alternative to [`Element.setAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute), [`Element.getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute), and [`Element.removeAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute). It also has special support for list-based attributes like `class`.

The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`attrSync()`](/firedom/api/dom/attrsync.md). Unlike the _Sync_ counterpart, `attrAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import attrAsync from '@web-native-js/firedom/src/dom/attrAsync.js';
```

## Syntax

```javascript
// Method signature
let promise = attrAsync(el, requestOrPayload[, valOrMutation = null[, subValMutation = null]]);
```

### &gt; Set/Remove Attribute

```javascript
// Set a single attribute
let promise = attrAsync(el, name, value);
// Remove a single attribute
let promise = attrAsync(el, name, false);

// Set multiple attributes
let promise = attrAsync(el, {
    name: value,
});
// Remove multiple attributes
let promise = attrAsync(el, {
    name: false,
});
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `name` - `String`: The attribute name to set or remove.
* `value` - `String|Boolean`: The attribute value to set. When `true`, the string `"true"` is set on the attribute. When `false`, the attribute is unset from the element.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Set/Remove Attribute Entry

```javascript
// Set a single attribute entry
let promise = attrAsync(el, name, entry, state === true);
// Remove a single attribute entry
let promise = attrAsync(el, name, entry, state === false);

// Set multiple attribute entries
let promise = attrAsync(el, {
    name: entry,
}, state === true);
// Remove multiple attribute entries
let promise = attrAsync(el, {
    name: entry,
}, state === false);
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `name` - `String`: The attribute name to modify.
* `entry` - `String`: The text to insert or remove.
* `state` - `Boolean`: The indication of _insert_ or _remove_. When `true`, the given text is inserted into the attribute's value list. When `false`, the given text is removed from the attribute's value list.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Get Attribute

```javascript
let promise = attrAsync(el, name);
```

#### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `name` - `String`: The attribute name to read.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target attribute's value is returned when the promise resolves.

## Examples

```markup
<div class="class1 class2" role="article"></div>
```

```javascript
let article = document.querySelector('.class1');

// Set attribute
attrAsync(article, 'role', 'main').then(article => {
    // Do something with article
});

// Insert a class entry
attrAsync(article, 'class', 'class3', true).then(article => {
    // Do something with article
});

// Get attribute value
attrAsync(article, 'role').then(value => {
    // Do something with value
});
```

## Implementation Note

Technically, DOM operations initiated with `attrAsync()` are internally batched to an appropriate queue using the [Reflow](/firedom/api/reflow.md) utility. _Read_ operations run first, then _write_ operations. This works to eliminate _layout thrashing_ as discussed in _Reflow_'s documentation.

Notice the order of execution in the following example.

```javascript
// Remove class1
attrAsync(article, 'class', 'class1', false).then(() => {
    console.log('class1 removed');
});

// Read the class attribute
attrAsync(article, 'class').then(value => {
    console.log('classList: ' + value);
});

// Remove class2
attrAsync(article, 'class', 'class2', false).then(() => {
    console.log('class2 removed');
});

// Read the class attribute
attrAsync(article, 'class').then(value => {
    console.log('classList: ' + value);
});

// ------------
// console
// ------------
classList: class1 class2
classList: class1 class2
class1 removed
class2 removed
```

The proper way to synchronize with an async function is to move code into its `then()` block as seen below.

```javascript
// Remove class2
attrAsync(article, 'class', 'class2', false).then(article => {
    console.log('class2 removed');
    // Read the class attribute
    attrAsync(article, 'class').then(value => {
        console.log('classList: ' + value);
    });
});
// ------------
// console
// ------------
class2 removed
classList: class1
```

