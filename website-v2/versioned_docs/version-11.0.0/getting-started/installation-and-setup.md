---
id: installation-and-setup
title: Installation and Setup
slug: /getting-started/installation-and-setup/
---

import DocsRating from '../../../src/core/DocsRating';
import {OssOnly, FbInternalOnly} from 'internaldocs-fb-helpers';


## Installation

Install React and Relay using `yarn` or `npm`:

```sh
yarn add react react-dom react-relay
```

## Set up Relay with a single config file

The below configuration of `babel-plugin-relay` and `relay-compiler` can be applied using a single configuration file by
using the `relay-config` package. Besides unifying all Relay configuration in a single place, other tooling can leverage
this to provide zero-config setup (e.g. [vscode-apollo-relay](https://github.com/relay-tools/vscode-apollo-relay)).

Install the package:

```sh
yarn add --dev relay-config
```

And create the configuration file:

```javascript
// relay.config.js
module.exports = {
  // ...
  // Configuration options accepted by the `relay-compiler` command-line tool and `babel-plugin-relay`.
  src: "./src",
  schema: "./data/schema.graphql",
  exclude: ["**/node_modules/**", "**/__mocks__/**", "**/__generated__/**"],
}
```

## Set up babel-plugin-relay

Relay Modern requires a Babel plugin to convert GraphQL to runtime artifacts:

```sh
yarn add --dev babel-plugin-relay graphql
```

Add `"relay"` to the list of plugins your `.babelrc` file:

```javascript
{
  "plugins": [
    "relay"
  ]
}
```

Please note that the `"relay"` plugin should run before other plugins or
presets to ensure the `graphql` template literals are correctly transformed. See
Babel's [documentation on this topic](https://babeljs.io/docs/plugins/#pluginpreset-ordering).

Alternatively, instead of using `babel-plugin-relay`, you can use Relay with [babel-plugin-macros](https://github.com/kentcdodds/babel-plugin-macros). After installing `babel-plugin-macros` and adding it to your Babel config:

```javascript
const graphql = require('babel-plugin-relay/macro');
```

If you need to configure `babel-plugin-relay` further (e.g. to enable `compat` mode), you can do so by [specifying the options in a number of ways](https://github.com/kentcdodds/babel-plugin-macros/blob/master/other/docs/user.md#config-experimental).

For example:

```javascript
// babel-plugin-macros.config.js
module.exports = {
  // ...
  // Other macros config
  relay: {
    compat: true,
  },
}
```

## Set up relay-compiler

Relay's ahead-of-time compilation requires the [Relay Compiler](../../guides/compiler/), which you can install via `yarn` or `npm`:

```sh
yarn add --dev relay-compiler
```

This installs the bin script `relay-compiler` in your node_modules folder. It's recommended to run this from a `yarn`/`npm` script by adding a script to your `package.json` file:

```js
"scripts": {
  "relay": "relay-compiler --src ./src --schema ./schema.graphql"
}
```

or if you are using jsx:

```js
"scripts": {
  "relay": "relay-compiler --src ./src --schema ./schema.graphql --extensions js jsx"
}
```

Then, after making edits to your application files, just run the `relay` script to generate new compiled artifacts:

```sh
yarn run relay
```

Alternatively, you can pass the `--watch` option to watch for file changes in your source code and automatically re-generate the compiled artifacts (**Note:** Requires [watchman](https://facebook.github.io/watchman) to be installed):

```sh
yarn run relay --watch
```

For more details, check out our [Relay Compiler docs](../../guides/compiler/).

## JavaScript environment requirements

The Relay Modern packages distributed on NPM use the widely-supported ES5
version of JavaScript to support as many browser environments as possible.

However, Relay Modern expects modern JavaScript global types (`Map`, `Set`,
`Promise`, `Object.assign`) to be defined. If you support older browsers and
devices which may not yet provide these natively, consider including a global
polyfill in your bundled application, such as [core-js][] or
[@babel/polyfill](https://babeljs.io/docs/usage/polyfill/).

A polyfilled environment for Relay using [core-js][] to support older browsers
might look like:

```js

require('core-js/es6/map');
require('core-js/es6/set');
require('core-js/es6/promise');
require('core-js/es6/object');

require('./myRelayApplication');

```

[core-js]: https://github.com/zloirock/core-js


<DocsRating />
