# Server-Side Rendering

In the [getting started](../) guide, we saw how to setup CHTML for server-side rendering. In this section, we will see how to link bundles files from the server during setup.

As discussed in an [earlier section](../compositional-concepts/namespaces-and-templates-layout-and-bundling.md), bundle files are, in a browser environment, linked to a document as external `<template>` files. In the case of server-side rendering, we are now closer to these bundle files, so we should able to link the files directly from the filesystem.

The code below is similar to what we've seen before. We are now only supplying CHTML with these bundles on the `Chtml.init()` function.

```javascript
// Import the class directly
import Chtml from '@web-native-js/chtml/src/Chtml.js';
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

// Now, we read the bundle files from the server
const bundleFiles = ['./tests/bundle.html'].map(file => fs.readFileSync(path.resolve(file)));
// Next, we create a template element for each
const bundleTemplates = bundleFiles.map(fileContent => {
    let template = JSDOM.window.document.createElement('template');
    template.innerHTML = fileContent.toString();
    return template;
});

// Finally, we initialize CHTML with the bundleTemplates
Chtml.init(JSDOM.window, () => bundleTemplates);

// -----------
// Render
// -----------
Chtml.ready(() => {

    const app = new Chtml(JSDOM.window.document.body);

});
```

To serve the document to the browser in a HTTP request, we could do the following:

```javascript
import http from 'http';

http.createServer(function (req, res) {
    // Set Content-type
    res.setHeader('Content-type', 'text/html');
    // Send the document as string
    res.end(app.el.outerHTML);
}).listen(9000);
```

## Browser-Only Directives

As we have seen, CGTML, by design, accepts the same markup and directives both for rendering in the browser and on the server; thanks to _jsdom_ for creating that near-browser environment on the server. But since this is only "near-browser", there will be a few methods that won't be available or this mock environment. For example, since there is no concept of the UI on the server, UI-dependent DOM methods like `el.getBoundingClientRect()`, will not work as they would in a browser. You can learn more about this on the [jsdom](https://github.com/jsdom/jsdom) documentation.

It is easy to work around environmental inconsistencies using `if () ... else` statements in directives, in combination with the component's `env` property, to achieve environment-specific behaviour.

```markup
<body>

    <script type="text/scoped-js">
      if (env === 'browser') {
          html('My rendered width is: ' + el.getBoundingClientRect().width);
      } else {
          // do nothing
      }
    </script>

</body>
```

Note that the `component.env` property is a direct mirror of the global [`params.env`](configuration.md) parameter.

