# Server-Side Rendering
THIS DOCUMENT IS BEING UPDATED!

In the [installation guide](/chtml/guide/intallation.md), we saw how to setup CHTML for server-side rendering. In this section, we will see how to link bundle files from the server during setup.

As discussed in an [earlier section](/chtml/html-transport/README.md#bundles), bundle files are, in a browser environment, linked to a document as external `<template>` files. In the case of server-side rendering, we are now closer to these bundle files, so we should be able to link the files directly from the filesystem.

The code below is similar to what we've seen before. We are now only supplying CHTML with these bundles on the `Chtml.init()` function.

```js
// Import the class directly
import Chtml, {HTMLTransport} from '@web-native-js/chtml';
// Import jsDom
import jsdom from 'jsdom';
// Utilities we'll need
import fs from 'fs';
import path from 'path';

// -----------
// Setup
// -----------

// Read the document file from the server
const documentFile = fs.readFileSync(path.resolve('./index.html'));
// Instantiate jsdom so we can obtain the "window" for CHTML
// Detailed instruction on setting up jsdom is available in the jsdom docs
const JSDOM = new jsdom.JSDOM(documentFile.toString());

// Now, we read the bundle files as string from the server
const bundleFiles = ['./tests/bundle.html'].map(file => fs.readFileSync(path.resolve(file)).toString());
// Finally, we initialize Chtml with the bundleTemplates
Chtml.init(JSDOM.window);

// -----------
// Render
// -----------
HTMLTransport.ready(() => {
    let app = {};
    JSDOM.window.document.body.bind(app);
});
```

To serve the document to the browser in a HTTP request, we could do the following:

```js
import http from 'http';

http.createServer(function (req, res) {
    // Set Content-type
    res.setHeader('Content-type', 'text/html');
    // Send the document as string
    res.end(JSDOM.window.document.body.outerHTML);
}).listen(9000);
```

## Browser-Specific Rendering
As we have seen, CHTML, by design, accepts the same markup and ScopedJS statements both for rendering in the browser and on the server; thanks to *jsdom* for creating that near-browser environment on the server. But since this is only "near-browser", there will be a few ScopedJS statements that won't be available in this mock environment. For example, since there is no concept of the UI on the server, UI-dependent DOM methods like `el.getBoundingClientRect()`, will not work as they would in a browser. You can learn more about this on the [jsdom](https://github.com/jsdom/jsdom) documentation.

It is easy to work around environmental inconsistencies. All we need to do is use `if () ... else` blocks to wrap environment-specific statements.

```html
<body>

  <script type="text/scoped-js">
    if (environment === 'browser') {
        this.innerHTML = 'My rendered width is: ' + el.getBoundingClientRect().width;
    } else {
        // do nothing
    }
  </script>

</body>
```