# Bundling
Namespaces in the HTML Transport system can be implemented in a folder-based layout. Here, components are defined in stand-alone HTML files placed in a hierarchy of folders on the server.

The following *article* components can be redefined as standalone files as shown:

```html
<template is="chtml-bundle">
    <div namespace="html/components/article/readonly" scope="article">...</div>
    <div namespace="html/components/article/editable" scope="article">...</div>
</template>
```

```html
components
  |-- article
    |-- readonly.html
    |-- editable.html
```

The *namespace* attribute is not needed for components in a file; a component's namespace is naturally the combination of its path and filename.

file://components/article/readonly.html

```html
<div scope="article">...</div>
```

file://components/article/editable.html

```html
<div scope="article">...</div>
```

To put all our components back into one file, we would need to *bundle* them. This time, namespaces would be automatically derived and assigned to each component based on their file location. All of this can be automated using the nodejs-based *Bundler* utility.

## Usage

```js
import Bundler from ‘@web-native-js/chtml/src/html-transport/Bundler.js’;
const bundler = new Bundler(entry[, params = {}]);
```

Above, `entry` is the base directory for component files. Files are scanned recursively from here into subdirectories and are bundled each with a namespace that reflects their location. The optional `params` parameter provides additional configuration for the instance.

To get the bundled contents as string, call the `output()` method.

```js
var bundleContent = bundler.output();
```

To save the contents to a file, provide the destination file to the `output()` method; this can be an absolute path or a relative path – relative to the bundle `entry` given initially.

```js
bundler.output(outputFile);
```

The `Bundler.bundle()` function may also be used

```js
Bundler.bundle(entry[, outputFile[, params]]);
```

Let’s demonstrate this with the sample file structure below.

```html
root
  |-- project-files
  |  |-- components
  |  |  |-- article
  |  |    |-- readonly.html
  |  |    |-- editable.html
  |  |-- page1.html
  |  |-- page2.html
  |-- public-folder
```

Now we bundle the files in the `project-files` directory into `public-folder`.

```javascript
const bundler = new Bundler(‘/root/project-files/’);
bundler.output(‘/root/public-folder/bundle.html’);
```

This file can now be loaded as a CHTML bundle.

```html
<template is=”chtml-bundle” src=”public-folder/bundle.html”></template>
```

On checking the bundle, you will notice that the namespace of each module is prefixed with the extension name of their original file. Here’s how that could look:

```html
<div namespace=”html/components/article/readonly" scope="article"></div>
<div namespace=”html/components/article/editable" scope="article"></div>
<div namespace=”html/page1”></div>
<div namespace=”html/page2”></div>
```

## Bundling From Multiple Entries

To create multiple bundles from multiple entries, use the static `Bundler.bundle()` method. This method accepts a list of entries as an object and returns an object with each bundle name mapped to their respective bundle content.

```js
var bundles = Bundler.bundle({
    images: ‘project-files/images/’,
    template: ‘project-files/templates/’,
});
console.log(bundles.images);
```

To save all bundles to a common public directory, provide an output file path as a second argument to `Bundler.bundle()`.

```js
Bundler.bundle({
    images: ‘project-files/images/’,
    templates: ‘project-files/templates/’,
}, ‘/public-folder/[name].bundle.html’);
```

Notice the `[name]` placeholder in the destination filename. For each of the bundles, this placeholder is replaced with the unique bundle name. So, while all bundles are saved to `/public-folder/`, each bundle is saved with a unique filename. \(If a placeholder is not in the given template file path, something bad happens – each bundle is saved to the same file and previous bundles are overwritten!\)

We can now load each bundle.

```html
<template is=”chtml-bundle” src=”public-folder/images.bundle.html”></template>
<template is=”chtml-bundle” src=”public-folder/templates.bundle.html”></template>
```

## Bundling Assets

While HTML modules are created by reading the file’s contents, assets, like images, are handled differently. These files are copied from their location into the output directory where the regular bundles are located. An appropriate HTML element that points to this new location is automatically generated in the bundle. This is illustrated below.

This is the normal file structure. It now contains an image.

```html
root
  |-- project-files
  |  |-- components
  |  |  |-- article
  |  |    |-- readonly.html
  |  |    |-- editable.html
  |  |-- images
  |  |  |-- image1.png
  |  |-- page1.html
  |  |-- page2.html
  |-- public-folder
```

Let’s bundle the files in the `project-files` directory.

```js
const bundler = new Bundler(‘/root/project-files/’);
bundler.output(‘/root/public-folder/bundle.html’);
```

Now the image at `/root/project-files/images/image1.png` will now be copied to `/root/public-folder/images/image1.png` and an `<img>` element pointing to this new location is added to the bundle.

This is the new file structure:

```html
root
  |-- project-files
  |  |-- components
  |  |  |-- article
  |  |    |-- readonly.html
  |  |    |-- editable.html
  |  |-- images
  |  |  |-- image1.png
  |  |-- page1.html
  |  |-- page2.html
  |-- public-folder
     |-- images
     |  |-- image1.png
     |-- bundle.html
```

And our `bundle.html` should look like this:

```html
<div namespace=”html/components/article/readonly" scope="article"></div>
<div namespace=”html/components/article/editable" scope="article"></div>
<img namespace=”png/images/image1” src=”images/image1.png” />
<div namespace=”html/page1”></div>
<div namespace=”html/page2”></div>
```

In another way, it is possible to bundle small images \(or other media\) in *data-URL* format. This way, the browser won't have to load them via HTTP request. Cutting down on the number of assets to load should greatly speed up the site's load time.

The Bundler just needs to know at what file size to use the *data-URL* format. Set the `params.maxDataURLsize` to a size measured in bytes.

```js
Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {maxDataURLsize: 1024});
```

Media files lower than `1024` in size will now be bundled in *data-URL* format. The bundled asset should now look like this:

```html
<img namespace=”png/images/image1” src="data:image/png,%89PNG%0D%0A=" />
```

## Additional Customization
The `params` parameter can be used to influence the way modules are bundled.

* By default, a module's derived namespace path is set on the `namespace` attribute. To change this, use the `params.namespaceKey` parameter. But take note that this also has to be same value with HTML Transport's [`ENV.params.namespaceAttribute`](/chtml/html-transport/README.md#htmltransport-configuration) parameter.

    ```js
    Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {namespaceKey: 'ns'});
    ```

    ```html
    <div ns=”html/components/article/readonly" scope="article"></div>
    <div ns=”html/components/article/editable" scope="article"></div>
    <img ns=”png/images/image1” src=”images/image1.png” />
    <div ns=”html/page1”></div>
    <div na=”html/page2”></div>
    ```

* It is possible to inplement a different pattern for namespace paths. To do this, provide a callback function on `params.namespaceCallback`. For each module being processed, this function will recieve the module's file information and should return an appropriate namespace.

    ```js
    Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {
        namespaceCallback: (path, filenameWithoutExt, fileExt) => {
            return (fileExt ? fileExt.replace('.', '') + '/' : '') + filenameWithoutExt.replace(/\\/g, '/');
        }
    });
    ```

