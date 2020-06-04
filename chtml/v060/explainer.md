# CHTML Explainer

## Status
## TODO
link to official Scoped CSS
HTML parts and walls explainer
list more CSS naming schemes
write about frameworks

Often, an HTML document follows some **information hierarchy** and other pieces of **standalone functional blocks** that are related **hierarchically**. When we look, we realize that what we really end up with, or try to achieve, is a hierarchy of scopes and sub scopes, or better put, **a component architecture**. Yet, the HTML language itself is yet to have a way of defining these smaller-sized scopes. What we have, instead, is **one global document scope** for every element, every content/functional block, every stylesheet, and every script.

What we have:

What we want:

This state of misfit (having one **flat** scope for what is **hierarchically-related**) has been the root of much of our UI engineering problems:

+ **Naming Conflicts**. Since everything in HTML belongs to a global scope, IDs are often conflicting, especially where the document is being dynamically-generated. We often have to implement some auto-ID generator. And where this has to be manually done, the back-and-forth just never stops.

    In the IDs below, we are practically having to flatten the hierarchical structure of our information, just to avoid conflicts.

    ```html
    <body>

        <article id="continents">
            <b>Continents</b>

            <section id="continents-europe">
                <b>Europe</b>

                <div id="continents-europe-about">
                    <b>About Europe</b>
                    <ul>
                        <li>Fact1</li>
                        <li>Fact2</li>
                    </ul>
                </div>

                <div id="continents-europe-countries">
                    <b>Countries in Europe</b>
                    <ul>
                        <li>Country1</li>
                        <li>Country2</li>
                    </ul>
                </div>
            </section>

            <section id="continents-africa">
                <b>Africa</b>

                <div id="continents-africa-about">
                    <b>About Africa</b>
                    <ul>
                        <li>Fact1</li>
                        <li>Fact2</li>
                    </ul>
                </div>

                <div id="continents-africa-countries">
                    <b>Countries in Africa</b>
                    <ul>
                        <li>Country1</li>
                        <li>Country2</li>
                    </ul>
                </div>
            </section>

        </article>

    </body>
    ```

+ **Clumsy Naming Conventions**. The history of modular naming schemes for HTML/CSS has not been a story of success. We have experimented with different class-naming conventions like BEM. Maybe it hasn’t been obvious, but all such experiments are pointing to a missing feature in HTML itself.
+ **CSS Specificity Wars**. It always happens that we intend to style one element, we end up with many styled elements. We try a narrower selector, but then, the volatility, as everything breaks on changes in the DOM tree! This doubles with vertical specificity (targeting a specific deep descendant). Since there are no scope boundaries, we end up with verbose, often inaccurate, very inefficient and very volatile selectors.
+ **Frameworks Abstractions**. Not pollyfills

## Doesn't the Shadow DOM Fix This?

No! Although the Shadow DOM provides the desired encapsulation for naming, styling and functionality, it stands far from being the ideal solution to every scoping need. This is because:
+ It is impractical to use custom elements and the Shadow DOM everywhere as generic structural pieces. All we are asking for here is structural modularity without throwing the regular HTML tags away. We also do not always want total subtree encapsulation! So, the overengineering with this approach would be too unmanageable!
+ Even the Shadow DOM - being the same globally-scoped document (albeit smaller) - is itself not immune to the challenge. Try creating a little large Shadow tree, and the problem with naming elements and CSS specificity wars will resurface! Yes, even here!
+ While a component architecture involves both *encapsulation* and some sense of parent/child *relationships*, what the Shadow DOM would only ever do for us is encapsulation. Web Components generally isn’t thinking for a means of relationship between "components".

A component architecture thus remains a fundamental need in the HTML language itself. Every piece of HTML (even the Shadow tree) would stand to benefit from a more modular tree.

## Everything Is Pointing to a Component-Oriented HTML!

I propose CHTML - a new set of platform features that bring a component architecture to the HTML language. CHTML will bring the missing bits and tie the remaining nuts to make the platform everything for modern UI development.

### Overview

CHTML is a suite of four new technologies that provide for a component-based HTML. The good news is that CHTML is already obtainable today through the Web-Native project. Here, I will only describe each component with an example or two, then link to the appropriate place in the project docs to see more.

