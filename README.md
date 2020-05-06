# eslint-plugin-import-alias

An ESLint rule for forcing import path aliases.

This is a fork of <https://github.com/steelsojka/eslint-import-alias> by Steven Sojka. The original project seems to be abandoned.

## Installation

```shell
yarn add -D @matsgottenbos/eslint-plugin-import-alias
```

## Usage

```javascript
import { test } from '~/test'; // valid
import { test } from '@src/test'; // valid
import { test } from './test'; // invalid
import { test } from '../test'; // invalid

// Optional relative depth can be specified to allow unaliased paths when they do not go up to parent directories higher than the relative depth
import { test } from './test'; // valid, { relativeDepth: 0 }
import { test } from './test/test'; // valid, { relativeDepth: 0 }
import { test } from '../test'; // valid, { relativeDepth: 1 }
import { test } from '../../test'; // invalid, { relativeDepth: 1 }
```

## Configure

```javascript
{
  "rules": {
    "import-alias/import-alias": [
      "error",
      {
        "relativeDepth": 0,
        "aliases": [
          // Example: rewrites imports pointing to `src/modules/app/index.js` to `@src/modules/app/index.js`
          { "alias": "@src", "matcher": "^src" },

          // Example: rewrites imports pointing to `test/unit/modules/app` to `@test/modules/app`
          { "alias": "@test", "matcher": "^test\/unit" },

          // Example: rewrites imports pointing to `test/e2e/modules/app` to `@testRoot/e2e/modules/app`
          { "alias": "@testRoot", "matcher": "^(test)\/e2e" },
        ]
      }
    ]
  }
}
```

Aliases can be configured to fix the path and rewrite to an aliased path. Each alias has the alias text and a regex matcher that will match against the resolved path from the root directory of the eslint process (usually the project root). For example, if the resolved file path is in the 'src' folder (src/modules/app/test) then 'src' will be replaced with '@src'.
Optionally, you can define a capture group to replace only the part within the capture group, but still match against the whole regex.

A 'rootDir' can be defined to resolve the file paths from. This defaults to `process.cwd()`. In a lot of cases, this is already the project root in most cases.

```javascript
{
  "rules": {
    "import-alias/import-alias": [
      "error",
      {
        "relativeDepth": 0,
        "rootDir": __dirname,
        "aliases": [
          // Example: rewrites imports pointing to `src/modules/app/index.js` to `~/modules/app/index.js`
          { "alias": "~/", "matcher": "^" },

          // Example: rewrites imports pointing to `src/modules/app/index.js` to `@src/modules/app/index.js`
          { "alias": "@src", "matcher": "^src" },
        ]
      }
    ]
  }
};
```
