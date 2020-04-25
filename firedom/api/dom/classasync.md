# `DOM/classAsync()`

This function is a convenience function over [`attrAsync()`](/firedom/api/dom/attrasync.md) for adding and removing entries on an element's _class_ attribute.

The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`classSync()`](/firedom/api/dom/classsync.md). Unlike the _Async_ counterpart, `classAsync()` is a promise-based function that runs in a different flow from that of the calling code.

## Import

```javascript
import classAsync from '@web-native-js/firedom/src/dom/classAsync.js';
```

## Syntax

```javascript
// Method signature
let promise = classAsync(el[, valOrMutation = null[, subValMutation = null]]);
```

### &gt; Set/Remove the Class Attribute

```javascript
// Set the class attribute
let el = classAsync(el, classlist);

// Remove the class attribute
let promise = classAsync(el, false);
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `classlist` - `String|Boolean`: Space-delimitted class names. When `true`, the string `"true"` is implied. When `false`, the class attribute is unset from the element.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Add/Remove Class Entry

```javascript
// Set a class entry
let promise = classAsync(el, entry, state === true);
// Remove a class entry
let promise = classAsync(el, entry, state === false);
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `entry` - `String`: The classname to insert or remove.
* `state` - `Boolean`: The indication of _insert_ or _remove_. When `true`, the given classname is inserted into the aelement's classlist. When `false`, the given classname is removed from the aelement's classlist.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Get the Class Attribute

```javascript
let promise = classAsync(el);
```

#### Parameters

* `el` - `HTMLElement`: The source DOM element.

#### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The element's classlist is returned as a string when the promise resolves.

## Examples

```markup
<div class="class1 class2"></div>
```

```javascript
let article = document.querySelector('.class1');

// Insert a class entry
classAsync(article, 'class3', true);

// Get the classlist
classAsync(article).then(classlist => {
    // Do something with classlist
});
```

## Implementation Note

See [`attrAsync()`](/firedom/api/dom/attrasync.md#implementation-note)

