# Template-driven Forms

Template-driven Forms are Angular forms where most form logic is defined in the template.

They use directives like:

- `ngModel`
- `ngForm`
- `ngModelGroup`

Template-driven forms are simpler and require less TypeScript setup than reactive forms.

## Basic Example

```html
<form #form="ngForm" (ngSubmit)="submit(form.value)">
  <input
    name="email"
    ngModel
    required
    email
  />

  <button type="submit" [disabled]="form.invalid">
    Save
  </button>
</form>
```

Angular creates the form model automatically from the template.

## ngModel

`ngModel` binds a form control to a value.

```html
<input name="username" [(ngModel)]="username" />
```

This uses two-way binding:

- component value goes into the input
- input changes update the component value

## Validation

Validation is usually declared in the template.

```html
<input
  name="email"
  ngModel
  required
  email
  #email="ngModel"
/>

@if (email.invalid && email.touched) {
  <p>Email is invalid</p>
}
```

Common validators:

- `required`
- `email`
- `minlength`
- `maxlength`
- `pattern`

## When To Use

Use template-driven forms for:

- small forms
- simple forms
- quick prototypes
- forms with limited validation
- basic two-way binding

## When Not To Use

Avoid template-driven forms for:

- large enterprise forms
- dynamic form structures
- complex validation
- heavy TypeScript logic
- strongly typed form models
- advanced RxJS workflows

Use [[Reactive Forms]] for those cases.

## Reactive Forms vs Template-driven Forms

| Feature | Reactive Forms | Template-driven Forms |
|---|---|---|
| Form model | TypeScript | template |
| Scalability | high | lower |
| Testing | easier | harder |
| Dynamic forms | better | limited |
| Typing | supports typed forms | weaker |
| Boilerplate | more | less |
| Best for | complex apps | simple forms |

## ControlValueAccessor

Template-driven forms also use [[ControlValueAccessor]] for custom form components.

```html
<app-rating name="rating" ngModel />
```

The custom component must implement ControlValueAccessor to work with `ngModel`.

## Short Answer

Template-driven Forms are Angular forms where form structure, binding, and validation mainly live in the template using directives like `ngModel`. They are simple and low-boilerplate, so they fit small forms and prototypes. For complex, dynamic, strongly typed, or enterprise forms, Reactive Forms are usually preferred.

## Related Notes

- [[Reactive Forms]]
- [[Reactive Forms#Typed Forms|Typed Forms]]
- [[ControlValueAccessor]]
- [[Binding#Two-way Binding|Two-way Binding]]
