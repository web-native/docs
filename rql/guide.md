# MQL Guide

## Installation

### Embed as script

```html
<script src="https://unpkg.com/@web-native-js/mql/dist/main.js"></script>

<script>
// The above tag loads Mql into a global "WebNative" object.
const Mql = window.WebNative.Mql;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/mql
```

#### Import

Mql is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

Mql works both in browser and server environments.

```javascript
// Node-style import
import Mql from '@web-native-js/mql';

// Standard JavaScript import. (Actual path depends on where you installed Mql to.)
import Mql from './node_modules/@web-native-js/mql/src/index.js';
```

