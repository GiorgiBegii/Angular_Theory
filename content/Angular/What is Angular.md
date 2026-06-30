Angular is a TypeScript-based frontend framework for building scalable web applications. It provides built-in solutions for routing, forms, dependency injection, and HTTP communication, allowing developers to focus on business logic instead of setting up infrastructure.

## App Startup

A basic Angular app involves these core files and entities:

- `main.ts` - bootstraps the Angular application.
- `app.component.ts` - root component class, usually `AppComponent`.
- `app.component.html` - root component template.
- `app.component.css` / `app.component.scss` - root component styles.
- `app.config.ts` - application-level configuration and providers in standalone Angular.
- `app.module.ts` - older NgModule-based application configuration.
- `index.html` - entry HTML file where Angular renders the app.
- `angular.json` - Angular workspace, build, serve, test, and asset configuration.
- `tsconfig.json` - TypeScript compiler configuration.

In modern standalone Angular, `main.ts` usually starts the app like this:

```ts
bootstrapApplication(AppComponent, appConfig);
```

`AppComponent` is rendered inside `index.html`, usually through:

```html
<app-root></app-root>
```

`app.config.ts` commonly contains:

- router providers
- HTTP providers
- animations
- hydration / SSR features
- global services and app-wide providers

Short answer: `main.ts` starts Angular, `AppComponent` is the root UI, `app.config.ts` or `app.module.ts` configures providers, `index.html` hosts the app, and `angular.json` / `tsconfig.json` configure the project and TypeScript.
