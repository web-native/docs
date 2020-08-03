<<<<<<< HEAD:play-ui/v002/api/dom/disconnectedcallback.md
# `DOM/disconnectedCallback()`
This function observes when the specified elements are removed from the document or given context. This is a convenience function for [`mutationCallback()`](/play-ui/v002/api/dom/mutationcallback.md)
=======
# DOM/disconnectedCallback\(\)

This function observes when the specified elements are removed from the document or given context. This is a convenience function for [`mutationCallback()`](mutationcallback.md)
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/disconnectedcallback.md

## Import

```javascript
import disconnectedCallback from '@web-native-js/play-ui/src/dom/disconnectedCallback.js';
```

## Syntax

```javascript
disconnectedCallback(els, callback[, context = null[, observeIndirectMutation = true]]);
```

### Parameters

* `els` - `HTMLElement|String|Array`: The specific element or elements to observe. This could be a single element instance, a CSS selector or an array of element specifiers.
* `callback` - `Function`: The callback function that recieves the notification. This callback will recieve the following arguments.
  * `nodes` - `[HTMLElement]`: A variadic list of the elements removed from the DOM.
* `context` - `DOMDocument|HTMLElement`: \(Optional\) The subtree to observe. This is the _document_ itself by default.
* `observeIndirectMutation` - `Boolean`: \(Optional\) A specifier that tells whether to watch direct or indirect mutations of the specified elements.

<<<<<<< HEAD:play-ui/v002/api/dom/disconnectedcallback.md
**Return**
+ `MutationObserver` - The instantiated MutationObserver.
=======
#### Return

* `MutationObserver` - The instantiated MutationObserver.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/disconnectedcallback.md

## Usage

```markup
<body>
  <div id="el1"></div>
  <div id="el2"></div>
</body>
```

```javascript
// Obtain an element instance
let el1 = document.querySelector('#el1');

// Observe both by instance and by selector
disconnectedCallback([el1, '#el2'], (...nodes) => {
    console.log('These nodes are now removed from the document.', ...nodes);
});

// Start mutating
setTimeout(() => {
    el1.remove(); // These nodes are now removed from the document. #el1
    setTimeout(() => {
        document.querySelector('#el2').remove(); // These nodes are now removed from the document. #el2
    }, 1000);
}, 1000);
```

