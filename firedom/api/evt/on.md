# `on()`
This function binds an event listener to an element. This works like [`EventTarget.addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) but adds support for user gestures and custom event implementation.

## Import

```js
import on from '@web-native-js/firedom/src/evt/on.js';
```

## Syntax

```js
on(el, type, callback);
```

#### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `type` - `String`: The event name.
+ `callback` - `Function`: The handler function.

#### Return
+ `Listener` - The `Listener` instance; part of Firedom's event system.

## Examples

```js
on(document.body, 'doubletap', event => {
    alert('You doubletapped me!');
})
```

## Gestures
Firedom uses the [Hammer.js](https://hammerjs.github.io/) gesture library to support the following gestures out of the box. For details of these gestures, see the *Hammer.js* documentation.
+ **press**: press, pressup
+ **rotate**: rotate, rotatestart, rotatemove, rotateend, rotatecancel
+ **pinch**: pinch, pinchstart, pinchmove, pinchend, pinchcancel, pinchin, pinchout
+ **pan**: pan, panstart, panmove, panend, pancancel, panleft, panright, panup, pandown
+ **swipe**: swipe, swipeleft, swiperight, swipeup, swipedown
+ **tap**: tap, (by custom extension: tripletap, doubletap, singletap)

Ensure to include Hammer in your page.

```html
<script src="//unpkg.com/@web-native-js/cui/ext/hammer.min.js"></script>
```
