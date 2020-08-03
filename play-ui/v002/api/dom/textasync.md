# DOM/textAsync\(\)

The suffix *Async* differentiates this method from its *Sync* counterpart - [`textSync()`](/play-ui/v002/api/dom/textsync.md). Unlike the *Sync* counterpart, `textAsync()` is a promise-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import textAsync from '@web-native-js/play-ui/src/dom/textAsync.js';
```

## Syntax

```javascript
let promise = textAsync(el[, content = null);
```

### &gt; Set Text Content

```javascript
let promise = textAsync(el, content);
```

**Parameters**

* `el` - `HTMLElement`: The target DOM element.
* `content` - `String`: The text content to set.

**Return**

* `Promise` - A _Promise_ that resolves when the operation finally gets executed. The target DOM element is returned when the promise resolves.

### &gt; Get Text Content

```javascript
textAsync(el).then(content => {
    // Do something with content
});
```

**Parameters**

**el** - `HTMLElement`: The source DOM element.

**Return**

`Promise` - A _Promise_ that resolves when the operation finally gets executed. The source element's text content is returned when the promise resolves.

## Usage

```markup
<body>
  <div>DIV1</div>
</body>
```

```javascript
// Set content
textAsync(document.body, 'Hello world!').then(body => {
    // Do something with body
});

// Get content
textAsync(document.body).then(content => {
    // Do something with content
});
```

## Implementation Note
Technically, DOM operations initiated with `textAsync()` are internally batched to an appropriate queue using the [Reflow](/play-ui/v002/api/reflow.md) utility. *Read* operations run first, then *write* operations. This works to eliminate *layout thrashing* as discussed in *Reflow*'s documentation.

Notice the order of execution in the following example.

```javascript
// Set content
textAsync(document.body, 'Hi').then(() => {
    console.log('Set operation 1');
});

// Get content
textAsync(document.body).then(content => {
    console.log('Get operation 1');
});

// Set content
textAsync(document.body, 'Hi again').then(() => {
    console.log('Set operation 2');
});

// Get content
textAsync(document.body).then(content => {
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
textAsync(document.body, 'Hi').then(body => {
    console.log('Set operation 1');
    // Get content
    textAsync(body).then(content => {
        console.log('Get operation 1');
    });
});

// ------------
// console
// ------------
Set operation 1
Get operation 1
```

