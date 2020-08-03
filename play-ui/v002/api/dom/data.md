# DOM/data\(\)

This function sets or gets custom data/values associated with the given element.

## Import

```javascript
import data from '@web-native-js/play-ui/src/dom/data.js';
```

## Syntax

```javascript
data(el, requestOrPayload[, val = null]);
```

### &gt; Set/Unset Value

```javascript
// Set a single value
let el = data(el, key, value);
// Unset a data key
let el = data(el, key, undefined);

// Set multiple values
let el = data(el, {
    key: value,
});
// Unset multiple data keys
let el = data(el, {
    key: undefined,
});
```

<<<<<<< HEAD:play-ui/v002/api/dom/data.md
**Parameters**
+ `el` - `HTMLElement`: The target DOM element.
+ `key` - `String`: The key to set or unset.
+ `value` - `Any`: The data value to set. When `undefined`, the data key is unset from the element.

**Return**
+ `HTMLElement` - The target DOM element.
=======
#### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `key` - `String`: The key to set or unset.
* `value` - `Any`: The data value to set. When `undefined`, the data key is unset from the element.

#### Return

* `HTMLElement` - The target DOM element.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/data.md

### &gt; Get Value

```javascript
let value = data(el, key);
```

<<<<<<< HEAD:play-ui/v002/api/dom/data.md
**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `key` - `String`: The data key to read.

**Return**
+ `Any` - The data value.
=======
#### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `key` - `String`: The data key to read.

#### Return

* `Any` - The data value.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/data.md

## Usage

```javascript
// Set
data(document.body, 'two', 2);

// Get
console.log(data(document.body, 'two')); // 2
```

