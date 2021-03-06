# `onfinish()`
This [Timeline](/play-ui/v002/api/ani/Timeline/README.md) instance method accepts a callback to call when the animation with the highest timing completes.

## Syntax

```js
// Obtain an Ani instance and call onfinish()
ani.onfinish(callback);
```

### Parameters
+ `callback` - `Function`: The function to call on finish. This function receives no parameters:

### Return
+ `undefined`

## Usage

```js
let timeline = new Timeline;
timeline.onfinish().then(() => {
    console.log('The end!');
});
// Duration: 600ms
timeline.add(new Ani(el1, {opacity:0}, {duration: 600}));
setTimeout(() => {
    // Duration: 400ms. But htis will finish last in the timeline
    timeline.add(new Ani(el2, {opacity:0}, {duration: 600}));
}, 300);
```