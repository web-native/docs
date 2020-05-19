# PlayUI Guide
PlayUI can be installed and used as a library. Individual modules may also be used as standalone functions.

## Library Install
Install the built PlayUI library to use modules as instance methods.

### Embed as script

```html
<script src="https://unpkg.com/@web-native-js/play-ui/dist/main.js"></script>

<script>
// The above tag loads PlayUI into a global "WebNative" object.
const $ = window.WebNative.PlayUI;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/play-ui
```

#### Import
PlayUI is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

PlayUI works both in browser and server environments.

```js
// Node-style import
import $ from '@web-native-js/play-ui';

// Standard JavaScript import. (Actual path depends on where you installed PlayUI to.)
import $ from './node_modules/@web-native-js/play-ui/src/index.js';
```

### Usage

```js
$('#el1').on('doubletap', e => {
    $('#el2').play('fadeout').then(instance => instance.css('display', 'none'));
});
```

## Individual Module Install
Install individual PlayUI modules to use as standalone functions. The [API reference](/play-ui/api/) documents the installation for each module. Here are a few examples.

```js
import {on} from '@web-native-js/play-ui/src/evt/index.js';
import {play} from '@web-native-js/play-ui/src/ani/index.js';
import {cssAsync} from '@web-native-js/play-ui/src/css/index.js';
```

### Usage

```js
let el = document.querySelector('#el1');
on(el, 'doubletap', e => {
    play(el, 'fadeout').then(el => cssAsync(el, 'display', 'none'));
});
```

## API
Visit the [API reference](/play-ui/api/)

