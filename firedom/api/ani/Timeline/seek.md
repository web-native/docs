# `seek()`
This [Timeline](/firedom/api/ani/Timeline/README.md) instance method seeks each animation to the specific percentage of playback.

## Syntax

```js
// Obtain an Ani instance and call seek()
timeline.seek(to[, except = []]);
```

### Parameters
+ `to` - `Number`: A number between `0` and `1`, representing 0% and 100% respectively, of each animation's total timing. Compare the *Ani* instance's [`seek()`](/firedom/api/ani/Ani/seek.md) method.
+ `except` - `Array`: An optional list of *Ani* objects to excempt.

### Return
+ `undefined`