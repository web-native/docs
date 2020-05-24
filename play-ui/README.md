# PlayUI
> The resilient, jQuery-inspired DOM abstraction layer and a UI manipulation library - carefully put together for performance and a playful developer experience.

PlayUI provides APIs for easing DOM-related concerns, UI manipulation, events and gestures, animations, and more. It offers a minimal footprint with concise APIs that let the platform speak for itself.

### Usage
PlayUI can be used as a *constructible function* (like the jQuery function) with its core functions available as instance methods.

```js
import $ from '@web-native-js/play-ui';
// Or if PlayUI was loaded via a script tag
const $ = window.WebNative.PlayUI;

$(el1).on('doubletap', e => {
    $(el2).play({opacity: 0}).then($el2 => $el2.css('display', 'none'));
});
```

PlayUI's core functions may also be imported as individually to use in a project.

```js
import {on} from '@web-native-js/play-ui/src/evt/index.js';
import {play} from '@web-native-js/play-ui/src/ani/index.js';
import {cssAsync} from '@web-native-js/play-ui/src/css/index.js';


on(el1, 'doubletap', e => {
    play(el2, {opacity: 0}).then(el2 => cssAsync(el2, 'display', 'none'));
});
```

## Getting Started
+ Take [the installation guide](/play-ui/guide.md).
+ Visit the [API reference](/play-ui/api/).

