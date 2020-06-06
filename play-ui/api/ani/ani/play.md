# play\(\)

This [Ani](./) instance method plays the animation. Unlike the WAAPI's `play()` instance method, this method returns a promise.

## Syntax

```javascript
// Obtain an Ani instance and call play()
ani.play();
```

### Parameters

_None_

### Return

* `Promise` - A promise that resolves when the animation completes. The `ani` instance is returned on resolve.

## Examples

```javascript
let ani = new Ani(el, [{opacity:1}, {opacity:0}], {duration:600});
ani.play().then(ani => {
    console.log('The end!');
    // Call other methods
    ani.reverse();
});
```

