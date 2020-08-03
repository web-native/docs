# DOM/attrChangeCallback\(\)

This function observes changes on attributes of the given element.

## Import

```javascript
import attrChangeCallback from '@web-native-js/play-ui/src/dom/attrChangeCallback.js';
```

## Syntax

```javascript
attrChangeCallback(el, callback[, filter = []]);
```

### Parameters

* `el` - `HTMLElement`: The specific element to observe.
* `callback` - `Function`: The callback function that recieves the notification. This callback will be called once for every change with the following arguments.
  * `mutation` - `MutationEntry`: An object representing the mutation that occured.
* `filter` - `Array`: \(Optional\) A more specific list of attributes to observe.

<<<<<<< HEAD:play-ui/v002/api/dom/attrchangecallback.md
**Return**
+ `MutationObserver` - The instantiated MutationObserver.
=======
#### Return

* `MutationObserver` - The instantiated MutationObserver.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/attrchangecallback.md

## Usage

```markup
<body>
  <div id="el1" class="cls1"></div>
</body>
```

```javascript
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

