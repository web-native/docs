# Installation

Chtml can be embedded as a script tag on your HTML page or installed via _npm_. In each case, you can choose to install the entire suite or a selection of its component libraries.

## On this Page

* [Embed As Script](installation.md#embed-as-script)
  * [Embed The Complete Build](installation.md#embed-the-complete-build)
  * [Embed Complete Builds](installation.md#embed-individual-builds)
* [Install Via NPM](installation.md#install-via-npm)
  * [Import The Default Export](installation.md#import-the-default-export)
  * [Import Individual Modules](installation.md#import-individual-modules)
  * [Initialization](installation.md#initialization)
* [Next Steps](installation.md#next-steps)

## Embed As Script

### Embed The Complete Build

The complete build ships everything about CHTML - ScopedHTML, ScopedJS, ScopeCSS, and HTMLTransport.

```markup
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

### Embed Individual Builds

Add a component library to use only a specific aspect of CHTML on your page.

#### ScopedHTML

```markup
<script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-html.js"></script>

<script>
// The above tag loads ScopedHTML into a global "WebNative" object.
// ScopedHTML would be automatically initialized with the browser's window object
const ScopedHTML = window.WebNative.ScopedHTML;
</script>
```

#### ScopedCSS

```markup
<script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-css.js"></script>

<script>
// The above tag loads ScopedCSS into a global "WebNative" object.
// ScopedCSS would be automatically initialized with the browser's window object
const ScopedCSS = window.WebNative.ScopedCSS;
</script>
```

#### ScopedJS

```markup
<script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-js.js"></script>

<script>
// The above tag loads ScopedJS into a global "WebNative" object.
// ScopedJS would be automatically initialized with the browser's window object
const ScopedJS = window.WebNative.ScopedJS;
</script>
```

#### HTMLTransport

```markup
<script src="https://unpkg.com/@web-native-js/chtml/dist/html-transport.js"></script>

<script>
// The above tag loads HTMLTransport into a global "WebNative" object.
// HTMLTransport would be automatically initialized with the browser's window object
const HTMLTransport = window.WebNative.HTMLTransport;
</script>
```

## Install Via NPM

```text
$ npm i -g npm
$ npm i --save @web-native-js/chtml
```

CHTML is written in and distributed as standard JavaScript modules, and is thus imported only with the `import` keyword.

### Import The Default Export

The default export is the complete CHTML package itself.

```javascript
// Node-style import
import Chtml from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import Chtml from './node_modules/@web-native-js/chtml/src/index.js';
```

### Import Individual Modules

Add a module to your project to use only a specific aspect of CHTML.

#### ScopedHTML

```javascript
// Node-style import
import {ScopedHTML} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import ScopedHTML from './node_modules/@web-native-js/chtml/src/scoped-html/index.js';
```

#### ScopedCSS

```javascript
// Node-style import
import {ScopedCSS} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import ScopedCSS from './node_modules/@web-native-js/chtml/src/scoped-css/index.js';
```

#### ScopedJS

```javascript
// Node-style import
import {ScopedJS} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import ScopedJS from './node_modules/@web-native-js/chtml/src/scoped-js/index.js';
```

#### HTMLTransport

```javascript
// Node-style import
import {HTMLTransport} from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import HTMLTransport from './node_modules/@web-native-js/chtml/src/html-transport/index.js';
```

### Initialization

Unlike the built editions of CHTML libraries included as scripts that are automatically initialized with the browser window, imported CHTML libraries must be explicitly initialized with a window.

To initialize all component libraries in one go:

```javascript
Chtml.init(window);
```

To initialize a specific library:

```javascript
ScopedHTML.init(window);
```

To initialize CHTML libraries on the server for server-side rendering, an emulation of a _window_ is required. This is obtainable with the [jsdom](https://github.com/jsdom/jsdom) library.

Simply install _jsdom_ and initialize CHTML with the jsdom window object.

```text
$ npm i jsdom
```

```javascript
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

More about server-side rendering is covered [here](server-side-rendering.md).

## Next Steps

Let's now learn the core concepts of CHTML.

[Chtml](../)

