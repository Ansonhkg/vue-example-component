# What is it?

A minimal guide to publish vue component to npm.

# Publish Vue component as NPM Package in 5 minutes.

- [What is it?](#what-is-it)
- [Publish Vue component as NPM Package in 5 minutes.](#publish-vue-component-as-npm-package-in-5-minutes)
- [Expected Usage](#expected-usage)
- [Steps](#steps)
  - [Folder strcture](#folder-strcture)
  - [package.json](#packagejson)
  - [./src/index.js](#srcindexjs)
  - [./build/rollup.config.js](#buildrollupconfigjs)
  - [./src/my-component.vue](#srcmy-componentvue)
- [Serve](#serve)
- [Build](#build)
- [Publish](#publish)

# Expected Usage

In your Vue app 
```html
<template>
    <MyComponent></MyComponent>
</template>

<script>
import MyComponent from '@ansonhkg/my-component'
default export {
    components:{
        MyComponent
    }
}
</script>
```

---

# Steps

## Folder strcture

Generate the following directory structure in your project folder.

```
package.json
build/
    rollup.config.js
src/
    index.js
    my-component.vue
dist/
```

## package.json

- Minimum setup for package.json. Replace `my-component` with your component name. 
- `"name": "my-component"` is referring to your npm package name that you are going to publish. It has to be unique, so normally I would prepend it with my username eg. `"name": "@ansonhkg/my-component"`.

```JSON
{
  "name": "@ansonhkg/my-component", // <== CHANGE THIS
  "version": "0.0.1",
  "main": "dist/my-component.umd.js", // <== CHANGE THIS
  "module": "dist/my-component.esm.js", // <== CHANGE THIS
  "unpkg": "dist/my-component.min.js", // <== CHANGE THIS
  "browser": {
    "./sfc": "src/my-component.vue" // <== CHANGE THIS
  },
  "scripts": {
    "build": "npm run build:umd & npm run build:es & npm run build:unpkg",
    "build:umd": "rollup --config build/rollup.config.js --format umd --file dist/my-component.umd.js", // <== CHANGE THIS
    "build:es": "rollup --config build/rollup.config.js --format es --file dist/my-component.esm.js", // <== CHANGE THIS
    "build:unpkg": "rollup --config build/rollup.config.js --format iife --file dist/my-component.min.js" // <== CHANGE THIS
  },
  "devDependencies": {
    "rollup": "^1.17.0",
    "rollup-plugin-buble": "^0.19.8",
    "rollup-plugin-commonjs": "^10.0.1",
    "rollup-plugin-vue": "^5.0.1",
    "vue": "^2.6.10",
    "vue-template-compiler": "^2.6.10"
  }
}
```

## ./src/index.js

This file provides an `install` function for Vue to detect if this indeed a Vue application, then install the component automatically.

```js
// Import vue component
import component from './my-component.vue'; // <== CHANGE THIS

// Declare install function executed by Vue.use()
export function install(Vue) {
	if (install.installed) return;
	install.installed = true;
	Vue.component('MyComponent', component); // <== CHANGE THIS
}

// Create module definition for Vue.use()
const plugin = {
	install,
};

// Auto-install when vue is found (eg. in browser via <script> tag)
let GlobalVue = null;
if (typeof window !== 'undefined') {
	GlobalVue = window.Vue;
} else if (typeof global !== 'undefined') {
	GlobalVue = global.Vue;
}
if (GlobalVue) {
	GlobalVue.use(plugin);
}

// To allow use as module (npm/webpack/etc.) export component
export default component; 
```

## ./build/rollup.config.js

Minimum setup for Rollup configuration.

```js
import commonjs from 'rollup-plugin-commonjs'; // Convert CommonJS modules to ES6
import vue from 'rollup-plugin-vue'; // Handle .vue SFC files
import buble from 'rollup-plugin-buble'; // Transpile/polyfill with reasonable browser support
export default {
    input: 'src/index.js', // Path relative to package.json
    output: {
        name: 'MyComponent', // CHANGE THIS
        exports: 'named',
    },
    plugins: [
        commonjs(),
        vue({
            css: true, // Dynamically inject css as a <style> tag
            compileTemplate: true, // Explicitly convert template to render function
        }),
        buble(), // Transpile to ES5
    ],
};
```

## ./src/my-component.vue

A sample component for the sake of demonstration.

```html
<template>
  <div class="container">This is an example component.</div>
</template>

<script>
export default {
  mounted() {
    console.log("Example component is mounted!");
  }
};
</script>

<style>
.container {
  background-color: red;
  padding: 10px;
  width: 100px;
  height: 100px;
  position: absolute;
}
</style>

```

# Serve

Install [@vue/cli](https://cli.vuejs.org/guide/installation.html) if you haven't done so. 

```
vue serve --open src/my-component.vue
```

# Build

```
yarn && yarn build
```

# Publish

```
# initial publish
npm publish --access public

# ...then bump patch version and publish
yarn version --patch && yarn publish
```

> If you don't have a NPM package account, you will need to [sign one up](https://www.npmjs.com/signup). Once you have signed up, you can use the `npm adduser` command to login from the terminal.

