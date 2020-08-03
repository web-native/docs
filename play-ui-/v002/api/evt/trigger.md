# `trigger()`
This function is used to trigger event/gesture handlers previously bound using the [`on()`](/play-ui/v002/api/evt/on.md) function.

## Import

```js
import trigger from '@web-native-js/play-ui/src/evt/trigger.js';
```

## Syntax

```js
trigger(el, eventName[, details = {}]);
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `eventName` - `String`: The event name.
+ `details` - `Object`: (Optional) oustom data for the event.

**Return**
An [*Event* instance](#the-returned-event-instance).

## The Returned Event Instance

`trigger()` returns the fired event object which be inspected about the disposition of its handlers.

```js
let event = trigger(el, eventName);
```

Inspect the event to see the disposition of the fired listeners.

```js
if (event.defaultPrevented) {
    // event.preventDefault() has been called by a handler
    // Or a handler returned false
} else if (event.propagationStopped) {
    // event.stopPropagation() has been called by a handler
    // Or a handler returned false
} else if (event.promises) {
    // event.promise() has been called by a handler
    // Or a handler returned a promise
    event.promises.then(() => {
    // When all promises resolve
    }).catch(() => {
    // When any of the promises fail
    });
}
```