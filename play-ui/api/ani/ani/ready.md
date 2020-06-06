# ready\(\)

This [Ani](./) instance method accepts a callback to call when the animation is created and ready to play. This is useful as there are cases where the animation is created asynchronously as a performance strategy. For example, when the keyword `auto` is used for the _height_ or _width_ property, Ani will first resolve those to actual values before creating the actual animation. This happens asynchronously by queueing the operation in [Reflow](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/reflow.md).

## Syntax

```javascript
// Obtain an Ani instance and call ready()
ani.ready(readyCallback[, failureCallback = null]);
```

### Parameters

* `readyCallback` - `Function`: The function to call on ready. This function receives the following parameters:
  * `waapi` - `Animation`: The underlying WAAPI `Animation` instance.
  * `timing` - `Object`: The instance's timing object.
  * `firstFrame` - `Object`: The animation's _firstFrame_ object. Especially useful where this has to be automatically derived by Ani.
  * `lastFrame` - `Object`: The animation's _lastFrame_ object. Especially useful where this has to be automatically resolved by Ani.
* `failureCallback` - `Function`: A function to call on fatal errors preventing the creation of the animation. This function receives the following parameters:
  * `errorMsg` - `String`: The description of the error.

### Return

* `undefined`

## Examples

```markup
<div style="width:0px"></div>
```

```javascript
// Obtain an Ani instance and call ready()
let ani = new Ani(el, {width: 'auto'});
ani.ready((waapi, timing, firstFrame, lastFrame) => {
    // Show the two keyframes
    console.log(firstFrame, lastFrame);
});
```

