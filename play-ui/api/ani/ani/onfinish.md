# onfinish\(\)

This [Ani](./) instance method accepts a callback to call when the animation finishes. This is different from WAAPI's `onfinish` instance property. Unlike that property that makes room for only one callback function, Ani's `onfinish()` method can be called any number of times with a callback.

## Syntax

```javascript
// Obtain an Ani instance and call onfinish()
ani.onfinish(callback);
```

### Parameters

* `callback` - `Function`: The function to call on finish. This function receives the following parameters:
  * `el` - `HTMLElement`: The underlying DOM element playing the animation.

### Return

* `undefined`

## Examples

```javascript
// Obtain an Ani instance
let ani = new Ani(el, {opacity: 0});
// Bind a callback
ani.onfinish(el => {
    console.log('The ' + el.id + ' element just completed an animation!');
});
// Bind another callback
ani.onfinish(el => {
    el.remove();
});
```

