# `seek()`
This [Ani](/play-ui/api/ani/Ani/README.md) instance method seeks to a percentage point in the animation's timeline.

## Syntax

```js
// Obtain an Ani instance and call seek()
ani.seek(to);
```

### Parameters
+ `to` - `Number`: A number between `0` and `1`, representing 0% and 100% respectively, of the animation's total timing. Here the total timing is calculated as `delay + duration + endDelay`.

### Return
+ `Ani` - The `ani` instance.

## Examples

```js
let ani = new Ani(el, [{opacity:1}, {opacity:0}], {delay: 100, duration:600});
// The following will seek the animation to 350ms
ani.seek(0.5);
```