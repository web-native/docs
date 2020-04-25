# Firedom

> The professional-grade DOM abstraction layer, carefully put together for developer experience and performance.

Firedom provides APIs for easing multiple DOM-related concerns. It offers a minimal footprint with concise APIs that let the platform speak for itself.

```javascript
let el = select('#el');
on(el, 'doubletap', e => {
    play(el, 'fadeout').then(el => cssAsync(el, 'display', 'none'));
});
```

## Guide

Visit the [guide](guide.md) for install options.

## API

Visit the [API reference](api/).