+ **Scoped HTML** - A new way to structure an HTML document as a hierarchy of scopes and sub scopes. Each scope establishes a new naming context for elements within its subtree - introducing *Roots* and *Scoped IDs*.

    **Roots and Scoped IDs:**

    ```html
    <body>

        <article  root id="continents">
            <b>Continents</b>

            <section root id="europe">
                <b>Europe</b>

                <div id="about">
                    <b>About Europe</b>
                    <ul>
                        <li>Fact1</li>
                        <li>Fact2</li>
                    </ul>
                </div>

                <div id="countries">
                    <b>Countries in Europe</b>
                    <ul>
                        <li>Country1</li>
                        <li>Country2</li>
                    </ul>
                </div>
            </section>

            <section root id="africa">
                <b>Africa</b>

                <div id="about">
                    <b>About Africa</b>
                    <ul>
                        <li>Fact1</li>
                        <li>Fact2</li>
                    </ul>
                </div>

                <div id="countries">
                    <b>Countries in Africa</b>
                    <ul>
                        <li>Country1</li>
                        <li>Country2</li>
                    </ul>
                </div>
            </section>

        </article>

    </body>
    ```

    Note that Scoped HTML in the current implementation of CHTML is making do with the `scoped:id` attribute instead of the desired `id` attribute - just to maintain the current state of validity of HTML documents. As seen above, what we really want is have the `id` attribute scoped to its closest *root* (or the document root where no *root* context is found). This change to how IDs are interpreted will give us a system of IDs that are only contextually unique, eradicating the naming wars - all without necessarily putting pre-CHTML websites at risk as their lack of *roots* would keep their IDs associated with the document root.

    Now, deeply Scoped IDs may even be queried from the global scope using the path notation: `#continents / #europe / #about`. This also visually follows the information hierarchy of the document, whereas the flattened `#continents-europe-about` would be leaving no clue.

        
    Scoped IDs are especially useful in maintaining a structural API for applications. Above, our structural API would be:

    ```html
    continents
    |- europe
    |   |- about
    |   |- countries
    |- africa
        |- about
        |- countries
    ```

    We also have the *ScopedHTML* API that makes it easy to traverse these structural models.

    ```js
    // The regular querySelector() function would give us the #article element
    let continents = document.querySelector('#continents');

    // The new scopeTree DOM property would give us the structural parts
    let europe = continents.scopeTree.europe;
    let africa = continents.scopeTree.africa;

    // Deeply-nested...
    let aboutAfrica = continents.scopeTree.africa.scopeTree.about;
    ```

    Now, much structural guesswork and inefficient DOM queries would have been avoided with a simple, bankable structural API.

    **Scope Selectors – (Coming soon to the CHTML at Web-Native):**

    + The regular ID selector `#` will now respect scope boudaries, without necessarily breaking pre-CHTML documents as these have only one scope.
    + The forward slash `/` will be used to denote a scope boundary.
    + Two new query selectors (`scopeSelector()` and `scopeSelectorAll()`) will now be created, or the regular `querySelector()` and `querySelectorAll()` DOM methods will now be upgraded, to support Scope Selectors.

    [Read the full Scoped HTML docs](https://docs.web-native.dev/chtml/v060/specs/scoped-html)

+ **Scoped CSS** - Stylesheets that are scoped to their containing element – actually, the return of scoped CSS redesigned to support *Scope Selectors*.

    ```html
    <div root class="article">
        <style scoped>
            :root {
                color: red;
            }
            .wrapper {}
            #title {
                font-weight: bold;
            }
            #content {
                font-weight: normal;
            }
        </style>
        <div class="wrapper">
            <div scoped:id="title"></div>
            <div scoped:id="content"></div>
        </div>
    </div>
    ```

    [Read the full Scoped CSS docs](https://docs.web-native.dev/chtml/v060/specs/scoped-css)

+ **Scoped JS** - Scripts that run within the scope of their containing element, with automatic observability inbuilt. Scoped JS is as fundamentally different from regular scripts as module scripts are, this time, to serve as everything for presentational logic.

    We define a *scoped script* with the special `text/scoped-js` MIME type. (What I really would love is simply using the `scoped` Boolean attribute, as in the case of scoped CSS.)

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

    The awesome thing about scoped scripts is that they are isolated from the global document scope with the `this` reference bound to their containing element.

    It is also possible to bind external values to a scoped script, usually a data object from an application. 

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
        </script>
    </div>
    ```

    Scoped JS also features automatic obervability that keeps the UI in sync with bound values.

    [Read the full Scoped JS docs](https://docs.web-native.dev/chtml/v060/specs/scoped-js)

+ **HTML Transport** - An import/export system that lets us define, extend, and distribute HTML components – now, the platform itself as an UI component framework.

    HTML Transport uses existing HTML features to make a feature-rich system of defining and reusing HTML snippets.

    
    ```html
    <head>
        <template is="html-bundle">

            <div namespace="export/one"></div>
            <div namespace="export/two"></div>

        </template>
    </head>
    ```

    These *exports* easily importable and can be placed anywhere in the main document.

    ```html
    <body>
        <html-import namespace="export/one"></html-import>
        <html-import namespace="export/two"></html-import>
    </body>
    ```

    And in a script, we could programmatically import them.

    ```js
    let import1 = HTMLTransport.import('export/one');
    ```

    Taking things beyond imports and exports, HTML Transport sets a new bar in code organization, extensibility and composability.

    [Read the full HTML Transport docs](https://docs.web-native.dev/chtml/v060/specs/html-transport)
