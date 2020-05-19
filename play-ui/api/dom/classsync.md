# `DOM/classSync()`

This function is a convenience function over [`attrSync()`](/play-ui/api/dom/attrsync.md) for setting/unsetting the _class_ attribute or adding/removing entries on an element's _class_ list.

The suffix _Sync_ differentiates this method from its _Async_ counterpart - [`classAsync()`](/play-ui/api/dom/classasync.md). Unlike the _Async_ counterpart, `classSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```javascript
import classSync from '@web-native-js/play-ui/src/dom/classSync.js';
```

## Syntax

```javascript
// Method signature
classSync(el[, valOrMutation = null[, subValMutation = null]]);
```

### &gt; Set/Remove the Class Attribute

```javascript
// Set the class attribute
let el = classSync(el, classlist);

// Remove the class attribute
let el = classSync(el, false);
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `classlist` - `String|Boolean`: Space-delimitted class names. When `true`, the string `"true"` is implied. When `false`, the class attribute is unset from the element.

#### Return

* `HTMLElement` - The target DOM element.

### &gt; Add/Remove Class Entry

```javascript
// Set a class entry
let el = classSync(el, entry, state === true);
// Remove a class entry
let el = classSync(el, entry, state === false);
```

#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `entry` - `String`: The classname to insert or remove.
* `state` - `Boolean`: The indication of _insert_ or _remove_. When `true`, the given classname is inserted into the aelement's classlist. When `false`, the given classname is removed from the aelement's classlist.

#### Return

* `HTMLElement` - The target DOM element.

### &gt; Get the Class Attribute

```javascript
let classlist = classSync(el);
```

#### Parameters

* `el` - `HTMLElement`: The source DOM element.

#### Return

* `String` - The element's classlist.

## Examples

```markup
<div class="class1 class2"></div>
```

```javascript
let article = document.querySelector('.class1');

// Insert a class entry
classSync(article, 'class3', true);

// Get the classlist
let classlist = classSync(article);
```

