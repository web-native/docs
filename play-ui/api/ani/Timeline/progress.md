# `progress()`
This [Timeline](/play-ui/api/ani/Timeline/README.md) instance method returns the average percentage progress over all animations at any point during the animation.

## Syntax

```js
// Obtain a Timeline instance and call progress()
timeline.progress();
```

### Parameters
*None*

### Return
+ `Number`: A number between `0` and `1`, representing 0% and 100% respectively, of the timeline's average progress. Compare the *Ani* instance's [`progress()`](/play-ui/api/ani/Ani/progress.md) method.