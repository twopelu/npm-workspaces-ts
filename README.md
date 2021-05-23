# Npm Workspaces

This project demonstrates the use of workspaces introduced in Npm 7.

See official docs in https://docs.npmjs.com/cli/v7/using-npm/workspaces.

## Project setup

The structure of the project is as follows:

```
.
+-- package.json
`-- a
   `-- package.json
`-- b
   `-- package.json
```

The main folder contains a index.ts file which depends on a and b modules.

Both a and b modules are defined as workspaces in the main package.json:

```
"workspaces": [
    "a",
    "b"
],
```

Run `npm install` to build all the modules and create the symlinks:

```
.
+-- node_modules
|  `-- npm-workspaces-lib-a -> ../a
|  `-- npm-workspaces-lib-b -> ../b
+-- package.json
`-- a
   `-- package.json
`-- b
   `-- package.json
```

## Using TypeScript with Workspaces

To make workspaces work with TypeScript all modules have to be compiled previously.

Add the `npm run build --workspaces` to the npm prepare script in the package.json:

```
"prepare": "npm run build --workspaces && npm run build",
```

This will generate compiled JS code in the /dist folders.

## Imports and Exports

Make sure all the modules are commonjs and target ES2015 to compile:

```
{
  "compilerOptions": {
    "target": "ES2015",
    "module": "commonjs",
    "declaration": true,
    "outDir": "dist",
    "strict": true
  },
  "include": [
    "src/**/*"
  ]
}
```

And use this sintax to import or export functions as per ES Modules (???):

```
import { add } from 'npm-workspaces-lib-a';
```

```
export function add(a: number, b: number) {
    return a + b;
}
```

## Run and Test with Hot-Reaload

The main folder contains a server.ts script to check hot-reloading.

Install the **nodemon** package to have hot-realoaded when changes.

`npm install --save-dev nodemon`

Then follow these steps from main folder:

1. `cd a && tsc --watch`
2. `cd b && tsc --watch`
3. `tsc --watch`
4. `npm start`

Then make changes in a or b and see logs.
