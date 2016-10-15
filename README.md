# Typescript - React - Redux - Electron Boilerplate

Boilerplate project for building an electron app using React, Redux, Electron and Typescript.

[![Build status](https://ci.appveyor.com/api/projects/status/o5rekt4awy1k8xj5/branch/master?svg=true)](https://ci.appveyor.com/project/xwvvvvwx/typescript-boilerplate/branch/master)
[![Build status](https://travis-ci.org/xwvvvvwx/typescript-react-electron-boilerplate.svg?branch=master)](https://travis-ci.org/xwvvvvwx/typescript-react-electron-boilerplate)

## Contents
* [Features](#features)
* [Install](#install)
* [Run](#run)
* [Code Organization](#code-organization)
* [CSS](#css)
* [Adding Type Definitions](#adding-type-definitions)
* [Testing](#testing)
* [Packaging](#packaging)
* [Linting](#linting)

## Features

- Hot module reloading
- Installers built using [`electron-builder`](https://github.com/electron-userland/electron-builder)
- CI integration with [`travis`](https://travis-ci.org/) and [`appveyor`](https://www.appveyor.com/)
- Unit testing with [`mocha`](https://mochajs.org/) / [`chai`](http://chaijs.com/) / [`enzyme`](https://github.com/airbnb/enzyme)
- [`React`](https://github.com/facebook/react-devtools) / [`Redux`](https://github.com/gaearon/redux-devtools) / [`Electron`](https://github.com/electron/devtron) devtools
- linting with [`tslint`](https://palantir.github.io/tslint/) and [`stylelint`](https://github.com/stylelint/stylelint)

## Install

Clone the repo and install deps:
```
$ git clone https://github.com/xwvvvvwx/typescript-react-electron-boilerplate.git your-project-name
$ cd your-project-name && npm install
```

## Run

Run these two commands simultaneously in separate console tabs.
```
$ npm run server
$ npm start
```

**Notes**
- Uses [webpack](https://webpack.github.io/)
- Hot Module Replacement with vanilla HMR (see [Hot Reloading In React](https://medium.com/@dan_abramov/hot-reloading-in-react-1140438583bf#.389tj16hj) for details on trade offs)
- Typescript code is processed with [`awesome-typescript-loader`](https://github.com/s-panferov/awesome-typescript-loader)
- Redux / React devtools are automatically installed with [electron-devtools-installer](https://github.com/GPMDP/electron-devtools-installer)
- Electron devtools (devtron) automatically installed with [electron-debug](https://github.com/sindresorhus/electron-debug)
- Notifications of failed builds using [webpack-notifier](https://www.npmjs.com/package/webpack-notifier)

## Code organization

- Uses the [two package.json strucure](https://github.com/electron-userland/electron-builder#two-packagejson-structure) recomened by [`electron-builder`](https://github.com/electron-userland/electron-builder)
- `src/` contains sourcecode. This source is passed through webpack to generate the actual application.
- `app/` contains static resources (`main.js` and `package.json`) as well as webpack build artifacts (`bundle.js`, `index.html`)
- `install-resources/` contains images / resources for the installers
- Application entry point is `app/main.js`
- This loads `app/index.html` (generated by webpack)
- This calls `src/index.tsx` (generates store / handles HMR wrapping)
- This loads `src/components/App.tsx` (root component)
- Store is built from `src/reducers/main.ts`
- Modules can be imported using absolute paths (`src` is the root)

## CSS

- CSS is processed with [postcss](https://github.com/postcss/postcss)
- The [cssnext](http://cssnext.io/) plugin is installed
- CSS has local scope (each component can have it's own CSS file)
- Global variables are defined in `src/global-css-vars`
- Global variables are passed to css files using [postcss-simple-vars](https://github.com/postcss/postcss-simple-vars)

**Using CSS in a React Component**

- Typescript gets upset if you try and import css files, so you can bypass the typechecker by using `require` instead

```css
/* style.css */
.className {
    color: red;
}
```

```typescript
/* component.tsx */
const styles = require('./styles.css')

const Component: React.StatelessComponent<{}>  = () => (
  <p className={styles.className}>
    Hello, world!
  </p>
)
```

**Using a global var in a css file**

- variables are prefixed with `$`
```css
.className {
    font-family: $body-font
}
```

**Installing new postcss plugins**

1. install the plugin with npm
2. Require the plugin in `webpack.config.js`
3. Add the plugin to the list of postcss plugins in the webpack config

```javascript
var plugin = require('post-css-plugin-name')

module.exports = {
  /*
  webpack config....
  */
  postcss: function () {
    return {
      plugins: [
        // add new plugins here
        plugin()
      ]
    };
  }
}
```


## Adding type definitions

- Uses typescript 2
- Type definitions provided by a project are discovered automatically (e.g. `redux`)
- Community type definitions are installed with npm (see [The Future of Type Definition Files](https://blogs.msdn.microsoft.com/typescript/2016/06/15/the-future-of-declaration-files/))

To add a new type definition file run:
```
$ npm i -D @types/<PACKAGE_NAME>
```

## Testing

**Run tests**:<br>
```
$ npm run test
$ npm run test:watch
```

**Writing Tests**
- any file under `src` of the form `*.spec.ts` or `*.spec.tsx` will be executed
- Tests are run using [mocha](https://mochajs.org/) running on [ts-node](https://github.com/TypeStrong/ts-node).
- Assertions can be written using [chai](http://chaijs.com/).
- React components can be tested using [enzyme](http://airbnb.io/enzyme/index.html).
- See [actionCreators.spec.ts](https://github.com/xwvvvvwx/typescript-boilerplate/blob/master/src/actions/test/actionCreators.spec.ts) for examples of pure TS tests.
- See [components/Todo/tests.spec.tsx](https://github.com/xwvvvvwx/typescript-react-electron-boilerplate/blob/master/src/components/Todo/tests.spec.tsx) for examples of react component testing.

## Packaging

**Package App**
```
$ npm run build-mac
$ npm run build-linux
$ npm run build-win
$ npm run build
```

**Notes**
- Installers are output to the dist directory
- Installer config lives in the application [`package.json`](https://github.com/xwvvvvwx/typescript-react-electron-boilerplate/blob/master/package.json)
- `npm run build` builds installer for the platform that it is executed from
- Uses [`Electron Builder`](https://github.com/electron-userland/electron-builder)
- Installers use [`Squirrel`](https://github.com/Squirrel)

**Platforms**
- On OSX a zip and dmg are generated
- On Windows a .exe installer is generated
- On Linux a deb and AppImage are generated

## Linting

**Lint Code**
```
$ npm run lint
```

**Notes**
- Uses [tslint](http://palantir.github.io/tslint/) for typescript
- Uses [stylelint](https://github.com/stylelint/stylelint) for css
