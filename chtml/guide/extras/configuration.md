# CHTML Configuration

The Chtml API relies on a certain `params` object to work. This object comes preset with default values, but it can be modified to further customize CHTML to a usecase.

```javascript
import {params} from '@web-native-js/chtml';
```

Below are the parameters and what they do:

```javascript
{
    // Sets the rendering environment
    env: '', // server | client

    // The family of attributes used in markup
    attrMap: {

        // The "data-tree" attribute
        hint: 'data-tree',

        // The "data-namespace" attribute
        namespace: 'data-namespace',

        // A component's "data-role" attribute
        superrole: 'data-role',

        // A node's "data-role" attribute
        subrole: 'data-role',

        // The attribute for defining the CHTML template elements
        bundle: 'chtml-bundle',
    },

    // Selectors for fidning CHTML's special elements
    tagMap: {

        // The selector for a component's JSEN script element.
        // <script type="text/scoped-js>...</script>
        jsen: 'script[type="text/scoped-js"]',

        // The selector for the document's bundle elements.
        // The "is" attribute must also correspond to the params.attrMap.bundle settign above
        // <template is="chtml-bundle>...</template>
        bundle: 'template[is="chtml-bundle"]',

        // The selector for the CHTML import element
        // <chtml-import></chtml-import>
        import: 'chtml-import',
    },

    // A componet's nodeList property
    // component.tree.nodeName
    treeProperty:'tree',

    // // A component's model property
    // The component.bind() method relies on tjis property
    modelProperty:'model',

    // Specifies whether to remove a component's JSEM script element from the DOM while
    // an instance remains mounted on the component.
    // When removed, the script element is returned back on unmouting the component instance
    hideDataBlockScript:true,
}
```

The object above is used globally across modules in CHTML. It is also possible to override these values at the component level. The `params` object passed to instantiate a component is used to achieve this as it would be inheriting from thie global `params` object.

```javascript
let component = new Chtml(el, {treeProperty: 'nodes'});
let node1 = Reflex.get(component.nodes, 'node1');
let node2 = component.get('node2');
```

