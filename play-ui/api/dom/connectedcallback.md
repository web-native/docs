# `DOM/connectedCallback()`
This function observes when the specified elements are added to the document or given context. This is a convenience function for [`mutationCallback()`](/play-ui/api/dom/mutationcallback.md)

## Import

```js
import connectedCallback from '@web-native-js/play-ui/src/dom/connectedCallback.js';
```

## Syntax

```js
connectedCallback(els, callback[, context = null[, observeIndirectMutation = true]]);
```

### Parameters
+ `els` - `HTMLElement|String|Array`: The specific element or elements to observe. This could be a single element instance, a CSS selector or an array of element specifiers.
+ `callback` - `Function`: The callback function that recieves the notification. This callback will recieve the following arguments.
    + `nodes` - `[HTMLElement]`: A variadic list of the elements added to the DOM.
+ `context` - `DOMDocument|HTMLElement`: (Optional) The subtree to observe. This is the *document* itself by default.
+ `observeIndirectMutation` - `Boolean`: (Optional) A specifier that tells whether to watch direct or indirect mutations of the specified elements.

#### Return
+ `MutationObserver` - The instantiated MutationObserver.

## Examples

```html
<body>
  <div id="el1"></div>
</body>
```

```js
// Obtain an element instance
let el1 = document.querySelector('#el1');

// Observe both by instance and by selector
connectedCallback([el1, '#el2'], (...nodes) => {
    console.log('These nodes are now added to the document.', ...nodes);
});

// Start mutating
el1.remove();
setTimeout(() => {
    document.body.appendChild(el1); // These nodes are now added to the document. #el1
    setTimeout(() => {
        document.body.append('<div id="el2"></div>'); // These nodes are now added to the document. #el2
    }, 1000);
}, 1000);
```