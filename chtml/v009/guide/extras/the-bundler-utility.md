# The Bundler Utility

This is a little _node.js_-based utility that automatically bundles markup-based files into a single file that can be linked-to as an external CHTML bundle.

## Usage

```javascript
import Bundler from ‘@web-native-js/chtml/src/Bundler.js’;
const bundler = new Bundler(entry[, params = {}]);
```

Above, `entry` is the base directory for component files. Files are scanned recursively from here into subdirectories and are bundled each with a namespace that reflects their location. The optional `params` parameter provides additional configuration for the instance.

To get the bundled contents as string, call the `output()` method.

```javascript
var bundleContent = bundler.output();
```

To save the contents to a file, provide the destination file to the `output()` method; this can be an absolute path or a relative path – relative to the bundle `entry` given initially.

```javascript
bundler.output(outputFile);
```

The `Bundler.bundle()` function may also be used

```javascript
Bundler.bundle(entry[, outputFile[, params]]);
```

Let’s demonstrate this with the sample file structure below.

```markup
root
      |--- project-files
      |      |--- components
      |      |    |--- component1.html
      |      |--- page1.html
      |      |--- page2.html
      |--- public-folder
```

Now we bundle the files in the `project-files` directory into `public-folder`.

```javascript
const bundler = new Bundler(‘/root/project-files/’);
bundler.output(‘/root/public-folder/bundle.html’);
```

This file can now be loaded as a CHTML bundle.

```markup
<template is=”chtml-bundle” src=”public-folder/bundle.html”></template>
```

On checking the bundle, you will notice that the namespace of each module is prefixed with the extension name of their original file. Here’s how that could look:

```markup
<div data-namespace=”html/components/component1”></div>
<div data-namespace=”html/page1”></div>
<div data-namespace=”html/page2”></div>
```

## Bundling From Multiple Entries

To create multiple bundles from multiple entries, use the static `Bundler.bundle()` method. This method accepts a list of entries as an object and returns an object with each bundle name mapped to their respective bundle content.

```javascript
var bundles = Bundler.bundle({
    images: ‘project-files/images/’,
    template: ‘project-files/templates/’,
});
console.log(bundles.images);
```

To save all bundles to a common public directory, provide an output file path as second argument to `Bundler.bundle()`.

```javascript
Bundler.bundle({
    images: ‘project-files/images/’,
    templates: ‘project-files/templates/’,
}, ‘/public-folder/[name].bundle.html’);
```

Notice the `[name]` placeholder in the destination filename. For each of the bundles, this placeholder is replaced with the unique bundle name. So, while all bundles are saved to `/public-folder/`, each bundle is saved with a unique filename. \(If a placeholder is not in the given template file path, something bad happens – each bundle is saved to the same file and previous bundles are overwritten!\)

We can now load each bundle.

```markup
<template is=”chtml-bundle” src=”public-folder/images.bundle.html”></template>
<template is=”chtml-bundle” src=”public-folder/templates.bundle.html”></template>
```

## Bundling Assets

While HTML modules are created by reading the file’s contents, assets, like images, are handled differently. These files are copied from their location into the output directory where the regular bundles are located. An appropriate HTML element that points to this new location is automatically generated in the bundle. This is illustrated below.

This is the normal file structure. It now contains an image.

```markup
root
      |--- project-files
      |      |--- components
      |      |    |--- component1.html
      |      |--- images
      |      |    |--- image1.png
      |      |--- page1.html
      |      |--- page2.html
      |--- public-folder
```

Let’s bundle the files in the `project-files` directory.

```javascript
const bundler = new Bundler(‘/root/project-files/’);
bundler.output(‘/root/public-folder/bundle.html’);
```

Now the image at `/root/project-files/images/image1.png` will now be copied to `/root/public-folder/images/image1.png` and an `<img>` element pointing to this new location is added to the bundle.

This is the new file structure:

```markup
root
      |--- project-files
      |      |--- components
      |      |    |--- component1.html
      |      |--- images
      |      |    |--- image1.png
      |      |--- page1.html
      |      |--- page2.html
      |--- public-folder
              |--- images
              |    |--- image1.png
              |--- bundle.html
```

And our `bundle.html` should look like this:

```markup
<div data-namespace=”html/components/component1”></div>
<img data-namespace=”png/images/image1” src=”images/image1.png” />
<div data-namespace=”html/page1”></div>
<div data-namespace=”html/page2”></div>
```

In another way, it is possible to bundle small images \(or other media\) in data-URL format. This way, the browser won't have to load them via HTTP request. Cutting down on the number of assets to load should greatly speed up the site's load time.

The Bundler just needs to know at what file size to use the data-URL format. Set the static `params.maxDataURLsize` to a size measured in bytes.

```javascript
Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {maxDataURLsize: 1024});
```

Media files lower than `1024` in size will now be bundled in data-URL format. The bundled asset should now look like this:

```markup
<img data-namespace=”png/images/image1” src="data:image/png,%89PNG%0D%0A=" />
```

## Additional Customization

The `params` parameter can be used to influence the way modules are bundled.

* By default, a module's derived namespace path is set on the `data-namespace` attribute. To change this, use the `params.namespaceKey` parameter. But take note that this also has to be value in the main CHTML's global [`paramss`](configuration.md) object.

  ```javascript
      Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {namespaceKey: 'c-ns'});
  ```

  ```markup
      <div c-ns=”html/components/component1”></div>
      <img c-ns=”png/images/image1” src=”images/image1.png” />
      <div c-ns=”html/page1”></div>
      <div c-ns=”html/page2”></div>
  ```

* It is possible to inplement a different pattern for namespace paths. To do this, provide a callback function on `params.namespaceCallback`. For each module being processed, this function will recieve the module's file information and should return an appropriate namespace.

  ```javascript
      Bundler.bundle('/root/project-files', '../public-folder/bundle.html', {
            namespaceCallback: (path, filenameWithoutExt, fileExt) => {
                  return (fileExt ? fileExt.replace('.', '') + '/' : '') + filenameWithoutExt.replace(/\\/g, '/');
            }
      });
  ```
