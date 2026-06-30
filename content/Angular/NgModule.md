# NgModule

`NgModule` is an Angular class decorated with `@NgModule()` that groups related Angular code together.

Before standalone components, `NgModule` was the main way to organize Angular applications.

## Basic example

```ts
@NgModule({
  declarations: [UserCardComponent],
  imports: [CommonModule],
  exports: [UserCardComponent],
  providers: [UserService],
})
export class UserModule {}
```

## Main parts

### declarations

`declarations` registers components, directives, and pipes that belong to this module.

```ts
declarations: [
  UserCardComponent,
  HighlightDirective,
  FullNamePipe,
]
```

Only non-standalone components/directives/pipes are declared here.

Standalone components are not declared in an `NgModule`.

## imports

`imports` brings in other modules whose exported features are needed.

```ts
imports: [
  CommonModule,
  ReactiveFormsModule,
  RouterModule,
]
```

Example:

- need `ngIf` / `ngFor` in old syntax -> import `CommonModule`
- need reactive forms -> import `ReactiveFormsModule`
- need router directives -> import `RouterModule`

## exports

`exports` makes declarations available to other modules.

```ts
exports: [
  UserCardComponent,
]
```

If a component is declared but not exported, other modules cannot use it.

## providers

`providers` registers services for dependency injection.

```ts
providers: [
  UserService,
]
```

Important:

- root module providers are usually app-wide
- lazy module providers can create feature-scoped instances
- component providers are often better for local component state

## bootstrap

`bootstrap` tells Angular which root component starts the application.

Old module-based app:

```ts
@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Modern standalone apps usually use `bootstrapApplication()` instead.

## AppModule

`AppModule` was traditionally the root Angular module.

It usually contained:

- `BrowserModule`
- root component declaration
- app-level imports
- app-level providers
- bootstrap component

Modern standalone apps may not have `AppModule`.

## Feature Module

A feature module groups related feature code.

Example:

```text
UsersModule
- users routes
- user list
- user detail
- user services
```

Feature modules were commonly used for lazy loading before standalone route configs became popular.

## Shared Module

A shared module usually exports reusable UI components, directives, and pipes.

Example:

```ts
@NgModule({
  declarations: [ButtonComponent, DateFormatPipe],
  exports: [ButtonComponent, DateFormatPipe],
})
export class SharedModule {}
```

In standalone Angular, shared modules are often replaced by directly importing standalone components/pipes/directives.

## Core Module

A core module traditionally contained singleton services and app-wide infrastructure.

Examples:

- auth services
- logging services
- HTTP interceptors
- app configuration

In modern Angular, this is often replaced by app-level providers in `app.config.ts`.

## NgModule vs Standalone Components

Old module-based style:

```text
Component -> declared in NgModule
NgModule -> imports dependencies
NgModule -> exports component
```

Modern standalone style:

```text
Component -> standalone: true
Component -> imports dependencies directly
```

Standalone components reduce the need for `declarations`, `exports`, and many feature/shared modules.

## When NgModule is still used

You may still see `NgModule` in:

- older Angular projects
- third-party libraries
- module-based lazy loading
- apps not yet migrated to standalone
- some library packaging patterns
- teams that still prefer module-based architecture

## Common mistakes

- declaring standalone components in an `NgModule`
- forgetting to export a declared component
- importing `BrowserModule` in feature modules instead of `CommonModule`
- putting feature-local state in root providers
- creating huge shared modules that import/export everything
- using modules only as unnecessary wrappers around one component

## Mental model

Ask:

```text
Is this old module-based Angular or modern standalone Angular?
Does this module group real feature behavior?
Is this provider app-wide, route-scoped, or feature-scoped?
Can this be simplified with standalone imports?
```

## Short answer

`NgModule` is Angular's older grouping mechanism for components, directives, pipes, providers, and imports. It was used to organize apps and define compilation/dependency boundaries. In modern Angular, standalone components reduce the need for most `NgModule` declarations, but `NgModule` is still important for understanding older projects and some libraries.

## Related

- [[Standalone Components]]
- [[Dependency Injection]]
- [[Angular Router#Lazy Loading|Lazy Loading]]
- [[Angular Router#loadComponent vs loadChildren|loadChildren]]
- [[What is Angular#App Startup|app.config.ts]]
- [[What is Angular]]
