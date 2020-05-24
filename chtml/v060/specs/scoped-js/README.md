# Scoped JS
Scoped JS is a special technology that lets us couple JavaScript functionality with any element in a page. Instead of the traditional way of retrieving elements into scripts, Scoped JS lets us place functionality just right where they are needed.

## On this page:
+ [Scoped Scripts](#scoped-scripts)
+ [Automatic Observability](#automatic-observability)
+ [Globals](#globals)
+ [ScopedJS Configuration](#scopedjs-configuration)

## Scoped Scripts
*Scoped scripts* go by the special `text/scoped-js` MIME type. This differentiates them from regular scripts and sets them apart from normal browser handling.

The script below is scoped to the `#alert` element. And the `this` reference is uniquely pointed to the `#alert` element.

```html
<div id="alert">
    <div class="message">This task is now complete!</div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      this.querySelector('.exit').addEventListener('click', () => {
          this.remove();
      });
    </script>
</div>
```

The closest we could get with native HTML would be the use of intrinsic event attributes. And we would quickly be hitting the limits of what's possible with such an approach.

```html
<div id="alert">
    <div class="message">This task is now complete!</div>
    <div class="exit" title="Close this message." onclick="this.parentNode.remove()">X</div>
</div>
```

Taking things further, it is possible to receive external values in a scoped script, usually a data object from an application. Properties of the data object bound to a soped script can be accessed by name.

```js
let alertEl = document.querySelector('#alert');
alertEl.bind({
    message: 'This task is now complete!',
});
```

```html
<div id="alert">
    <div class="message"></div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      let messageEl = this.querySelector('.message');
      messageEl.innerHTML = message;
      this.querySelector('.exit').addEventListener('click', () => {
          this.remove();
      });
    </script>
</div>
```

It is also possible to bind non-objects to a scoped script. This time, our script would need to **directly recieve** the bound value *as-is*. To do this, we would define the recieving function in our script and make it the script's return value.

```html
<div id="alert">
    <div class="message"></div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      var alternativeText = 'Alternative Hello World!';
      return (boundValue) => {
          this.innerHTML = boundValue ? boundValue : alternativeText;
      };
    </script>
</div>
```

Now, calling `alertEl.bind('Hello World!')` would also invoke our recieving function with the same arguments.

## Automatic Observability
Change detection is a critical feature in client-side scripting. And this is a native feature in ScopedJS! ScopedJS supports the [Reflex API](/reflex/) for making live changes to an object or array that has already been bound. Live changes automatically trigger the specific Scoped JS **statements** that rely on the changed properties.

Below is an example showing how the final state of an animation is kept in sync with the state of an external reference.

```html
<div id="alert">
    <div class="message"></div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">

      if (message) {
          let messageEl = this.querySelector('.message');
          messageEl.innerHTML = message;
      }

      // Let's fade-out the element
      let animation = this.animate([{opacity:1}, {opacity:0}], {duration: 400});
      animation.onfinish = () => {
          // But after fade-out, the visibility of this element should now
          // depend on whatever the application decides for opacityLevel
          this.style.opacity = opacityLevel;
      };

    </script>
</div>
```

Our area of interest is the statement `this.style.opacity = opacityLevel` within the animation's `onfinish` block. By referencing the `opacityLevel` variable, this statement is now bound to the state of the `opacityLevel` property, and will rerun whenever `opacityLevel` is updated. So in our application, we could keep blinking the element simply by changing the value of `opacityLevel` at intervals.

Below, we are now using the Reflex API to modify `opacityLevel` in-place instead of rebinding the data object afresh.

```js
let alertEl = document.querySelector('#alert');

// Initial values
let data = {
    message: 'This task is now complete!',
    opacityLevel: 0,
};
alertEl.bind(data);

// The blinking logic
let opacityLevel = 0;
setInterval(() => {
    opacityLevel = opacityLevel ? 0 : 1;
    Reflex.set(data, 'opacityLevel', opacityLevel);
}, 600);
```

## Globals
Unless explicitly given, Scoped JS wouldn't have access to any variable in the global scope. The Scoped JS's `ENV.globals` configuration parameter is where we create the global scope that Scoped JS sees.

If *ScopedJS* was loaded via a script tag, `ENV.globals` would be available from the window object:

```js
let ScopedJS_ENV = window.WebNative.ScopedJS.ENV;
let ScopedJSGlobals = ScopedJS_ENV.globals;
```

An import statement would be used otherwise:

```js
// Import directly
import {ENV} from '@web-native-js/chtml/src/scoped-js/index.js';
let ScopedJSGlobals = ENV.globals;

// We could also access the same ENV object from Chtml's ENV object
import {ENV as CHTML_ENV} from '@web-native-js/chtml';
let ScopedJSGlobals = CHTML_ENV.ScopedJS.globals;
```

Now we can assign global references. jQuery is a good example.

```js
ScopedJSGlobals.$ = window.jQuery;
```

```html
<div id="alert">
    <div class="message"></div>
    <div class="exit" title="Close this message.">X</div>
    <script type="text/scoped-js">
      $('.exit', this).on('click', () => {
          $(this).remove();
      });
    </script>
</div>
```

## ScopedJS Configuration
ScopedJS has been designd to be fully customizable. Simply obtain ScopedJS's `ENV` object and configure its `params` property.

```js
// If ScopedJS has been loaded via a script tag
let ENV = window.WebNative.ScopedJS.ENV;

// To use the import method
import {ENV} from '@web-native-js/chtml/src/scoped-js/index.js';
// To access the same ENV from the Chtml's ENV
import {ENV as CHTML_ENV} from '@web-native-js/chtml';
let ENV = CHTML_ENV.ScopedJS;
```

Here are the configuration options.

+ **ENV.params.scriptElement** - (String): This setting configures script element specifiers on a page. The default is `script[type="text/scoped-js"]`. Only change this where it is absolutely necessary, and be sure it is not a standard MIME type.

    ```js
    ENV.params.scriptElement = 'script[type="text/javascript+scoped"]';
    ```

    ```html
    <div>
      <script type="text/javascript+scoped">
      // Code...
      </script>
    </div>
    ```

+ **ENV.params.autoHide** - (Boolean): This setting determines whether or not ScopedJS scripts are automatically removed from the DOM once parsed. The default is `true` - remove.

+ **ENV.params.bindMethodName** - (String): This setting specifies the method name for binding data on an element's scoped script. The default is `bind`.

    ```js
    ENV.params.bindMethodName = 'render';
    ```

    ```js
    let alertEl = document.querySelector('#alert');
    let data = {
        message: 'This task is now complete!',
    };

    alertEl.render(data);

    // Only subsequent deep modifications may require Reflex
    setTimeout(() => {
        Reflex.set(data, 'opacityLevel', 1);
        Reflex.deleteProperty(data, 'message');
    }, 100)l
    ```

+ **ENV.params.inertContexts** - (Array): This setting specifies the elements under which scoped scripts should be *inert*. The default is an empty list.

    ```js
    ENV.params.inertContexts.append('my-custom-element');
    ```
    
    ```js
    <my-custom-element>
      <!-- This scoped script will be innert -->
      <script type="text/scoped-js">
      // Code...
      </script>
    </my-custom-element>
    ```
