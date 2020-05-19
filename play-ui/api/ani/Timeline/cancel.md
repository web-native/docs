# `cancel()`
This [Timeline](/play-ui/api/ani/Timeline/README.md) instance method cancels all animations. Animations added after this call are canclled automatically.

## Syntax

```js
// Obtain a Timeline instance and call cancel()
timeline.cancel(except = []);
```

### Parameters
+ `except` - `Array`: An optional list of *Ani* objects to excempt.

### Return
+ `undefined`