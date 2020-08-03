# The DOM

The set of DOM APIs for traversing and manipulating the DOM.

Below, you will notice PlayUI's radical new approach to the DOM that offers a pair of **synchronous** and **asynchronous** methods for operations that affect the browser's _Layout_. The _asynchronous_ methods employ a technique that prevents [Layout Thrashing](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing) to keep the UI at maximum performance. These _asynchronous_ methods are what you want! The _synchronous_ versions would, however, need to go with the [`Reflow`](/play-ui/v002/api/reflow.md) DOM abstraction utility to achieve the same level of performance.

Modules in this set can be imported individually or collectively.

```javascript
// Import all modules
import * as DOM from '@web-native-js/play-ui/src/dom/index.js';
let select = DOM.select;

// Import a module
import select from '@web-native-js/play-ui/src/dom/select.js';
```

## API
+ [`DOM/select()`](/play-ui/v002/api/dom/select.md)
+ [`DOM/selectAll()`](/play-ui/v002/api/dom/selectall.md)
+ [`DOM/el()`](/play-ui/v002/api/dom/el.md)
+ [`DOM/appendSync()`](/play-ui/v002/api/dom/appendsync.md)
+ [`DOM/appendAsync()`](/play-ui/v002/api/dom/appendasync.md)
+ [`DOM/prependSync()`](/play-ui/v002/api/dom/prependsync.md)
+ [`DOM/prependAsync()`](/play-ui/v002/api/dom/prependasync.md)
+ [`DOM/htmlSync()`](/play-ui/v002/api/dom/htmlsync.md)
+ [`DOM/htmlAsync()`](/play-ui/v002/api/dom/htmlasync.md)
+ [`DOM/textSync()`](/play-ui/v002/api/dom/textsync.md)
+ [`DOM/textAsync()`](/play-ui/v002/api/dom/textasync.md)
+ [`DOM/attrSync()`](/play-ui/v002/api/dom/attrsync.md)
+ [`DOM/attrAsync()`](/play-ui/v002/api/dom/attrasync.md)
+ [`DOM/classSync()`](/play-ui/v002/api/dom/classsync.md)
+ [`DOM/classAsync()`](/play-ui/v002/api/dom/classasync.md)
+ [`DOM/data()`](/play-ui/v002/api/dom/data.md)
+ [`DOM/getTextNodes()`](/play-ui/v002/api/dom/gettextnodes.md)
+ [`DOM/mutationCallback()`](/play-ui/v002/api/dom/mutationcallback.md)
+ [`DOM/connectedCallback()`](/play-ui/v002/api/dom/connectedcallback.md)
+ [`DOM/disconnectedCallback()`](/play-ui/v002/api/dom/disconnectedcallback.md)
+ [`DOM/attrChangeCallback()`](/play-ui/v002/api/dom/attrchangecallback.md)
