# Installation
Chtml can be used in whole or in part.

## The Complete Build
The complete build ships everything about CHTML - ScopedHTML, ScopedJS, ScopeCSS, and HTMLTransport. You can embed it as a script tag on your HTML page or install it via *npm*.

### Embed as script

```html
<script src="https://unpkg.com/@web-native-js/chtml/dist/main.js"></script>

<script>
// The above tag loads CHTML into a global "WebNative" object.
const Chtml = window.WebNative.Chtml;

// The component libraries would also be available on the global "WebNative" object.
// They would all be automatically initialized with the browser's window object
const ScopedHTML = window.WebNative.ScopedHTML;
const ScopeCSS = window.WebNative.ScopeCSS;
const ScopedJS = window.WebNative.ScopedJS;
const HTMLTransport = window.WebNative.HTMLTransport;
</script>
```

### Install via npm

```text
$ npm i -g npm
$ npm i --save @web-native-js/chtml
```

#### Import
CHTML is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

To use CHTML for rendering on the browser, import the default export from the package. The imported module comes with CHTML automatically initialized for the browser environment.

```js
// Node-style import
import Chtml from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import Chtml from './node_modules/@web-native-js/chtml/src/index.js';
```

To use CHTML for server-side rendering, you'll need to install the [jsdom](https://github.com/jsdom/jsdom) library - the only required dependency for this purpose. *jsdom* will help us emulate the "window" environment for CHTML here on the server.

```js
// Import the class directly
import Chtml from '@web-native-js/chtml';
// Import jsDom
import jsdom from 'jsdom';
// Utilities we'll need
import fs from 'fs';
import path from 'path';

// Read the document file from the server
const documentFile = fs.readFileSync(path.resolve('./index.html'));
// Instantiate jsdom so we can obtain the "window" for CHTML
// Detailed instruction on setting up jsdom is available in the jsdom docs
const JSDOM = new jsdom.JSDOM(documentFile.toString());

// Initialize CHTML...
// and we'll be good to go
Chtml.init(JSDOM.window);
```

More about server-side rendering is covered [here](/chtml/guide/server-side-rendering.md).

## Individual Builds
To use individual aspects of Chtml, you would simply include/import the specific library.

### ScopedHTML
To include ScopedHTML as a script:

```html
<script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-html.js"></script>

<script>
// The above tag loads ScopedHTML into a global "WebNative" object.
// ScopedHTML would be automatically initialized with the browser's window object
const ScopedHTML = window.WebNative.ScopedHTML;
</script>
```

To import ScopedHTML:

```js
// Node-style import
import {ScopedHTML} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import ScopedHTML from './node_modules/@web-native-js/chtml/src/scoped-html/index.js';

// Initialize with the jsdom window object to use on the server side
ScopedHTML.init(JSDOM.window);
```

### ScopedJS
To include ScopedJS as a script:

```html
<script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-js.js"></script>

<script>
// The above tag loads ScopedJS into a global "WebNative" object.
// ScopedJS would be automatically initialized with the browser's window object
const ScopedJS = window.WebNative.ScopedJS;
</script>
```

To import ScopedJS:

```js
// Node-style import
import {ScopedJS} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import ScopedJS from './node_modules/@web-native-js/chtml/src/scoped-js/index.js';

// Initialize with the jsdom window object to use on the server side
ScopedJS.init(JSDOM.window);
```

### HTMLTransport
To include HTMLTransport as a script:

```html
<script src="https://unpkg.com/@web-native-js/chtml/dist/html-transport.js"></script>

<script>
// The above tag loads ScopedJS into a global "WebNative" object.
// HTMLTransport would be automatically initialized with the browser's window object
const HTMLTransport = window.WebNative.HTMLTransport;
</script>
```

To import HTMLTransport:

```js
// Node-style import
import {HTMLTransport} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import HTMLTransport from './node_modules/@web-native-js/chtml/src/html-transport/index.js';

// Initialize with the jsdom window object to use on the server side
HTMLTransport.init(JSDOM.window);
```

## Next Steps
Let's now learn the core concepts of CHTML.

[Chtml](/chtml/)



