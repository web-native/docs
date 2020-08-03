> The resilient, jQuery-inspired DOM abstraction layer and a UI manipulation library.

# PlayUI v0.0.2

PlayUI provides APIs for easing DOM-related concerns, UI manipulation, events and gestures, animations, and more. It offers a minimal footprint with concise APIs that let the platform speak for itself.

[Check this project out on GitHub](https://github.com/web-native/play-ui).

## Usage

PlayUI can be used as a *constructible function* (like the jQuery function).

```js
// If PlayUI was installed via npm
import $ from '@web-native-js/play-ui';

// If PlayUI was loaded via a script tag
const $ = window.WebNative.PlayUI;

// Bind to user gestures
$(el1).on('doubletap', e => {
    // Play... then...
    $(el2).play({opacity: 0}).then($el2 => $el2.css('display', 'none'));
});
```

PlayUI's core functions may also be imported individually to use in a project.

```javascript
import on from '@web-native-js/play-ui/src/evt/on.js';
import play from '@web-native-js/play-ui/src/ani/play.js';
import cssAsync from '@web-native-js/play-ui/src/css/cssAsync.js';

// Bind to user gestures
on(el1, 'doubletap', e => {
    // Play... then...
    play(el2, {opacity: 0}).then(el2 => cssAsync(el2, 'display', 'none'));
});
```

## Documentation

+ [Installation](/play-ui/v002/installation.md).
+ [API](/play-ui/v002/api).

## Issues

To report bugs or request features, please submit an [issue](https://github.com/web-native/play-ui/issues).

## License

MIT.
