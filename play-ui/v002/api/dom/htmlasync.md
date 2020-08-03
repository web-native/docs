# DOM/htmlAsync\(\)

This function sets or gets an element's HTML/XML content. It is the programmatic alternative to [`Element.innerHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML). Additionally, when this function receives `undefined` for a _set_ operation, an empty string will be used instead.

<<<<<<< HEAD:play-ui/v002/api/dom/htmlasync.md
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`htmlSync()`](/play-ui/v002/api/dom/htmlsync.md). Unlike the _Sync_ counterpart, `htmlAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
=======
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`htmlSync()`](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/dom/htmlsync.md). Unlike the _Sync_ counterpart, `htmlAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/htmlasync.md

## Import

```javascript
import htmlAsync from '@web-native-js/play-ui/src/dom/htmlAsync.js';
```

## Syntax

```javascript
// Method signature
let promise = htmlAsync(el[, content = null]);
```

### &gt; Set Content

```javascript
let promise = htmlAsync(el, content);
```

**Parameters**

* `el` - `HTMLElement`: The target DOM element.
* `content` - `String|HTMLElement`: The content to set. This could be a plain text, an HTML/XML markup, or even a DOM node.

**Return**

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Get Content

```javascript
let promise = htmlAsync(el);
```

**Parameters**

* `el` - `HTMLElement`: The source DOM element.

**Return**

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The source element's content is returned when the promise resolves.

## Usage

```markup
<body>
  <div>DIV1</div>
</body>
```

```javascript
// Set content
htmlAsync(document.body, '<main><div>DIV1</div></main>').then(body => {
    // Do something with body
});

// Get content
htmlAsync(document.body).then(content => {
    // Do something with content
});
```

## Implementation Note
<<<<<<< HEAD:play-ui/v002/api/dom/htmlasync.md
Technically, DOM operations initiated with `htmlAsync()` are internally batched to an appropriate queue using the [Reflow](/play-ui/v002/api/reflow.md) utility. *Read* operations run first, then *write* operations. This works to eliminate *layout thrashing* as discussed in *Reflow*'s documentation.
=======

Technically, DOM operations initiated with `htmlAsync()` are internally batched to an appropriate queue using the [Reflow](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/reflow.md) utility. _Read_ operations run first, then _write_ operations. This works to eliminate _layout thrashing_ as discussed in _Reflow_'s documentation.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/dom/htmlasync.md

Notice the order of execution in the following example.

```javascript
// Set content
htmlAsync(document.body, 'Hi').then(() => {
    console.log('Set operation 1');
});

// Get content
htmlAsync(document.body).then(content => {
    console.log('Get operation 1');
});

// Set content
htmlAsync(document.body, 'Hi again').then(() => {
    console.log('Set operation 2');
});

// Get content
htmlAsync(document.body).then(content => {
    console.log('Get operation 2');
});

// ------------
// console
// ------------
Get operation 1
Get operation 2
Set operation 1
Set operation 2
```

The proper way to synchronize with an async function is to move code into its `then()` block as seen below.

```javascript
// Set content
htmlAsync(document.body, 'Hi').then(body => {
    console.log('Set operation 1');
    // Get content
    htmlAsync(body).then(content => {
        console.log('Get operation 1');
    });
});

// ------------
// console
// ------------
Set operation 1
Get operation 1
```
