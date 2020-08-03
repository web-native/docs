# `play()`
This function creates and play an animation. It is a convenient way to use the PlayUI's [Ani](/play-ui/v002/api/ani/Ani/README.md) class.

## Import

```js
import play from '@web-native-js/play-ui/src/ani/play.js';
```

## Syntax

```js
let promise = play(el, effect[, timing = {}]);
```

### Parameters
+ `el` - `HTMLElement`: The source DOM element.
+ `effect` - `Array|Object|String`: The effect to play. This could be a standard keyframes array, a CSS object or a stylesheet-based animation name.
+ `timing` - `Object`: Options for the animation. See [Ani](/play-ui/v002/api/ani/Ani.md#parameters).

### Return
+ `Promise` - A promise that resolves when the animation finishes.

## Usage

### Play from Standard Keyframes
Below, we fade out an element with keyframes.

```js
let el = document.querySelector('#el');
play(el, [{opacity:1}, {opacity:0}], {duration:600}).then(el => {
    console.log('The end!');
});
```

### Play a CSS Transition
Below, we fade out an element by simply specifying a end-state keyframe and letting Ani derive the starting keyframe from the element's current state.

```js
play(el, {opacity:0}, {duration:600}).then(el => {
    console.log('The end!');
})
```

### Play a CSS Animation Name
Below, we fade out an element with an animation keyframes defined in the document's stylesheet.

```html
<style>

@keyframes fadeout {
  0% { opacity: 1;}
  100% { opacity: 0;}
}

</style>
```

```js
play(el, 'fadeout', {duration:600}).then(el => {
    console.log('The end!');
})
```