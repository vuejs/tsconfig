# `@vue/tsconfig`

TSConfigs for Vue projects to extend.

Requires TypeScript >= 5.0. For TypeScript v4.5 to v4.9, please use [v0.1.x](https://www.npmjs.com/package/@vue/tsconfig/v/0.1.3).

[See below for the changes in v0.2.x.](#migrating-from-typescript--50)

## Installation

```sh
npm add -D @vue/tsconfig
```

## Usage

Add one of the available configurations to your `tsconfig.json`:

### The Base Configuration (Runtime-agnostic)

```json
"extends": "@vue/tsconfig/tsconfig.json"
```

### Configuration for Browser Environment

```json
"extends": "@vue/tsconfig/tsconfig.dom.json"
```

### Configuration for Node Environments

First install the base tsconfig and types for the Node.js version you are targeting, for example:

```sh
npm add -D @tsconfig/node18 @types/node@18
```

Then extend the Node.js tsconfig and the Vue tsconfig in your `tsconfig.json`:

```json
"extends": [
  "@tsconfig/node18/tsconfig.json",
  "@vue/tsconfig/tsconfig.json"
],
"compilerOptions": [
  "types": ["node"]
]
```

Make sure to place `@vue/tsconfig/tsconfig.json` *after* `@tsconfig/node18/tsconfig.json` so that it takes precedence.

## Migrating from TypeScript < 5.0

- The usage of base `tsconfig.json` is unchanged.
- `tsconfig.web.json` is now renamed to `tsconfig.dom.json`, to align with `@vue/runtime-dom` and `@vue/compiler-dom`.
- `tsconfig.node.json` is removed, please read the [Node.js section](#configuration-for-node-environments) above for Node.js usage.

Some configurations have been updated, which might affect your projects:

- `moduleResolution` changed from `node` to [`bundler`](https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#moduleresolution-bundler)
- The `lib` option in `tsconfig.dom .json` now includes `ES2020` by default.
  - Previously it was ES2016, which was the lowest ES version that Vue 3 supports.
  - Vite 4 transpiles down to ES2020 by default, this new default is to align with the build tool.
  - This change won't throw any new errors on your existing code, but if you are targeting old browsers and want TypeScript to throw errors on newer features used, you can override the `lib` option in your `tsconfig.json`:

    ```json
    {
      "extends": "@vue/tsconfig/tsconfig.dom.json",
      "compilerOptions": {
        "lib": ["ES2016", "DOM", "DOM.Iterable"]
      }
    }
    ```
