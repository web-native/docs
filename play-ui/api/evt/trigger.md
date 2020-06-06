# EVT/trigger\(\)

This function is used to trigger event/gesture handlers previously bound using the [`on()`](on.md) function.

## Import

```javascript
import trigger from '@web-native-js/play-ui/src/evt/trigger.js';
```

## Syntax

```javascript
trigger(el, type[, data = {}]);
```

### Parameters

* `el` - `HTMLElement`: The source DOM element.
* `type` - `String`: The event name.
* `data` - `Object`: \(Optional\) oustom data for the event.

### Return

* `Event` - The `Event` instance; part of PlayUI's event system.

