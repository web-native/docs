# `DOM/mutationCallback()`
This function observes when the specified elements are added to, or removed from the document or given context. 

## Import

```js
import mutationCallback from '@web-native-js/firedom/src/dom/mutationCallback.js';
```

## Syntax

```js
mutationCallback(els, callback[, connectedState = null[, context = null[, observeIndirectMutation = true]]]);
```

### Parameters
+ `els` - `HTMLElement|String|Array`: The specific element or elements to observe. This could be a single element instance, a CSS selector or an array of element specifiers.
+ `callback` - `Function`: The callback function that recieves the notification. This callback will recieve the following arguments.
    + `connectedState` - `Int`: The type of mutation that occured. This could be either `1` or `0`, indicating whether the elements were added to the DOM or removed.
    + `nodes` - `[HTMLElement]`: A variadic list of the elements added or removed from the DOM.
+ `connectedState` - `Int`: (Optional) The mutation type to watch for. This could be either `1` or `0`, indicating whether to watch only additions to the DOM or removals.
+ `context` - `DOMDocument|HTMLElement`: (Optional) The subtree to observe. This is the *document* itself by default.
+ `observeIndirectMutation` - `Boolean`: (Optional) A specifier that tells whether to watch direct or indirect mutations of the specified elements.

#### Return
+ `MutationObserver` - The instantiated MutationObserver.

## Examples

```html
<body>
  <div id="el1"></div>
  <div id="el2"></div>
</body>
```

```js
// Obtain an element instance
let el1 = document.querySelector('#el1');

// Observe both by instance and by selector
mutationCallback([el1, '#el2', '#el3'], (connectedState, ...nodes) => {
    if (connectedState) {
        console.log('These nodes are now added to the document.', ...nodes);
    } else {
        console.log('These nodes are now removed from the document.', ...nodes);
    }
});

// Start mutating
setTimeout(() => {
    el1.remove(); // These nodes are now removed from the document. el1
    setTimeout(() => {
        document.querySelector('#el2').remove(); // These nodes are now removed from the document. #el2
        setTimeout(() => {
            document.body.append('<div id="el3"></div>'); // These nodes are now added to the document. #el3
        }, 1000);
    }, 1000);
}, 1000);
```