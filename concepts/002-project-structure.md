# Project structure

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
There are 5 standard layers (bottom to top):

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
```

**Important:** feature layers must be isolated from each other.

**Special directories:**

1. `routes` - routes of the application.
2. `assets` - icons, images, fonts, etc.

## build

TODO

## config

TODO


## tests

TODO

## tools

TODO