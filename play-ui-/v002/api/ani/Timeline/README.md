# The Timeline Class
This class provides a convenient way to control multiple animation ([Ani](/play-ui/v002/api/ani/Ani/README.md)) instances.

## Import

```js
import Timeline from '@web-native-js/play-ui/src/ani/Timeline.js';
```

## Syntax

```js
let timeline = new Timeline(animations[, params = {}]);
```

### Parameters
+ `animations` - `Array`: Zero or more [Ani](/play-ui/v002/api/ani/Ani/README.md) instances.
+ `params` - `Object`: A few parameters for the timeline. (Currently none).

## Usage
Play/pause multiple animations in one call.

```js
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

+ **Runtime manipulation of timeline.** *Timeline* allows you to add/remove animation instances at runtime without altering coordination and synchronization.
    ```js
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
+ [`add()`](/play-ui/v002/api/ani/Timeline/add.md) - Adds a new *Ani* instance to the timeline.
+ [`remove()`](/play-ui/v002/api/ani/Timeline/remove.md) - Removes an *Ani* instance from the timeline.
+ [`onfinish()`](/play-ui/v002/api/ani/Timeline/onfinish.md) - Accepts a callback that runs when all animations are completed.
+ [`oncancel()`](/play-ui/v002/api/ani/Timeline/oncancel.md) - Accepts a callback that runs when any of the animations is cancelled.
+ [`progress()`](/play-ui/v002/api/ani/Timeline/progress.md) - Returns the average percentage progress over all animations at any point during the animation.
+ [`seek()`](/play-ui/v002/api/ani/Timeline/seek.md) - Seeks each animation to the specific percentage of playback.
+ [`reverse()`](/play-ui/v002/api/ani/Timeline/reverse.md) - Toggles the playback direction for all animations. Animations added after this call are either reversed automatically or left as-is, depending on whether the last call to `reverse()` implied *new reverse* or *reverse to original direction*.
+ [`play()`](/play-ui/v002/api/ani/Timeline/play.md) - Plays all animations and returns a promise that resolves when the animations complete. Animations added after this call are played automatically.
+ [`pause()`](/play-ui/v002/api/ani/Timeline/pause.md) - Pauses all animations. Animations added after this call are paused automatically.
+ [`finish()`](/play-ui/v002/api/ani/Timeline/finish.md) - Forces all animations to their finished state. Animations added after this call are forced to finish automatically.
+ [`cancel()`](/play-ui/v002/api/ani/Timeline/cancel.md) - Cancels all animations. Animations added after this call are canclled automatically.
+ [`clear()`](/play-ui/v002/api/ani/Timeline/clear.md) - Clears the timeline of all animations. 
