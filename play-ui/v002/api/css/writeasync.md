# CSS/writeAsync\(\)

This function sets one or more style properties on the given element. It is the programmatic alternative to [`ElementCSSInlineStyle.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style). It also has special support for vendor-prefixed properties.

<<<<<<< HEAD:play-ui/v002/api/css/writeasync.md
The suffix *Async* differentiates this method from its *Sync* counterpart - [`writeSync()`](/play-ui/v002/api/css/writesync.md). Unlike the *Sync* counterpart, `writeAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
=======
The suffix _Async_ differentiates this method from its _Sync_ counterpart - [`writeSync()`](writesync.md). Unlike the _Sync_ counterpart, `writeAsync()` is a promised-based function that runs in a different flow from that of the calling code. It follows a performance strategy that lets the browser engine decide the most convenient time to honour its call.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/writeasync.md

## Import

```javascript
import writeAsync from '@web-native-js/play-ui/src/css/writeAsync.js';
```

## Syntax
<<<<<<< HEAD:play-ui/v002/api/css/writeasync.md
See [`cssAsync() - Set/Unset Inline Styles`](/play-ui/v002/api/css/cssasync.md#greater-than-set-unset-inline-styles)
=======

See [`cssAsync() - Set/Unset Inline Styles`](cssasync.md#greater-than-set-unset-inline-styles)

>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/css/writeasync.md
