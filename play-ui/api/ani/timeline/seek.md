# seek\(\)

This [Timeline](./) instance method seeks each animation to the specific percentage of playback.

## Syntax

```javascript
// Obtain an Ani instance and call seek()
timeline.seek(to[, except = []]);
```

### Parameters

* `to` - `Number`: A number between `0` and `1`, representing 0% and 100% respectively, of each animation's total timing. Compare the _Ani_ instance's [`seek()`](../ani/seek.md) method.
* `except` - `Array`: An optional list of _Ani_ objects to excempt.

### Return

* `undefined`

