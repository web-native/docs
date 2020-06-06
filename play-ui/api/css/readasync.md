# CSS/readAsync\(\)

This function returns one or more style properties for the given element. It is a convenient alternative to [`window.getComputedStyle`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle). It also has special support for vendor-prefixed properties.

The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`readSync()`](readsync.md). Unlike the _Sync_ counterpart, `readAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.

## Import

```javascript
import readAsync from '@web-native-js/play-ui/src/css/readAsync.js';
```

## Syntax

See [`cssAsync() - Get Computed Properties`](cssasync.md#greater-than-get-computed-properties)

