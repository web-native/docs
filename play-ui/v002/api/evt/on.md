# EVT/on\(\)

This function binds an event/gesture handler to an element. This works like [`EventTarget.addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) but adds support for user gestures and custom event implementation.

## Import

```javascript
import on from '@web-native-js/play-ui/src/evt/on.js';
```

## Syntax

```js
on(el, eventName, handler[, params = {}]);
```

**Parameters**
+ `el` - `HTMLElement`: The source DOM element.
+ `eventName` - `String`: The event name.
+ `handler` - `Function`: The handler function. This recieves:
    + `event` - an event object
+ `params` - `Object`: Additional parameters.

**Return**
A [*Listener* instance](#the-returned-listener-instance).

## Usage

```javascript
on(document.body, 'doubletap', event => {
    console.log('You doubletapped me!', event.details);
});
```

Use the [`trigger()`](/play-ui/v002/api/evt/trigger.md) method to fire the event.

```js
// Trigger
trigger(document.body, 'doubletap');
// And we can add details
trigger(document.body, 'doubletap'), {time:0});
```

## Tagging a Listener

The `params.tags` parameter can be used to tag a listener. Tags are an *array* of values (*strings*, *numbers*, *objects*, etc) that can be used to identify the listener for later use.

```js
on(el, eventName, handler, {tags:['#tag']});
```

## The Returned Listener Instance

The `on()` function returns a *Listener* instance that gives us per-instance control.

```js
// Obtain the Listener instance
let instance = on(el, eventName, handler);

// Synthetically trigger the listener
instance.fire({
    type: 'doubletap',
});

// Disconnect the listener
instance.disconnect();
```

## Gestures

PlayUI uses the [Hammer.js](https://hammerjs.github.io/) gesture library to support the following gestures out of the box. For details of these gestures, see the _Hammer.js_ documentation.

* **press**: press, pressup
* **rotate**: rotate, rotatestart, rotatemove, rotateend, rotatecancel
* **pinch**: pinch, pinchstart, pinchmove, pinchend, pinchcancel, pinchin, pinchout
* **pan**: pan, panstart, panmove, panend, pancancel, panleft, panright, panup, pandown
* **swipe**: swipe, swipeleft, swiperight, swipeup, swipedown
* **tap**: tap, \(by custom extension: tripletap, doubletap, singletap\)

Ensure to include Hammer in your page.

```markup
<script src="//unpkg.com/@web-native-js/cui/ext/hammer.min.js"></script>
```
