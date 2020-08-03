# Installation

## Standard Build

The standard build gives you CHTML out of the box - the fully-featured edition of the library. You can embed it as a script tag on your HTML page. Or you install it via _npm_.

### Embed as script

```markup
<script src="https://unpkg.com/@web-native-js/chtml/dist/main.js"></script>

<script>
// The above tag loads CHTML into a global "WebNative" object.
const Chtml = window.WebNative.Chtml;
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

```javascript
// Node-style import
import Chtml from '@web-native-js/chtml';

// Standard JavaScript import. (Actual path depends on where you installed CHTML to.)
import Chtml from './node_modules/@web-native-js/chtml/src/index.js';
```

To use CHTML for server-side rendering, you'll import the `Chtml.js` class directly. You'll also need to install the [jsdom](https://github.com/jsdom/jsdom) library - the only required dependency for this purpose. _jsdom_ will help us emulate the "window" environment for CHTML here on the server.

```javascript
// Import the class directly
import Chtml from '@web-native-js/chtml/src/Chtml.js';
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

// Initialize CHTML... and we'll be good to go
Chtml.init(JSDOM.window);
```

We will learn more about server-side rendering [later on](extras/server-side-rendering.md).

## Other Builds

_Coming soon!_

## Next Steps

Let's now head to the core concepts of CHTML in a brief overview.

[Getting Started](./)



