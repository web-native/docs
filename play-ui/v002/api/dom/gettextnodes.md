# DOM/getTextNodes\(\)

This function finds and returns all text nodes within an element.

## Import

```javascript
import getTextNodes from '@web-native-js/play-ui/src/dom/getTextNodes.js';
```

## Syntax

```javascript
getTextNodes(el);
```

### Parameters

* `el` - `HTMLElement`: The source DOM element.

### Return

* `Array` - The text nodes found.

## Usage

```javascript
let textNodes = getTextNodes(document.body);
console.log(textNodes); // Array
```

