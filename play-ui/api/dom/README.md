# The DOM

The set of DOM APIs for traversing and manipulating the DOM.

Below, you will notice PlayUI's radical new approach to the DOM that offers a pair of **synchronous** and **asynchronous** methods for operations that affect the browser's _Layout_. The _asynchronous_ methods employ a technique that prevents [Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) to keep the UI at maximum performance. These _asynchronous_ methods are what you want! The _synchronous_ versions would, however, need to go with the [`Reflow`](/play-ui/api/reflow.md) DOM abstraction utility to achieve the same level of performance.

Modules in this set can be imported individually or collectively.

```javascript
// Import all modules
import * as DOM from '@web-native-js/play-ui/src/dom/index.js';
let select = DOM.select;

// Import a module
import select from '@web-native-js/play-ui/src/dom/select.js';
```

## API
+ [`DOM/select()`](/play-ui/api/dom/select.md)
+ [`DOM/selectAll()`](/play-ui/api/dom/selectall.md)
+ [`DOM/el()`](/play-ui/api/dom/el.md)
+ [`DOM/appendSync()`](/play-ui/api/dom/appendsync.md)
+ [`DOM/appendAsync()`](/play-ui/api/dom/appendasync.md)
+ [`DOM/prependSync()`](/play-ui/api/dom/prependsync.md)
+ [`DOM/prependAsync()`](/play-ui/api/dom/prependasync.md)
+ [`DOM/htmlSync()`](/play-ui/api/dom/htmlsync.md)
+ [`DOM/htmlAsync()`](/play-ui/api/dom/htmlasync.md)
+ [`DOM/textSync()`](/play-ui/api/dom/textsync.md)
+ [`DOM/textAsync()`](/play-ui/api/dom/textasync.md)
+ [`DOM/attrSync()`](/play-ui/api/dom/attrsync.md)
+ [`DOM/attrAsync()`](/play-ui/api/dom/attrasync.md)
+ [`DOM/classSync()`](/play-ui/api/dom/classsync.md)
+ [`DOM/classAsync()`](/play-ui/api/dom/classasync.md)
+ [`DOM/data()`](/play-ui/api/dom/data.md)
+ [`DOM/getTextNodes()`](/play-ui/api/dom/gettextnodes.md)
+ [`DOM/mutationCallback()`](/play-ui/api/dom/mutationcallback.md)
+ [`DOM/connectedCallback()`](/play-ui/api/dom/connectedcallback.md)
+ [`DOM/disconnectedCallback()`](/play-ui/api/dom/disconnectedcallback.md)
+ [`DOM/attrChangeCallback()`](/play-ui/api/dom/attrchangecallback.md)
