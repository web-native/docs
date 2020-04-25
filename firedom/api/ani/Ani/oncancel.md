# `oncancel()`
This [Ani](/firedom/api/ani/Ani/README.md) instance method accepts a callback to call when the animation is cancelled for any reason. This is different from WAAPI's `oncancel` instance property. Unlike that property that makes room for only one callback function, Ani's `oncancel()` method can be called any number of times with a callback.

## Syntax

```js
// Obtain an Ani instance and call oncancel()
ani.oncancel(callback);
```

### Parameters
+ `callback` - `Function`: The function to call on cancel. This function receives the following parameters:
    + `el` - `HTMLElement`: The underlying DOM element playing the animation.

### Return
+ `undefined`

## Examples

```js
// Obtain an Ani instance
let ani = new Ani(el, {opacity: 0});
// Bind a callback
ani.oncancel(el => {
    console.log('An animation on the ' + el.id + ' element just got oncancelled!');
});
// Bind another callback
ani.oncancel(el => {
    // Do something else
});
```