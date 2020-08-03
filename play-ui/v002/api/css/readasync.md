# CSS/readAsync\(\)

This function returns one or more style properties for the given element. It is a convenient alternative to [`window.getComputedStyle`](https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle). It also has special support for vendor-prefixed properties.

<<<<<<< HEAD:play-ui/v002/api/css/readasync.md
The suffix *Async* differentiates this method from its *Sync* counterpart - [`readSync()`](/play-ui/v002/api/css/readsync.md). Unlike the *Sync* counterpart, `readAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
=======
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`readSync()`](readsync.md). Unlike the _Sync_ counterpart, `readAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/readasync.md

## Import

```javascript
import readAsync from '@web-native-js/play-ui/src/css/readAsync.js';
```

## Syntax
<<<<<<< HEAD:play-ui/v002/api/css/readasync.md
See [`cssAsync() - Get Computed Properties`](/play-ui/v002/api/css/cssasync.md#greater-than-get-computed-properties)
=======

See [`cssAsync() - Get Computed Properties`](cssasync.md#greater-than-get-computed-properties)

>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/readasync.md
