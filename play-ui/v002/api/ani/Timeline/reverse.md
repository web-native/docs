# `reverse()`
This [Timeline](/play-ui/v002/api/ani/Timeline/README.md) instance method toggles the playback direction for all animations. Animations added after this call are either reversed automatically or left as-is, depending on whether the last call to `reverse()` implied *new reverse* or *reverse to original direction*.

## Syntax

```js
// Obtain an Ani instance and call reverse()
timeline.reverse(excepr = []);
```

### Parameters
+ `except` - `Array`: An optional list of *Ani* objects to excempt.

### Return
+ `undefined`