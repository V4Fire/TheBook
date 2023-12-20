# Quick Start

This guide will teach you how to create and use V4Fire components.

## Creating a component

```
src
└── components
  └── b-example
    ├── b-example.ts
    ├── b-example.ss
    ├── b-example.styl
    └── index.js
```

It's recommended to create components inside the `src/components` directory.
Typical component consist of these files:

- b-example.ts - class declaration of the component
- b-example.ss - component's template
- b-example.styl - component's styles
- index.js - declares component's parent, dependencies, libraries

> "b" stands for a block from the BEM methodology

**Important**

Components are declared globally, so there cannot be 2 components with the same name.
It's recommended to use prefix in a component's name, for example: `b-main-head`, `b-catalog-dialog`, etc.

### index.js

The `index.js` is the most important file of a component: it allows to statically
analyze all components of the project and their dependencies.

All components must extend base component `i-block` and should specify
components used in their template as dependencies, therefore `index.js`
should look like that: 

```js
package('b-example')
  .extends('i-block')
  .dependencies('b-button');
```

### b-example.ts

A component is declared using a class, which is decorated with the `@component` decorator.
To define props of the component the `@prop` decorator is used.
Reactive properties, which will force component to re-render, are defined using the `@field` decorator.

```ts
import iBlock, { component, prop, field } from 'components/super/i-block/i-block';
export * from 'components/super/i-block/i-block';

@component()
export default class bExample extends iBlock {
  @prop(Number)
  a: number;

  @prop(Number)
  b: number;

  @prop({
    type: String,
    required: false,
    validator: (value: string) => ['+', '-', '/', '*'].includes(value)
  })

  op: string = '+';

  @field()
  result: number;

  calc(): void {
    this.result = Function('a', 'b', `return a ${this.op} b`)(this.a, this.b);
  }
}
```

### b-example.ss

Template of the component is defined using a [Snakeskin](http://snakeskintpl.github.io/docs/index.html) language.

```ss
- namespace [%fileName%]

- include 'components/super/i-block'|b as placeholder

- template index() extends ['i-block'].index
  - block body
    < b-button @click = calc
      Calculate!

    /// It will become a <div class="b-example__result"></div> 
    < .&__result
      /// Runtime interpolation is declared using double curly brackets {{ ... }} 
      a {{ op }} b = {{ result }}
```

The main template of the component must be named `index`. 

> It is possible to declare multiple templates for a component,
> but only the `index` template will be used for the `render` function of the component.

It's important that component's template must have the `namespace` directive with `[%fileName%]` macro,
which will automatically create a namespace `b-example`.

> We use Vue.js to bind component's data to the template. It means that you
> can use standard Vue directives like `v-for`, `v-if`, `v-model`, etc.

### b-example.styl

Styles of the component are defined using the [Stylus](https://stylus-lang.com/) language
with custom [plugin for inheritance](https://github.com/pzlr/stylus-inheritance).

```styl
@import "components/super/i-block/i-block.styl"

// This object is associated with the component and can be inherited.
// Properties from i-block will be added to this object.
$p = {
  color: #F00
}

b-example extends i-block
  &__result
    color $p.color
```

## Using a component

Now you can easily use created component inside other component, for example:

```ss
/// src/pages/p-root/p-root.ss

- namespace [%fileName%]

- include 'components/super/i-static-page/i-static-page.component.ss'|b as placeholder

- template index() extends ['i-static-page.component'].index
  - block body
    < b-example :a = 1 | :b = 2 | :op = '-'
```

Don't forget to add it to `p-root` dependencies:

```js
// src/pages/p-root/index.js
package('p-root')
  .extends('i-static-page')
  .dependencies('b-example');
```

> "p" stands for page