# The DOM

The set of DOM APIs for traversing and manipulating the DOM.

Below, you will notice PlayUI's radical new approach to the DOM that offers a pair of **synchronous** and **asynchronous** methods for operations that affect the browser's _Layout_. The _asynchronous_ methods employ a technique that prevents [Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) to keep the UI at maximum performance. These _asynchronous_ methods are what you want! The _synchronous_ versions would, however, need to go with the [`Reflow`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/reflow.md) DOM abstraction utility to achieve the same level of performance.

Modules in this set can be imported individually or collectively.

```javascript
// Import all modules
import * as DOM from '@web-native-js/play-ui/src/dom/index.js';
let select = DOM.select;

// Import a module
import select from '@web-native-js/play-ui/src/dom/select.js';
```

## API

* [`DOM/select()`](select.md)
* [`DOM/selectAll()`](selectall.md)
* [`DOM/el()`](el.md)
* [`DOM/appendSync()`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/dom/appendsync.md)
* [`DOM/appendAsync()`](appendasync.md)
* [`DOM/prependSync()`](prependsync.md)
* [`DOM/prependAsync()`](prependasync.md)
* [`DOM/htmlSync()`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/dom/htmlsync.md)
* [`DOM/htmlAsync()`](htmlasync.md)
* [`DOM/textSync()`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/dom/textsync.md)
* [`DOM/textAsync()`](textasync.md)
* [`DOM/attrSync()`](attrsync.md)
* [`DOM/attrAsync()`](attrasync.md)
* [`DOM/classSync()`](classsync.md)
* [`DOM/classAsync()`](classasync.md)
* [`DOM/data()`](data.md)
* [`DOM/getTextNodes()`](gettextnodes.md)
* [`DOM/mutationCallback()`](mutationcallback.md)
* [`DOM/connectedCallback()`](connectedcallback.md)
* [`DOM/disconnectedCallback()`](disconnectedcallback.md)
* [`DOM/attrChangeCallback()`](attrchangecallback.md)

