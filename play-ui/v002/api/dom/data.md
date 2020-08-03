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

**Parameters**
+ `el` - `HTMLElement`: The target DOM element.
+ `key` - `String`: The key to set or unset.
+ `value` - `Any`: The data value to set. When `undefined`, the data key is unset from the element.

**Return**
+ `HTMLElement` - The target DOM element.

### &gt; Get Value

```javascript
let value = data(el, key);
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `key` - `String`: The data key to read.

**Return**
+ `Any` - The data value.

## Usage

```javascript
// Set
data(document.body, 'two', 2);

// Get
console.log(data(document.body, 'two')); // 2
```

