# `pause()`
This [Timeline](/play-ui/api/ani/Timeline/README.md) instance method pauses all animations. Animations added after this call are paused automatically.

## Syntax

```js
// Obtain a Timeline instance and call pause()
timeline.pause(except = []);
```

### Parameters
+ `except` - `Array`: An optional list of *Ani* objects to excempt.

### Return
+ `undefined`