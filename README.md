# Astro with Typescript 7

Typescript 7 has been great to use but Astro unfortunately still lacks full support, especially when running the `astro check` command from the @astrojs/check package.
Instead of having to revert your whole project back to Typescript 6, here is a hack/solution to make it work alongside with Typescript 7.

## Install Typescript 6

You'll first have to install the older version of typescript that works with Astro.

```bash
pnpm i -D @typescript/typescript6
```

## The Hack

- Run the `astro check` command.
- You'll get an error pointing to a file named `check.js`.
    - Location of error should end with the following: `node_modules/@astrojs/language-server/dist/check.js:184:19`
- Click on that file and go the first line inside the `initialize()` method of the `AstroCheck` class.

```js
this.ts = this.typescriptPath ? require(this.typescriptPath) : require("typescript");
```

- Replace that first line with the following:

```js
this.ts = require("@typescript/typescript6");
```

- Run the `check` command again.
- You might see another error pointing to a file named `createChecker.js`
    - Location of error should end with the following: `node_modules/@volar/kit/lib/createChecker.js:44:52`
- Click on that file and go to line 8 where it imports the typescript package:

```js
const ts = require("typescript");
```

- Replace that line with the following:

```js
const ts = require("@typescript/typescript6");
```

Et voila! You can now use Typescript 7 for your project while @astrojs/check uses Typescript 6.

**NOTE** - You probably will have to run these steps whenever you install a newer/different version of astro or @astrojs/check.
