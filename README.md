## Description
A JS minifier with more aggressive set of default options. Options are configurable
in your project's `package.json`.

> **Interesting side effect** This package increases security by removing part of
your server side code which is normally included by the Meteor's standard minifier.

## Usage
```
meteor remove standard-minifier-js
meteor add ssrwpo:uglifyjs2
```

## Configuration
### Default options
The following default options are activated:
```js
uglifyjs2: {
  deadCodes: [
    '_meteor.Meteor.isServer',
    'Meteor.isServer'
  ],
  fileRemoval: [
    'packages/ddp-server.js',
    'packages/shell-server.js',
    'packages/ssrwpo_uglifyjs2.js'
  ],
  packageDebug: false,
  options: {
    fromString: true,
    compress: {
      properties: true,
      dead_code: true,
      drop_debugger: true,
      conditionals: true,
      comparisons: true,
      evaluate: true,
      booleans: true,
      loops: true,
      unused: true,
      hoist_funs: true,
      if_return: true,
      join_vars: true,
      cascade: true,
      collapse_vars: true,
      negate_iife: true,
      pure_getters: true,
      drop_console: true,
      keep_fargs: false,
      keep_fnames: false,
      passes: 2,
      global_defs: {
        UGLYFYJS_DEAD: false
      }
    }
  }
}
```

### Dead code elimination
`deadCodes` is a list of `String`s is used for text replacement as a global definition
transformed into `UGLYFYJS_DEAD` allowing minifications with dead code removal.
This acts as macro for code removal in your project.

In your `package.json`, add the `deadCodes` option to additional dead code removals:
```json
"uglifyjs2": {
  ...
  "deadCodes": ["Meteor.isServer", ...],
  ...
}
```

By default, the list only contains: `['Meteor.isServer']`.

### UglifyJS2 options
Overwrite the default `options` to add or remove UglifyJS2's options.

In your `package.json`, add the `options` for tweaking UglifyJS2's options:
```json
"uglifyjs2": {
  ...
  "options": {
    "fromString": true,
    "compress": {
      ...
      "properties": false,
      ...
    }
  }
  ...
}
```

> :warning: Meteor relies on text analysis when using UglifyJS2. Therefore,
  removing the `fromString` option leads to unexpected results.

### Development minification
Meteor's standard minifier skip minification while in development. As dead code
removal could affect your project's behavior, this package allows you to minify
your code while developping.

In your `package.json`, add the `development` option to activate minification
while in development mode:
```json
"uglifyjs2": {
  ...
  "development": true,
  ...
}
```

### Package debug options
This adds some debug informations while processing production build. It helps
tracking the files that will get included into the web bundle JS file.

For activating it, simply add the `packageDebug` option in your `package.json`.
```json
"uglifyjs2": {
  ...
  "packageDebug": true,
  ...
}
```

### File removals
Meteor injects files in the client that doesn't belong to the client. This is
used by Meteor to display informations on used packages in the server. Removing
these informations increase security and removes some unecessary lost bytes.

Some defaults files are removed automatically with this package and you can
tweak these removals using `fileRemoval` options in your `package.json`.
```json
"uglifyjs2": {
  ...
  "fileRemoval": [
    "packages/ddp-server.js",
    "packages/shell-server.js",
    "packages/ssrwpo_uglifyjs2.js"
  ],
  ...
}
```

### Aggressive minification
By default, minification is done file by file. Aggressive minification aggregates
all files and then apply the minification allowing UglifyJS2 to go a little bit
further in its optimization.

Turned off by default, this option can be added using the `aggressive` flag
 in your `package.json`.
 ```json
 "uglifyjs2": {
   ...
   "aggressive": true,
   ...
 }
 ```

## Package development
### Exported symbols
This package exports a function `loadPackageJson` that parses the content
of then NPM's `package.json` file of a Meteor project.

### Dependencies & demo project

At the root of the repository:

```sh
yarn install
cd demo
yarn install
# Launch the demo in development mode
yarn dev
# Launch the demo in production mode
yarn prod
```

## Tips
* My build process is stucked at "Minifying app code": `meteor reset`.
* Start with a small project when choosing your options.
* Don't add `reduce_vars` as option as it prevents Meteor from starting.
* Use the default options as a canevas for experimenting additional UglifyJS2 options.

## Links
* [UglifyJS2](https://github.com/mishoo/UglifyJS2)
