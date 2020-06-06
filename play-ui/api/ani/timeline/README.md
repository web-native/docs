# ANI/Timeline

This class provides a convenient way to control multiple animation \([Ani](../ani/)\) instances.

## Import

```javascript
import Timeline from '@web-native-js/play-ui/src/ani/Timeline.js';
```

## Syntax

```javascript
let timeline = new Timeline(animations[, params = {}]);
```

### Parameters

* `animations` - `Array`: Zero or more [Ani](../ani/) instances.
* `params` - `Object`: A few parameters for the timeline. \(Currently none\).

## Examples

Play/pause multiple animations in one call.

```javascript
import Ani from '@web-native-js/play-ui/src/ani/Ani.js';

let ani1 = new Ani(el1, [{opacity:1}, {opacity:0}], {duration:600});
let ani2 = new Ani(el2, [{width:0}, {width:100}], {duration:900});

let timeline = new Timeline([ani1, ani2]);
timeline.pause();
setTimeout(() => {
   timeline.play().then(() => {
        console.log('The end; all animations!');
    });
}, 1000);
```

## Features

* **Runtime manipulation of timeline.** _Timeline_ allows you to add/remove animation instances at runtime without altering coordination and synchronization.

  ```javascript
    // Fade out from current opacity level
    let timeline = new Timeline([ani1, ani2]);
    // Kick off
    timeline.play().then(() => {
        console.log('The end; all animations!');
    });
    // On the fly
    timeline.add(ani3);
    timeline.remove(ani1);
    // Yet, our play.then() will work as expected
  ```

## Methods

* [`add()`](add.md) - Adds a new _Ani_ instance to the timeline.
* [`remove()`](remove.md) - Removes an _Ani_ instance from the timeline.
* [`onfinish()`](onfinish.md) - Accepts a callback that runs when all animations are completed.
* [`oncancel()`](oncancel.md) - Accepts a callback that runs when any of the animations is cancelled.
* [`progress()`](progress.md) - Returns the average percentage progress over all animations at any point during the animation.
* [`seek()`](seek.md) - Seeks each animation to the specific percentage of playback.
* [`reverse()`](reverse.md) - Toggles the playback direction for all animations. Animations added after this call are either reversed automatically or left as-is, depending on whether the last call to `reverse()` implied _new reverse_ or _reverse to original direction_.
* [`play()`](play.md) - Plays all animations and returns a promise that resolves when the animations complete. Animations added after this call are played automatically.
* [`pause()`](pause.md) - Pauses all animations. Animations added after this call are paused automatically.
* [`finish()`](finish.md) - Forces all animations to their finished state. Animations added after this call are forced to finish automatically.
* [`cancel()`](cancel.md) - Cancels all animations. Animations added after this call are canclled automatically.
* [`clear()`](clear.md) - Clears the timeline of all animations. 

