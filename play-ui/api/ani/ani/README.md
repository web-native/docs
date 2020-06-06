# ANI/Ani

This class provides an intuitive way to use the native [Web Animations API \(WAAPI\)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Animations_API).

## Import

```javascript
import Ani from '@web-native-js/play-ui/src/ani/Ani.js';
```

## Syntax

```javascript
let animation = new Ani(el, effect[, timing = {}]);
```

### Parameters

* `el` - `HTMLElement`: The subject DOM element.
* `effect` - `Array|Object|String`: The effect to play. This could be a standard keyframes array, a CSS object or a stylesheet-based animation name.
* `timing` - `Object`: Options for the animation.
  * `duration` - `Number`: The animation's duration in milliseconds.
  * `fill` - `String`: The element's final state on finish; one of `none` \(default\), `forwards`, `both`.
  * `delay` - `Number`: The animation's delay in milliseconds before playing.
  * `endDelay` - `Number`: The animation's delay in milliseconds after playing.
  * `direction` - `String`: The animation's direction; one of `normal` \(default\), `reverse`, `alternate`, `alternate-reverse`.
  * `easing` - `String`: The rate of the animation's change over time; one of `linear` \(default\), `ease`, `ease-in`, `ease-out`, `ease-in-out`, or a custom `cubic-bezier` value like `cubic-bezier(0.42, 0, 0.58, 1)`.
  * `iterations` - `Number`: The number of times the animation should repeat.
  * `iterationStart` - `Number`: At what point in the iteration the animation should start.
  * `cancelForCss` - `Boolean`: \(Specific to Ani\) Whether to cancel the animation on finish for subsequent CSS rules on the element to take effect. By default, properties animated with WAAPI cannot be modified with CSS.

## Examples

```javascript
let el = document.querySelector('#el');
let ani = new Ani(el, [{opacity:1}, {opacity:0}], {duration:600});
ani.play().then(el => {
    console.log('The end!');
});
```

## Features

Ani provides the following rich set of features over WAAPI:

* **Support for single-frame keyframes.** _Ani_ will automatically derive the animation's first frame from the element's current state.

  ```javascript
    // Fade out from current opacity level
    let ani = new Ani(el, {opacity: 0}, timing);
  ```

* **Support for CSS keyframes.** _Ani_ can play animations defined in the document's stylesheets. Just mention an animation name.

  ```markup
    <style>

    @keyframes fadeout {
    0% { opacity: 1;}
    100% { opacity: 0;}
    }

    </style>
  ```

  ```javascript
    // Play the animation from stylesheet
    let ani = new Ani(el, 'fadeout', timing);
  ```

* **Support for** _**auto**_ **height and width.** _Ani_ accepts and automacally computes the keyword _auto_ for the width and height properties.

  ```markup
    <div style="width:0px"></div>
  ```

  ```javascript
    // Expand to the element's real size at "auto"
    let ani = new Ani(el, {width: 'auto'}, timing);
  ```

* **Automatic units for unit-based CSS properties.** _Ani_ will automacally suffix numeric values with `px` for properties like `top` that don't go without units.

  ```javascript
    // Help add "px" to the value for width
    let ani = new Ani(el, {width: 50}, timing);
  ```

* **Support for shortform rules.** _Ani_ will accept an object or array for the following shortforms: inset, margin, padding.

  ```javascript
    // Destructure the following object into their individual properties
    let ani = new Ani(el, {inset: {top: 50, left: 100}}, timing);
    // Here we actually mean 
    let ani = new Ani(el, {top: 50, left: 100}, timing);
    // ---------------
    // Destructure the following array into their individual properties
    let ani = new Ani(el, {inset: [50, 100, 75, 125,]}, timing);
    // Here we actually mean 
    let ani = new Ani(el, {top: 50, right: 100, bottom: 75, left: 125}, timing);
    // All shortforms go in this order: top, right, bottom, left
  ```

* **Sensible default timing parameters.** _Ani_ will automatically create sensible values for the animation timing where not defined.

  ```javascript
    // The timing object has the following defaults
    {
        duration: 400,
        fill: 'both',
    }
  ```

* **Other usefull methods.** See below.

## Methods

* [`ready()`](ready.md) - Accepts a callback that runs when the animation is created and ready.
* [`onfinish()`](onfinish.md) - Accepts a callback that runs when the animation is completed.
* [`oncancel()`](oncancel.md) - Accepts a callback that runs when the animation is cancelled.
* [`progress()`](progress.md) - Returns the animation's percentage progress at any point during the animation.
* [`seek()`](seek.md) - Seeks to a specific percentage of playback.
* [`reverse()`](reverse.md) - Reverses the animation playback.
* [`play()`](play.md) - Plays the animation and returns a promise that resolves when the animation completes.
* [`pause()`](pause.md) - Pauses the animation.
* [`finish()`](finish.md) - Forces the animation to the finished state.
* [`cancel()`](cancel.md) - Cancels the animation.

