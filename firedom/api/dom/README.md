# The DOM

The set of DOM APIs for traversing and manipulating the DOM.

Below, you will notice Firedom's radical new approach to the DOM that offers a pair of **synchronous** and **asynchronous** methods for operations that affect the browser's _Layout_. The _asynchronous_ methods employ a technique that prevents [Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) to keep the UI at maximum performance. These _asynchronous_ methods are what you want! The _synchronous_ versions would, however, need to go with the [`Reflow`](/firedom/api/reflow.md) DOM abstraction utility to achieve the same level of performance.

Modules in this set can be imported individually or collectively.

```javascript
// Import all modules
import * as DOM from '@web-native-js/firedom/src/dom/index.js';
let select = DOM.select;

// Import a module
import select from '@web-native-js/firedom/src/dom/select.js';
```

## API
+ [`DOM/select()`](/firedom/api/dom/select.md)
+ [`DOM/selectAll()`](/firedom/api/dom/selectall.md)
+ [`DOM/el()`](/firedom/api/dom/el.md)
+ [`DOM/appendSync()`](/firedom/api/dom/appendsync.md)
+ [`DOM/appendAsync()`](/firedom/api/dom/appendasync.md)
+ [`DOM/prependSync()`](/firedom/api/dom/prependsync.md)
+ [`DOM/prependAsync()`](/firedom/api/dom/prependasync.md)
+ [`DOM/htmlSync()`](/firedom/api/dom/htmlsync.md)
+ [`DOM/htmlAsync()`](/firedom/api/dom/htmlasync.md)
+ [`DOM/textSync()`](/firedom/api/dom/textsync.md)
+ [`DOM/textAsync()`](/firedom/api/dom/textasync.md)
+ [`DOM/attrSync()`](/firedom/api/dom/attrsync.md)
+ [`DOM/attrAsync()`](/firedom/api/dom/attrasync.md)
+ [`DOM/classSync()`](/firedom/api/dom/classsync.md)
+ [`DOM/classAsync()`](/firedom/api/dom/classasync.md)
+ [`DOM/data()`](/firedom/api/dom/data.md)
+ [`DOM/getTextNodes()`](/firedom/api/dom/gettextnodes.md)
+ [`DOM/mutationCallback()`](/firedom/api/mutationcallback.md)
+ [`DOM/connectedCallback()`](/firedom/api/connectedcallback.md)
+ [`DOM/disconnectedCallback()`](/firedom/api/disconnectedcallback.md)
+ [`DOM/attrChangeCallback()`](/firedom/api/attrchangecallback.md)
