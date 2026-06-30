# HTTP

HTTP is the protocol used by browsers and clients to communicate with servers.

In Angular applications, HTTP is usually handled through `HttpClient`.

## Core HTTP Idea

HTTP works with request and response messages.

Client sends a request:

```text
GET /api/users
```

Server returns a response:

```text
200 OK
[
  { "id": 1, "name": "Ana" }
]
```

## Common HTTP Methods

| Method | Purpose |
|---|---|
| `GET` | read data |
| `POST` | create data |
| `PUT` | replace data |
| `PATCH` | partially update data |
| `DELETE` | remove data |

## Status Codes

Common status codes:

- `200` - success
- `201` - created
- `204` - success with no content
- `400` - bad request
- `401` - unauthenticated
- `403` - forbidden
- `404` - not found
- `409` - conflict
- `500` - server error

## Headers

Headers contain request or response metadata.

Examples:

- `Authorization`
- `Content-Type`
- `Accept`
- `Cache-Control`

```http
Authorization: Bearer token
Content-Type: application/json
```

## Query Params

Query params send extra values in the URL.

```text
/api/users?page=1&search=ana
```

Use them for:

- filtering
- sorting
- pagination
- search

## Body

Request body sends data to the server.

Common with:

- `POST`
- `PUT`
- `PATCH`

```json
{
  "name": "Ana",
  "email": "ana@test.com"
}
```

## Angular HttpClient

`HttpClient` provides a typed, Observable-based API for HTTP requests.

```ts
this.http.get<User[]>('/api/users');
```

It solves:

- backend communication
- typed responses
- request headers
- query params
- body serialization
- interceptors
- error handling
- RxJS integration

## HttpClient Examples

GET:

```ts
this.http.get<User[]>('/api/users');
```

POST:

```ts
this.http.post<User>('/api/users', user);
```

PUT:

```ts
this.http.put<User>(`/api/users/${user.id}`, user);
```

DELETE:

```ts
this.http.delete<void>(`/api/users/${id}`);
```

## HttpHeaders

`HttpHeaders` represents request metadata.

```ts
const headers = new HttpHeaders({
  Authorization: `Bearer ${token}`,
});

this.http.get<User[]>('/api/users', { headers });
```

Use headers for:

- auth tokens
- content type
- accepted response format
- custom request metadata

## HttpParams

`HttpParams` represents query string values.

```ts
const params = new HttpParams()
  .set('page', 1)
  .set('search', 'ana');

this.http.get<User[]>('/api/users', { params });
```

Use params for filtering, searching, sorting, and pagination.

## Typed Responses

Angular `HttpClient` supports generic response typing.

```ts
this.http.get<User[]>('/api/users');
```

This improves:

- IntelliSense
- compile-time safety
- refactoring
- maintainability

Important: TypeScript typing does not validate runtime API data. For untrusted data, use runtime validation.

## HTTP Interceptors

HTTP interceptors are middleware-like functions/classes that sit between `HttpClient` and the backend.

They can inspect or transform requests and responses globally.

Common uses:

- attach auth tokens
- centralized error handling
- logging
- retry logic
- loading indicators
- request/response transformation

## Interceptor Example

```ts
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = inject(AuthService).token;

  const authReq = req.clone({
    setHeaders: {
      Authorization: `Bearer ${token}`,
    },
  });

  return next(authReq);
};
```

Register:

```ts
provideHttpClient(
  withInterceptors([authInterceptor])
);
```

Interceptors keep cross-cutting HTTP logic out of feature services.

## HTTP Error Handling

HTTP errors should be handled at the correct level.

Local handling:

- expected validation errors
- feature-specific messages
- recoverable business errors

Interceptor handling:

- auth errors
- token refresh
- logging
- global error mapping
- retry for transient failures

Global error handling:

- unexpected application errors
- reporting to monitoring tools

## Error Example

```ts
this.http.get<User[]>('/api/users').pipe(
  catchError(error => {
    if (error.status === 404) {
      return of([]);
    }

    return throwError(() => error);
  })
);
```

## HttpClient and RxJS

`HttpClient` returns Observables.

That means HTTP requests can use RxJS operators:

- `map`
- `switchMap`
- `catchError`
- `retry`
- `shareReplay`

```ts
this.users$ = this.http.get<User[]>('/api/users').pipe(
  shareReplay({ bufferSize: 1, refCount: true })
);
```

## Short Answer

HTTP is the request/response protocol used for client-server communication. Angular `HttpClient` provides a typed, Observable-based API for HTTP requests. Core concepts include methods, status codes, headers, query params, request bodies, typed responses, interceptors, and error handling. Interceptors handle cross-cutting concerns like auth, logging, retries, and centralized HTTP errors.

## Related Notes

- [[What is Angular]]
- [[RxJS]]
- [[RxJS#RxJS Error Handling|RxJS Error Handling]]
- [[Production Readiness#Global Error Handling|Global Error Handling]]
- [[Behavioral Patterns#Chain of Responsibility|Chain of Responsibility]]
