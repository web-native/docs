# DOM/prependSync\(\)

This function prepends content to an element. It works exactly the same as [`ParentNode.prepend()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend) except that when the implied content is undefined, it is converted to an empty string.

<<<<<<< HEAD:play-ui/v002/api/dom/prependsync.md
The suffix *Sync* differentiates this method from its *Async* counterpart - [`prependAsync()`](/play-ui/v002/api/dom/prependasync.md). Unlike the *Async* counterpart, `prependSync()` is a normal function that runs in the same flow with that of the calling code.
=======
The suffix _Sync_ differentiates this method from its _Async_ counterpart - [`prependAsync()`](prependasync.md). Unlike the _Async_ counterpart, `prependSync()` is a normal function that runs in the same flow with that of the calling code.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/prependsync.md

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

## Usage

```markup
<body></body>
```

```javascript
// Prepend content
let body = prependSync(document.body, '!', 'world', ' ', 'Hello');
```

