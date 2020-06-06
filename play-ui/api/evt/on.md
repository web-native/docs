# EVT/on\(\)

This function binds an event/gesture handler to an element. This works like [`EventTarget.addEventListener()`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) but adds support for user gestures and custom event implementation.

## Import

```javascript
import on from '@web-native-js/play-ui/src/evt/on.js';
```

## Syntax

```javascript
on(el, type, callback);
```

### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `type` - `String`: The event name.
* `callback` - `Function`: The handler function.

### Return

* `Listener` - The `Listener` instance; part of PlayUI's event system.

## Examples

```javascript
on(document.body, 'doubletap', event => {
    alert('You doubletapped me!');
});
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

