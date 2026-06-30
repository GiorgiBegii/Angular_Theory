# Accessibility

Accessibility means building UI that can be used by people with different abilities, devices, and assistive technologies.

Simple idea:

```text
usable by keyboard
understandable by screen readers
visible and clear for users
```

## Semantic HTML

Use native HTML elements when possible.

Good:

```html
<button>Save</button>
```

Avoid:

```html
<div (click)="save()">Save</div>
```

Native elements already provide keyboard behavior and accessibility semantics.

## ARIA

ARIA adds accessibility information when native HTML is not enough.

Use ARIA to describe:

- roles
- labels
- expanded/collapsed state
- selected state
- live regions

Rule:

```text
prefer semantic HTML first
use ARIA only when needed
```

Bad ARIA can make accessibility worse.

## Focus Management

Focus management controls where keyboard focus goes.

Important cases:

- modals
- dropdowns
- route/page changes
- validation errors
- dynamic content

Good focus behavior:

- focus is visible
- tab order is logical
- modal traps focus while open
- focus returns after modal closes
- keyboard users can reach all actions

## Forms

Accessible forms need:

- labels
- clear validation messages
- error association
- keyboard support
- not relying only on color

## UI Kit Accessibility

UI kits can help, but they do not guarantee accessibility.

Check:

- keyboard navigation
- focus states
- ARIA attributes
- color contrast
- screen reader labels
- modal/dropdown behavior

## Short Answer

Accessibility in Angular means using semantic HTML, correct ARIA only when needed, visible and logical focus management, keyboard support, labeled forms, readable contrast, and testing important flows with keyboard and screen reader assumptions. ARIA helps describe custom UI, but native semantic elements should be preferred.

## Related Notes

- [[Reactive Forms]]
- [[UI Kits]]
- [[Production Readiness]]
- [[Components]]
