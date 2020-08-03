# DOM/appendAsync\(\)

This function appends content to an element. It works exactly the same as [`ParentNode.append()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append) except that when the implied content is undefined, it is converted to an empty string.

<<<<<<< HEAD:play-ui/v002/api/dom/appendasync.md
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`appendSync()`](/play-ui/v002/api/dom/appendsync.md). Unlike the _Sync_ counterpart, `appendAsync()` is a promise-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
=======
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`appendSync()`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/dom/appendsync.md). Unlike the _Sync_ counterpart, `appendAsync()` is a promise-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/appendasync.md

## Import

```javascript
import appendAsync from '@web-native-js/play-ui/src/dom/appendAsync.js';
```

## Syntax

```javascript
let promise = appendAsync(el[, ...content);
```

### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `content` - `[String|HTMLElement]`: The set of content to append. Each could be a plain text, an HTML/XML markup, or even a DOM node.

### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

## Usage

```markup
<body></body>
```

```javascript
// Append content
appendAsync(document.body, 'Hello', ' ', 'world', '!').then(body => {
    // Do something with body
});
```

## Implementation Note

<<<<<<< HEAD:play-ui/v002/api/dom/appendasync.md
Technically, DOM operations initiated with `appendAsync()` are internally batched to a _write_ queue using the [Reflow](/play-ui/v002/api/reflow.md) utility. _Read_ operations run first, then _write_ operations. This works to eliminate _layout thrashing_ as discussed in _Reflow_'s documentation.
=======
Technically, DOM operations initiated with `appendAsync()` are internally batched to a _write_ queue using the [Reflow](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/reflow.md) utility. _Read_ operations run first, then _write_ operations. This works to eliminate _layout thrashing_ as discussed in _Reflow_'s documentation.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/appendasync.md

Notice the order of execution in the following example.

```javascript
// Append content
appendAsync(document.body, 'Hello', ' ', 'world', '!').then(() => {
    console.log('Append operation');
});

// Get content
htmlAsync(document.body).then(content => {
    console.log('Current content is: ' + content);
});

// ------------
// console
// ------------
Current content is: 
Append operation
```

The proper way to synchronize with an async function is to move code into its `then()` block as seen below.

```javascript
// Append content
appendAsync(document.body, 'Hello', ' ', 'world', '!').then(() => {
    console.log('Append operation');
    // Get content
    htmlAsync(document.body).then(content => {
        console.log('Current content is: ' + content);
    });
});

// ------------
// console
// ------------
Append operation
Current content is: Hello world!
```

