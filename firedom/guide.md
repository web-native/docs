# Firedom Guide

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/firedom/dist/main.js"></script>

<script>
// The above tag loads Firedom into a global "WebNative" object.
const Firedom = window.WebNative.Firedom;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/firedom
```

#### Import

Firedom is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

Firedom works both in browser and server environments.

```javascript
// Node-style import
import Firedom from '@web-native-js/firedom';

// Standard JavaScript import. (Actual path depends on where you installed Firedom to.)
import Firedom from './node_modules/@web-native-js/firedom/src/index.js';
```

## API

Visit the [API reference](api/)

