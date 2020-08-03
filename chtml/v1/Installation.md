# Installation Guide

CHTML is designed as a polyfill and can be embedded as a script tag on an HTML page or installed via *npm*. In each case, you can choose to install the entire suite or one of its four features.

## On this Page

+ [Embed As Script](#embed-as-script)
  + [Embed The Complete Suite](#embed-the-complete-suite)
  + [Embed Individual Features](#embed-individual-features)
+ [Install Via NPM](#install-via-npm)
  + [Initialize the Complete Suite](#initialize-the-complete-suite)
  + [Initialize Individual Features](#initialize-individual-features)
+ [Server-Side Usage](#server-side-usage)
+ [Next Steps](#next-steps)

## Embed As Script 

+ **Embed The Complete Suite** - The complete set ships everything about CHTML - Scoped HTML, Scope CSS, Scoped JS, and HTML Partials.

  ```html
  <script src="https://unpkg.com/@web-native-js/chtml/dist/main.js"></script>
  ```

+ **Embed Individual Features** - To use only a specific feature of CHTML on your page, add a component library .

  + **Scoped HTML**

    ```html
    <script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-html.js"></script>
    ```

  + **Scoped CSS**

    ```html
    <script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-css.js"></script>
    ```

  + **Scoped JS**

    ```html
    <script src="https://unpkg.com/@web-native-js/chtml/dist/scoped-js.js"></script>
    ```

  + **HTML Partials**

    ```html
    <script src="https://unpkg.com/@web-native-js/chtml/dist/html-partials.js"></script>
    ```

## Install Via NPM

```text
$ npm i -g npm
$ npm i --save @web-native-js/chtml
```

\* CHTML is written in, and distributed as, standard JavaScript modules, and is imported with the `import` keyword.

In each case below, we are importing the `ENV` object and assigning our environment's *window* object to it before initializing the module.

+ **Initialize the Complete Suite**

  ```js
  // Import
  import init, { ENV } from '@web-native-js/chtml';
  // Initialize
  ENV.window = window;
  init();
  ```

+ **Initialize Individual Features**

  + **Scoped HTML**
    
    ```js
    // Import
    import init, { ENV } from '@web-native-js/chtml/src/scoped-html/index.js';
    // Initialize
    ENV.window = window;
    init();
    ```

  + **Scoped CSS**
    
    ```js
    // Import
    import init, { ENV } from '@web-native-js/chtml/src/scoped-css/index.js';
    // Initialize
    ENV.window = window;
    init();
    ```

  + **Scoped JS**
    
    ```js
    // Import
    import init, { ENV } from '@web-native-js/chtml/src/scoped-js/index.js';
    // Initialize
    ENV.window = window;
    init();
    ```

  + **HTML Partials**
    
    ```js
    // Import
    import init, { ENV } from '@web-native-js/chtml/src/html-partials/index.js';
    // Initialize
    ENV.window = window;
    init();
    ```

## Server-Side Usage

CHTML can be polyfilled on DOM instances created on the server with a library like [jsdom](https://github.com/jsdom/jsdom). In this case, the `window` object in the examples above will be the `window` object of the jsdom instance.


```js
// Import the class directly
import init, { ENV } from '@web-native-js/chtml';
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
ENV.window = JSDOM.window;
init();
```

## Next Steps

Let's now learn the core concepts of CHTML.

[Chtml](/chtml/)



