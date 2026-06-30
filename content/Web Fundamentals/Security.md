# Security

Frontend security means reducing the chance that user data, tokens, UI behavior, or browser execution can be abused.

Simple idea:

```text
never trust input
avoid unsafe execution
protect tokens and user actions
backend must enforce real authorization
```

## XSS

XSS means Cross-Site Scripting.

It happens when untrusted content is executed as JavaScript in the user's browser.

Risk:

- steal tokens
- act as the user
- modify the page
- read sensitive data visible to the page

Avoid:

- inserting untrusted HTML
- bypassing sanitization without strong reason
- trusting third-party scripts
- storing sensitive tokens where injected scripts can read them

## Sanitization

Sanitization removes or neutralizes unsafe content.

Angular sanitizes dangerous values in many template bindings.

Still be careful with:

- `[innerHTML]`
- bypass security APIs
- external HTML
- dynamic URLs
- third-party widgets

## CSP

CSP means Content Security Policy.

It is a browser security policy that limits which scripts, styles, images, and connections are allowed.

CSP helps reduce XSS impact by blocking unexpected script execution.

## CSRF

CSRF means Cross-Site Request Forgery.

It tricks a browser into sending an authenticated request to another site.

Protection:

- SameSite cookies
- CSRF tokens
- checking origin/referer
- avoiding unsafe state changes through GET requests

## Secrets

Never put real secrets in frontend code.

Frontend code can be inspected by users.

Do not store:

- private API keys
- backend credentials
- signing secrets
- database credentials

## Client vs Server Responsibility

Frontend checks improve UX, but server must enforce real security.

Frontend can:

- hide UI
- validate inputs early
- guide user behavior

Backend must:

- authenticate
- authorize
- validate
- protect data
- enforce permissions

## Short Answer

Client-side security includes avoiding unsafe HTML injection, using CSP, validating external content, protecting tokens carefully, and never storing real secrets in frontend code. XSS is dangerous because injected JavaScript runs as the user. Angular sanitization helps, but backend authorization and validation are still required.

## Related Notes

- [[HTTP]]
- [[Production Readiness]]
- [[Angular Architecture]]
