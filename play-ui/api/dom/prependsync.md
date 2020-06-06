# DOM/prependSync\(\)

This function prepends content to an element. It works exactly the same as [`ParentNode.prepend()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend) except that when the implied content is undefined, it is converted to an empty string.

The suffix _Sync_ differentiates this method from its _Async_ counterpart - [`prependAsync()`](prependasync.md). Unlike the _Async_ counterpart, `prependSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```javascript
import prependSync from '@web-native-js/play-ui/src/dom/prependSync.js';
```

## Syntax

```javascript
prependSync(el[, ...content);
```

### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `content` - `[String|HTMLElement]`: The set of content to prepend. Each could be a plain text, an HTML/XML markup, or even a DOM node.

### Return

* `HTMLElement` - The target DOM element.

## Examples

```markup
<body></body>
```

```javascript
// Prepend content
let body = prependSync(document.body, '!', 'world', ' ', 'Hello');
```

