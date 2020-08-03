<<<<<<< HEAD:play-ui/v002/api/ani/play.md
# `play()`
This function creates and play an animation. It is a convenient way to use the PlayUI's [Ani](/play-ui/v002/api/ani/Ani/README.md) class.
=======
# ANI/play\(\)

This function creates and play an animation. It is a convenient way to use the PlayUI's [Ani](ani/) class.
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/ani/play.md

## Import

```javascript
import play from '@web-native-js/play-ui/src/ani/play.js';
```

## Syntax

```javascript
let promise = play(el, effect[, timing = {}]);
```

### Parameters
<<<<<<< HEAD:play-ui/v002/api/ani/play.md
+ `el` - `HTMLElement`: The source DOM element.
+ `effect` - `Array|Object|String`: The effect to play. This could be a standard keyframes array, a CSS object or a stylesheet-based animation name.
+ `timing` - `Object`: Options for the animation. See [Ani](/play-ui/v002/api/ani/Ani.md#parameters).
=======

* `el` - `HTMLElement`: The source DOM element.
* `effect` - `Array|Object|String`: The effect to play. This could be a standard keyframes array, a CSS object or a stylesheet-based animation name.
* `timing` - `Object`: Options for the animation. See [Ani](https://github.com/web-native/docs/tree/4d4ea8f2ac9ea9b989339a1423c7dd36c5a6108a/play-ui/api/ani/Ani.md#parameters).
>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/api/ani/play.md

### Return

* `Promise` - A promise that resolves when the animation finishes.

## Usage

### Play from Standard Keyframes

Below, we fade out an element with keyframes.

```javascript
let el = document.querySelector('#el');
play(el, [{opacity:1}, {opacity:0}], {duration:600}).then(el => {
    console.log('The end!');
});
```

### Play a CSS Transition

Below, we fade out an element by simply specifying a end-state keyframe and letting Ani derive the starting keyframe from the element's current state.

```javascript
play(el, {opacity:0}, {duration:600}).then(el => {
    console.log('The end!');
})
```

### Play a CSS Animation Name

Below, we fade out an element with an animation keyframes defined in the document's stylesheet.

```markup
<style>

@keyframes fadeout {
  0% { opacity: 1;}
  100% { opacity: 0;}
}

</style>
```

```javascript
play(el, 'fadeout', {duration:600}).then(el => {
    console.log('The end!');
})
```

