# `progress()`
This [Ani](/play-ui/v002/api/ani/Ani/README.md) instance method returns the animation's percentage progress as a number.

## Syntax

```js
// Obtain an Ani instance and call progress()
let progress = ani.progress();
```

### Parameters
*None*

### Return
+ `Number`: A number between `0` and `1`, representing 0% and 100% respectively, of the animation's total timing. Here the total timing is calculated as `delay + duration + endDelay`.

## Usage

```js
let ani = new Ani(el, [{opacity:1}, {opacity:0}], {delay: 100, duration:600});
// Get the progress at 350ms
setTimeout(() => {
    console.log(ani.progress()); // About 0.5
}, 350);
```