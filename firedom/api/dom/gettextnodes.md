# `DOM/getTextNodes()`
This function finds and returns all text nodes within an element.

## Import

```js
import getTextNodes from '@web-native-js/firedom/src/dom/getTextNodes.js';
```

## Syntax

```js
getTextNodes(el);
```

### Parameters
+ `el` - `HTMLElement`: The source DOM element.

### Return
+ `Array` - The text nodes found.

## Examples

```js
let textNodes = getTextNodes(document.body);
console.log(textNodes); // Array
```