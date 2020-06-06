# Installation Guide

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/reflex-componemts/dist/main.js"></script>

<script>
// The above tag loads ReflexComponemts into a global "WebNative" object.
const ReflexComponemts = window.WebNative.ReflexComponemts;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/reflex-componemts
```

#### Import

Reflex Components are written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

```javascript
// Node-style import
import * as ReflexComponemts from '@web-native-js/reflex-componemts';

// Standard JavaScript import. (Actual path depends on where you installed Observables to.)
import * as ReflexComponemts from './node_modules/@web-native-js/reflex-componemts/src/index.js';
```

