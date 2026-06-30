# Production Readiness

Production readiness means the app can be built, deployed, monitored, debugged, and operated safely.

Simple idea:

```text
can we ship it?
can we observe it?
can we recover when it fails?
```

## CI/CD

CI/CD automates build, test, and deployment.

Typical pipeline:

- install dependencies
- lint/type-check
- run tests
- build production bundle
- run security/performance checks
- deploy to environment

CI/CD reduces manual release mistakes.

## Environment Handling

Environment handling separates configuration per environment.

Examples:

- development API URL
- staging API URL
- production API URL
- feature flags
- logging level

Do not store real secrets in frontend environment files. Frontend code is visible to users.

## Feature Flags

Feature flags allow enabling/disabling functionality without redeploying code.

Good for:

- gradual rollout
- A/B testing
- hiding incomplete features
- emergency disable switch

## Logging and Monitoring

Production apps need visibility.

Track:

- runtime errors
- failed HTTP requests
- slow pages
- user-impacting crashes
- important business events

Monitoring answers:

```text
what broke?
who is affected?
when did it start?
which release caused it?
```

## Global Error Handling

Angular can centralize unhandled errors with `ErrorHandler`.

Use global error handling for:

- logging unexpected errors
- reporting to monitoring tools
- adding release/user context

Still handle expected errors locally where the UI can recover.

## Source Maps

Source maps map minified production code back to original TypeScript.

Useful for debugging production errors.

Security note:

- private source maps should not be publicly exposed unless intentionally allowed
- many teams upload source maps to monitoring tools instead

## Production Checklist

Before release, check:

- production build works
- tests pass
- environment config is correct
- source maps policy is clear
- error reporting is enabled
- monitoring dashboards exist
- performance budget is checked
- accessibility basics are checked
- security headers/config are reviewed
- feature flags are in expected state

## Short Answer

A production-ready Angular app has reliable CI/CD, correct environment handling, feature flags, centralized logging and monitoring, global error reporting, controlled source maps, performance checks, accessibility checks, and a release process that makes failures visible and recoverable.

## Related Notes

- [[Angular Performance]]
- [[HTTP#HTTP Error Handling|HTTP Error Handling]]
- [[Security]]
- [[Accessibility]]
- [[Angular Architecture]]
