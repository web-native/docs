# `writeSync()`
This function sets one or more style properties on the given element. It is the programmatic alternative to [`ElementCSSInlineStyle.style`](https://developer.mozilla.org/en-US/docs/Web/API/ElementCSSInlineStyle/style). It also has special support for vendor-prefixed properties.

The suffix *Sync* differentiates this method from its *Async* counterpart - [`writeAsync()`](/firedom/api/css/writeasync.md). Unlike the *Async* counterpart, `writeSync()` is a normal function that runs in the same flow with that of the calling code.

## Import

```js
import writeSync from '@web-native-js/firedom/src/css/writeSync.js';
```

## Syntax
See [`cssAsync() - Set/Unset Inline Styles`](/firedom/api/css/csssync.md#greater-than-set-unset-inline-styles)
