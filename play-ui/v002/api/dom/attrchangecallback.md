# `DOM/attrChangeCallback()`
This function observes changes on attributes of the given element.

## Import

```js
import attrChangeCallback from '@web-native-js/play-ui/src/dom/attrChangeCallback.js';
```

## Syntax

```js
attrChangeCallback(el, callback[, filter = []]);
```

### Parameters
+ `el` - `HTMLElement`: The specific element to observe.
+ `callback` - `Function`: The callback function that recieves the notification. This callback will be called once for every change with the following arguments.
    + `mutation` - `MutationEntry`: An object representing the mutation that occured.
+ `filter` - `Array`: (Optional) A more specific list of attributes to observe.

**Return**
+ `MutationObserver` - The instantiated MutationObserver.

## Usage

```html
<body>
  <div id="el1" class="cls1"></div>
</body>
```

```js
// Obtain an element instance
let el1 = document.querySelector('#el1');

// Observe
attrChangeCallback(el1, mutation => {
    console.log('The class attribute has changed.', mutation);
});

// Start mutating
setTimeout(() => {
    el1.classList.add('cls2'); // The class attribute has changed.
}, 1000);
```