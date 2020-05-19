# `ready()`
This [Ani](/play-ui/api/ani/Ani/README.md) instance method accepts a callback to call when the animation is created and ready to play. This is useful as there are cases where the animation is created asynchronously as a performance strategy. For example, when the keyword `auto` is used for the *height* or *width* property, Ani will first resolve those to actual values before creating the actual animation. This happens asynchronously by queueing the operation in [Reflow](/play-ui/api/reflow.md).

## Syntax

```js
// Obtain an Ani instance and call ready()
ani.ready(readyCallback[, failureCallback = null]);
```

### Parameters
+ `readyCallback` - `Function`: The function to call on ready. This function receives the following parameters:
    + `waapi` - `Animation`: The underlying WAAPI `Animation` instance.
    + `timing` - `Object`: The instance's timing object.
    + `firstFrame` - `Object`: The animation's *firstFrame* object. Especially useful where this has to be automatically derived by Ani.
    + `lastFrame` - `Object`: The animation's *lastFrame* object. Especially useful where this has to be automatically resolved by Ani.
+ `failureCallback` - `Function`: A function to call on fatal errors preventing the creation of the animation. This function receives the following parameters:
    + `errorMsg` - `String`: The description of the error.

### Return
+ `undefined`

## Examples

```html
<div style="width:0px"></div>
```

```js
// Obtain an Ani instance and call ready()
let ani = new Ani(el, {width: 'auto'});
ani.ready((waapi, timing, firstFrame, lastFrame) => {
    // Show the two keyframes
    console.log(firstFrame, lastFrame);
});
```