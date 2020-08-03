# Scoped HTML

Scoped HTML is a DOM feature that let's an element establish its own naming context for descendant elements. It makes it possible to keep IDs out of HTML's global namespace and gives us a document that is structured as a hierarchy of *scopes* and *subscopes*.

## On this page:
+ [Convention](#convention)
+ [Scope API](#scope-api)
  + [Scope Observability](#scope-observability)

## Convention

Scopes are designated with the `root` Boolean attribute.

```html
<div root>
    <div scoped:id="..."></div>
</div>
```

***Scopes* and *Subscopes***

Below is scopes at scale:

```html
<body>

    <article root scoped:id="continents">
        <b>Continents</b>

        <section root scoped:id="europe">
            <b>Europe</b>

            <div scoped:id="about">
                <b>About Europe</b>
                <ul>
                    <li>Fact1</li>
                    <li>Fact2</li>
                </ul>
            </div>

            <div scoped:id="countries">
                <b>Countries in Europe</b>
                <ul>
                    <li>Country1</li>
                    <li>Country2</li>
                </ul>
            </div>
        </section>

        <section root scoped:id="africa">
            <b>Africa</b>

            <div scoped:id="about">
                <b>About Africa</b>
                <ul>
                    <li>Fact1</li>
                    <li>Fact2</li>
                </ul>
            </div>

            <div scoped:id="countries">
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

Above, what we get is simply a hierarchy of scopes:

```html
continents
|- europe
|   |- about
|   |- countries
|- africa
    |- about
    |- countries
```

## Scope API

A hierarchy of scopes makes it possible to have a *structural API*, or a UI component model, that an application can bank on. Scoped HTML exposes a new DOM property `scopeTree` for accessing scope trees.

```js
// The regular querySelector() function would give us the #article element
let continents = document.querySelector('#continents');

// The new scopeTree DOM property would give us the structural parts
let europe = continents.scopeTree.europe;
let africa = continents.scopeTree.africa;

// Deeply-nested...
let aboutAfrica = continents.scopeTree.africa.scopeTree.about;
```

Now, much naming wars, structural guesswork, and inefficient DOM queries can be eliminated with a simple, bankable API.

### Scope Observability

An element's `.scopeTree` property is implemented as a live object to capture the element's scope tree in real time. CHTML also supports the [Observer API](https://docs.web-native.dev/observer) for change detection; `Obs.observe()` can thus be used to observe additions and removals on the scope tree.

```js
Obs.observe(element.scopeTree, changes => {
    console.log(changes.map(change => change.name));
});
```