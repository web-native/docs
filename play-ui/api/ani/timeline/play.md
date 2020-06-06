# play\(\)

This [Timeline](./) instance method plays all animations and returns a _Promise_. Animations added after this call are played automatically.

## Syntax

```javascript
// Obtain a Timeline instance and call play()
timeline.play();
```

### Parameters

_None_

### Return

* `Promise` - A Promise that resolves when the animation with the highest timing completes. An animation's total timing withing the timeline could, in addition to its _delay_, _duration_, _endDelay_ parameters, be affected by a direct pause/play call, or its relative time of entering the timeline.

## Examples

```javascript
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

