# `play()`
This [Timeline](/firedom/api/ani/Timeline/README.md) instance method plays all animations and returns a *Promise*. Animations added after this call are played automatically.

## Syntax

```js
// Obtain a Timeline instance and call play()
timeline.play();
```

### Parameters
*None*

### Return
+ `Promise` - A Promise that resolves when the animation with the highest timing completes. An animation's total timing withing the timeline could, in addition to its *delay*, *duration*, *endDelay* parameters, be affected by a direct pause/play call, or its relative time of entering the timeline.

## Examples

```js
let timeline = new Timeline;
timeline.play().then(() => {
    console.log('The end!');
});
// Duration: 600ms
timeline.add(new Ani(el1, {opacity:0}, {duration: 600}));
setTimeout(() => {
    // Duration: 400ms. But htis will finish last in the timeline
    timeline.add(new Ani(el2, {opacity:0}, {duration: 600}));
}, 300);
```