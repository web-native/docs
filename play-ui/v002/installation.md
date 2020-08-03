# PlayUI Guide

PlayUI can be installed and used as a composed, instantiable module. Its core functions may also be imported and used as standalone modules.

## On this Page

* [Embed As Script](guide.md#embed-as-script)
* [Install vVa NPM](guide.md#install-via-npm)
  * [Import the Default Export](guide.md#import-the-default-export)
  * [Import Individual Modules](guide.md#import-individual-modules)
* [Next Steps](guide.md#next-steps)

## Embed As Script

Add the following script tag to your page to include the instantiable edition of PlayUI.

```markup
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

PlayUI is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

### Import the Default Export

Import the default export to include the instantiable edition of PlayUI in a project.

```javascript
// Node-style import
import $ from '@web-native-js/play-ui';

// Standard JavaScript import. (Actual path depends on where you installed PlayUI to.)
import $ from './node_modules/@web-native-js/play-ui/src/index.js';
```

### Import Individual Modules

Import a module to use a specific functionality in your project. Each module has been documented with their specific installation path. Here are examples.

```javascript
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
<<<<<<< HEAD:play-ui/v002/installation.md
+ Take [the introduction to PlayUI](/play-ui/).
+ Visit the [API reference](/play-ui/v002/api/).
=======

* Take [the introduction to PlayUI](./).
* Visit the [API reference](api/).

>>>>>>> cb39f35064f56fa8e785a6c3dc76ad40ec3a079d:play-ui/guide.md
