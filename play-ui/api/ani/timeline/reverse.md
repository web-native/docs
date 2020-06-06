# reverse\(\)

This [Timeline](./) instance method toggles the playback direction for all animations. Animations added after this call are either reversed automatically or left as-is, depending on whether the last call to `reverse()` implied _new reverse_ or _reverse to original direction_.

## Syntax

```javascript
// Obtain an Ani instance and call reverse()
timeline.reverse(excepr = []);
```

### Parameters

* `except` - `Array`: An optional list of _Ani_ objects to excempt.

### Return

* `undefined`

