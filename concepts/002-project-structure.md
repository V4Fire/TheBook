# Project structure

- `src` - application source code
- `build` - scripts and modules used for build process
- `config` - configuration: build, runtime, etc.
- `tests` - test helpers and configuration
- `tools` - scripts and useful tools, i.e.: custom linters, git hooks scripts, etc.

## src

**Layers of the application:**

```
entries/
pages/
...features/
  pages/
  components/
components/
models/
core/
```

The layers are standardized across all projects and vertically arranged.
Modules of the layer can only interact with modules from the layers strictly below.
There are 6 standard layers (bottom to top):

1. `core` - reusable functionality, detached from the specifics of the project/business.
2. `models` - business logic.
3. `components` - shared ui components.
4. `...features` - there can be as many feature layers as you need.
5. `pages` - pages of the application constructed from components and features.
6. `entries` - entries for webpack build, which are used to control the chunks of the application.

**Example of feature layers:**

```
search/
  components/
    b-search-input/
    b-search-dropdown/
  pages/
    p-search-results/
catalog/
  components/
    b-catalog-card/
    b-catalog-list/
  pages/
    p-catalog-categories/
    p-catalog-offers/
service-worker/
  modules/
  index.ts
scripts/
  detect.js
  chat-widget.js
```

Features can have custom structure, for example: workers, arbitrary scripts, etc.

**Important:** feature layers must be isolated from each other.

**Special directories:**

1. `routes` - routes of the application.
2. `assets` - icons, images, fonts, etc.

## build

TODO

## config

We use [config](https://www.npmjs.com/package/config/v/1.31.0) library for build
and runtime configuration. 

Config directory consists of three files:

- default.js
- development.js
- production.js

## tests

TODO

## tools

TODO