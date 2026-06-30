# Directives

Directives are Angular features that add behavior to elements or change how the DOM is rendered.

There are three main directive types:

- components
- structural directives
- attribute directives

Components are directives with a template.

## Structural Directives

Structural directives change the DOM structure.

They can:

- add elements
- remove elements
- repeat elements
- choose which template should render

Classic examples:

```html
<div *ngIf="isLoggedIn">Welcome</div>

<li *ngFor="let user of users">
  {{ user.name }}
</li>
```

`*ngIf` conditionally renders content.

`*ngFor` renders a list of items.

In modern Angular, `@if` and `@for` often replace `*ngIf` and `*ngFor`, but technically they are [[Control Flow Syntax]], not directive classes.

## Attribute Directives

Attribute directives change the appearance or behavior of an existing element.

They do not add or remove DOM elements.

Common examples:

```html
<div [ngClass]="{ active: isActive }"></div>
<div [ngStyle]="{ color: textColor }"></div>
```

Use attribute directives for:

- styling behavior
- permissions
- validation states
- highlighting
- reusable DOM behavior

Custom attribute directive example:

```ts
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private elementRef: ElementRef) {
    this.elementRef.nativeElement.style.backgroundColor = 'yellow';
  }
}
```

Usage:

```html
<p appHighlight>Important text</p>
```

## Structural vs Attribute

| Type | Changes DOM structure? | Example | Purpose |
|---|---:|---|---|
| Structural directive | yes | `*ngIf`, `*ngFor` | add/remove/render elements |
| Attribute directive | no | `ngClass`, `ngStyle` | change behavior or styling |

## Short Answer

Structural directives change DOM layout by adding, removing, or repeating elements. Attribute directives change behavior or styling of existing elements. In modern Angular, `@if` and `@for` are used for conditional and list rendering, but they belong to control flow syntax rather than classic directive classes.

## Related Notes

- [[Components]]
- [[Binding]]
- [[Control Flow Syntax]]
