# Install Reflex Componemts (Formally Observables)

## Installation

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/observables/dist/main.js"></script>

<script>
// The above tag loads Observables into a global "WebNative" object.
const Observables = window.WebNative.Observables;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/observables
```

#### Import

Observables is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

Observables works both in browser and server environments.

```javascript
// Node-style import
import Observables from '@web-native-js/observables';

// Standard JavaScript import. (Actual path depends on where you installed Observables to.)
import Observables from './node_modules/@web-native-js/observables/src/index.js';
```
