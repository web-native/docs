# `off()`
This function unbinds event/gesture handlers previously bound using the [`on()`](/play-ui/api/evt/on.md) function.

## Import

```js
import off from '@web-native-js/play-ui/src/evt/off.js';
```

## Syntax

```js
off(el, type[, callback]);
```

#### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `type` - `String`: The event name.
+ `callback` - `Function`: The handler function originally used with `on()`.

#### Return
+ `undefined`

## Examples

```js
// Unbind a specific listenr bound to doubletap
off(document.body, 'doubletap', callback);

// Unbind all listenrs bound to doubletap
off(document.body, 'doubletap');
```