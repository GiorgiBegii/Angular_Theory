# SSR

SSR means Server-Side Rendering.

Angular renders the initial HTML on the server, sends it to the browser, and then the browser makes it interactive.

Simple idea:

```text
server sends ready HTML
browser hydrates it
app becomes interactive
```

## Why Use SSR

SSR helps with:

- faster first content display
- SEO for public pages
- social preview metadata
- better perceived performance
- content visible before JavaScript finishes loading

SSR is especially useful for public, content-heavy, marketing, product, or SEO-sensitive pages.

## Hydration

Hydration means Angular reuses the server-rendered DOM in the browser instead of destroying and recreating it.

Without hydration:

```text
server HTML appears
browser throws it away
Angular renders again
```

With hydration:

```text
server HTML appears
Angular attaches behavior to existing DOM
```

Hydration improves startup performance and avoids visual flicker.

## TransferState

TransferState passes data fetched on the server to the browser.

Problem without TransferState:

```text
server fetches data
browser fetches same data again
```

With TransferState:

```text
server fetches data
data is serialized into HTML
browser reuses it
```

This avoids duplicate HTTP requests during hydration.

## SSR Constraints

Server code does not have browser globals.

Be careful with:

- `window`
- `document`
- `localStorage`
- `sessionStorage`
- direct DOM access
- browser-only libraries

Use platform checks or run browser-only code after the app is in the browser.

## Good SSR Rules

- avoid direct browser API usage during server render
- keep data fetching predictable
- use TransferState for reused server data
- keep page metadata SSR-friendly
- test both server render and browser hydration
- make sure authenticated/private pages do not leak user-specific HTML through caching

## Short Answer

Angular SSR renders the initial page HTML on the server. Hydration lets Angular attach behavior to that existing DOM in the browser instead of rerendering it. TransferState moves server-fetched data to the browser so the same request does not need to run twice. SSR improves SEO and perceived performance, but code must avoid unsafe browser-only APIs during server rendering.

## Related Notes

- [[Angular Performance]]
- [[Angular Router]]
- [[HTTP]]
- [[Production Readiness]]
