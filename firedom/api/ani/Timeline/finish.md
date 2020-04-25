# `finish()`
This [Timeline](/firedom/api/ani/Timeline/README.md) instance method forces all animations to their finished state. Animations added after this call are forced to finish automatically.

## Syntax

```js
// Obtain a Timeline instance and call finish()
timeline.finish(except = []);
```

### Parameters
+ `except` - `Array`: An optional list of *Ani* objects to excempt.

### Return
+ `undefined`