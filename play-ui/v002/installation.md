# PlayUI Guide

PlayUI can be installed and used as a composed, instantiable module. Its core functions may also be imported and used as standalone modules.

## On this Page

* [Embed As Script](#embed-as-script)
* [Install Via NPM](#install-via-npm)
* [Next Steps](#next-steps)

## Embed As Script

Add the following script tag to your page to include the instantiable edition of PlayUI.

```html
<script src="https://unpkg.com/@web-native-js/play-ui/dist/main.js"></script>

<script>
// The above tag loads PlayUI into a global "WebNative" object.
const $ = window.WebNative.PlayUI;
</script>
```

## Install Via NPM

```text
$ npm i -g npm
$ npm i --save @web-native-js/play-ui
```

The installed package is designed to be *initialized* with the *window* object of the current browser or server evironment. To do this, import the `ENV` object and assign the *window* object to it, then call the initializer.

+ **Initialize the Constructible Build** - Initialize the module below for the constructible build of PlayUI.

```js
// Node-style import
import PlayUI, { ENV } from '@web-native-js/play-ui';
// Standard JavaScript import. (Actual path depends on where you installed PlayUI to.)
import PlayUI, { ENV } from './node_modules/@web-native-js/play-ui/src/index.js';

// Initialize
ENV.window = window;
PlayUI.INIT();
```

+ **Initialize Individual Utility** - Import a specific utility to your project. Each utility has been documented with their specific installation path. Here are examples.

```js
// Simgle imports
import on from '@web-native-js/play-ui/src/evt/on.js';
import play from '@web-native-js/play-ui/src/ani/play.js';
import cssAsync from '@web-native-js/play-ui/src/css/cssAsync.js';

// Or import multiple from the same category
import {on, trigger} from '@web-native-js/play-ui/src/ani/index.js';

// Standard JavaScript import. (Actual path depends on where you installed PlayUI to.)
import on from './node_modules/@web-native-js/play-ui/src/evt/on.js';
```

## Next Steps
+ Take [the introduction to PlayUI](/play-ui/).
+ Visit the [API reference](/play-ui/v002/api/).
