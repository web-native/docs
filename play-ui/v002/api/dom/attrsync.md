# `DOM/attrSync()`

This function sets or gets an element's attribute. It is the shorter alternative to [`Element.setAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute), [`Element.getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute), and [`Element.removeAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute). It also has special support for list-based attributes like `class`.

The suffix _Sync_ differentiates this method from its _Async_ counterpart - [`attrAsync()`](/play-ui/v002/api/dom/attrasync.md). Unlike the _Async_ counterpart, `attrSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```javascript
import attrSync from '@web-native-js/play-ui/src/dom/attrSync.js';
```

## Syntax

```javascript
// Method signature
attrSync(el, requestOrPayload[, valOrMutation = null[, subValMutation = null]]);
```

### &gt; Set/Remove Attribute

```javascript
// Set a single attribute
let el = attrSync(el, name, value);
// Remove a single attribute
let el = attrSync(el, name, false);

// Set multiple attributes
let el = attrSync(el, {
    name: value,
});
// Remove multiple attributes
let el = attrSync(el, {
    name: false,
});
```

**Parameters**

* `el` - `HTMLElement`: The target DOM element.
* `name` - `String`: The attribute name to set or remove.
* `value` - `String|Boolean`: The attribute value to set. When `true`, the string `"true"` is set on the attribute. When `false`, the attribute is unset from the element.

**Return**

* `HTMLElement` - The target DOM element.

### &gt; Set/Remove Attribute Entry

```javascript
// Set a single attribute entry
let el = attrSync(el, name, entry, state === true);
// Remove a single attribute entry
let el = attrSync(el, name, entry, state === false);

// Set multiple attribute entries
let el = attrSync(el, {
    name: entry,
}, state === true);
// Remove multiple attribute entries
let el = attrSync(el, {
    name: entry,
}, state === false);
```

**Parameters**

* `el` - `HTMLElement`: The target DOM element.
* `name` - `String`: The attribute name to modify.
* `entry` - `String`: The text to insert or remove.
* `state` - `Boolean`: The indication of _insert_ or _remove_. When `true`, the given text is inserted into the attribute's value list. When `false`, the given text is removed from the attribute's value list.

**Return**

* `HTMLElement` - The target DOM element.

### &gt; Get Attribute

```javascript
let value = attrSync(el, name);
```

**Parameters**

* `el` - `HTMLElement`: The source DOM element.
* `name` - `String`: The attribute name to read.

**Return**

* `String` - The attribute value.

## Usage

```markup
<div class="class1 class2" role="article"></div>
```

```javascript
let article = document.querySelector('.class1');

// Set attribute
attrSync(article, 'role', 'main');

// Insert a class entry
attrSync(article, 'class', 'class3', true);

// Get attribute value
let value = attrSync(article, 'role');
```

