# `DOM/prependAsync()`

This function prepends content to an element. It works exactly the same as [`ParentNode.prepend()`](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend) except that when the implied content is undefined, it is converted to an empty string.

The suffix *Async* differentiates this method from its *Sync* counterpart - [`prependSync()`](/firedom/api/dom/prependsync.md). Unlike the *Sync* counterpart, `prependAsync()` is a promise-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import prependAsync from '@web-native-js/firedom/src/dom/prependAsync.js';
```

## Syntax

```javascript
let promise = prependAsync(el[, ...content);
```

### Parameters

* `el` - `HTMLElement`: The target DOM element.
* `content` - `[String|HTMLElement]`: The set of content to prepend. Each could be a plain text, an HTML/XML markup, or even a DOM node.

### Return

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

## Examples

```markup
<body></body>
```

```javascript
// Prepend content
prependAsync(document.body, '!', 'world', ' ', 'Hello').then(body => {
    // Do something with body
});
```

## Implementation Note
Technically, DOM operations initiated with `prependAsync()` are internally batched to a *write* queue using the [Reflow](/firedom/api/reflow.md) utility. *Read* operations run first, then *write* operations. This works to eliminate *layout thrashing* as discussed in *Reflow*'s documentation.

Notice the order of execution in the following example.

```javascript
// Prepend content
prependAsync(document.body, '!', 'world', ' ', 'Hello').then(() => {
    console.log('Prepend operation');
});

// Get content
htmlAsync(document.body).then(content => {
    console.log('Current content is: ' + content);
});

// ------------
// console
// ------------
Current content is: 
Prepend operation
```

The proper way to synchronize with an async function is to move code into its `then()` block as seen below.

```javascript
// Prepend content
prependAsync(document.body, '!', 'world', ' ', 'Hello').then(() => {
    console.log('Prepend operation');
    // Get content
    htmlAsync(document.body).then(content => {
        console.log('Current content is: ' + content);
    });
});

// ------------
// console
// ------------
Prepend operation
Current content is: Hello world!
```

