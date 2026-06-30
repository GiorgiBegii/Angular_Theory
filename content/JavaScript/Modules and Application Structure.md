# Modules and Application Structure

This note groups the concepts that explain how JavaScript and TypeScript code is split, loaded, scoped, and organized in Angular projects.

## ESM

ESM means ECMAScript Modules, the standard JavaScript module system.

It allows one file to export code and another file to import it.

```ts
export function sum(a: number, b: number) {
  return a + b;
}
```

```ts
import { sum } from './math';
```

In the browser, ESM can run with:

```html
<script type="module" src="main.js"></script>
```

Important points:

- modules have their own scope
- modules run in [[Modules and Application Structure#Strict Mode|Strict Mode]] automatically
- imports/exports are static and analyzable
- bundlers can use ESM for tree shaking
- Angular code is written as ESM and then bundled for the browser

## Modules

Modules split code into separate files with clear public APIs.

Simple idea:

```text
file exports something
another file imports it
```

Good modules:

- expose only what other files need
- hide internal implementation details
- avoid circular dependencies
- keep related code together
- make application boundaries easier to understand

Example:

```ts
// user-api.ts
export class UserApi {}
```

```ts
// user-page.ts
import { UserApi } from './user-api';
```

In Angular, modules usually mean JavaScript/TypeScript modules, not necessarily `NgModule`.

## Strict Mode

Strict mode is a JavaScript mode that makes the language stricter and safer.

It helps catch silent mistakes and prevents some risky JavaScript behavior.

### How To Enable

Add `"use strict"` at the top of a file or function:

```js
"use strict";

function saveUser() {
  // strict mode code
}
```

ES modules use strict mode automatically.

```html
<script type="module" src="main.js"></script>
```

### What It Changes

#### 1. No Accidental Global Variables

Without strict mode, assigning to an undeclared variable can create a global variable.

```js
name = "Ana";
```

In strict mode, this throws an error.

```js
"use strict";

name = "Ana"; // ReferenceError
```

#### 2. `this` Can Be `undefined`

In a regular standalone function, `this` is `undefined` in strict mode.

```js
"use strict";

function showThis() {
  console.log(this);
}

showThis(); // undefined
```

Without strict mode in browsers, `this` may point to `window`.

#### 3. Safer Function Parameters

Duplicate parameter names are not allowed.

```js
"use strict";

function sum(a, a) {
  return a;
}

// SyntaxError
```

#### 4. Some Silent Errors Become Real Errors

Strict mode throws errors for operations that normally fail silently.

```js
"use strict";

const user = {};
Object.freeze(user);

user.name = "Ana"; // TypeError
```

### Angular / TypeScript Context

Modern Angular apps are written with TypeScript modules, and modules run in strict mode automatically after compilation/bundling.

This means Angular code usually behaves like strict JavaScript by default.

Strict mode matters especially for:

- `this` behavior in callbacks
- avoiding accidental globals
- safer object mutation
- cleaner module code
- better runtime error visibility

## Application Structure

Application structure means how project files and folders are organized so the codebase can grow without becoming hard to navigate.

Common Angular structure:

```text
src/
+-- app/
|   +-- core/
|   +-- shared/
|   +-- features/
|   +-- app.config.ts
|   +-- app.routes.ts
+-- main.ts
```

Common folders:

- `core` - singleton services, app-level providers, interceptors, guards
- `shared` - reusable components, pipes, directives, utilities
- `features` - feature-specific pages, components, services, routes, state
- `data-access` - API services and data fetching logic
- `ui` - presentational components

Good structure keeps:

- feature code close together
- shared code reusable but not overloaded
- dependencies flowing in clear directions
- implementation details hidden behind public APIs
- routing, state, forms, and HTTP boundaries understandable

## Path Aliases

Path aliases replace long relative imports with stable import paths.

Without aliases:

```ts
import { UserApi } from '../../../core/api/user-api';
```

With aliases:

```ts
import { UserApi } from '@core/api/user-api';
```

They are usually configured in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "paths": {
      "@core/*": ["src/app/core/*"],
      "@shared/*": ["src/app/shared/*"],
      "@features/*": ["src/app/features/*"]
    }
  }
}
```

Benefits:

- easier imports
- less fragile folder movement
- clearer architecture boundaries
- cleaner feature/module imports

Avoid aliases that hide bad architecture. If everything imports everything, aliases only make the problem prettier.

## Short Answer

ESM is the standard JavaScript module system. Modules split code into files with clear imports and exports. ESM runs in strict mode automatically, giving safer runtime behavior. Application structure organizes code by features, shared utilities, and core services. Path aliases make imports cleaner and help enforce architectural boundaries.

## Related Notes

- [[Language Concepts#this|this]]
- [[Functions]]
- [[Angular Architecture#Dependency Boundaries|Dependency Boundaries]]
- [[What is Angular]]
- [[Angular Router]]
