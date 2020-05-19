# `trigger()`
This function is used to trigger event/gesture handlers previously bound using the [`on()`](/play-ui/api/evt/on.md) function.

## Import

```js
import trigger from '@web-native-js/play-ui/src/evt/trigger.js';
```

## Syntax

```js
trigger(el, type[, data = {}]);
```

#### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `type` - `String`: The event name.
+ `data` - `Object`: (Optional) oustom data for the event.

#### Return
+ `Event` - The `Event` instance; part of PlayUI's event system.