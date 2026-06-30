# ControlValueAccessor

ControlValueAccessor connects a custom Angular component to Angular Forms API.

It makes a custom component behave like a native form control.

Works with:

- `ngModel`
- `FormControl`
- `formControlName`
- reactive forms
- template-driven forms

## Problem It Solves

Angular Forms know how to work with native inputs:

```html
<input formControlName="name" />
```

But Angular does not automatically know how to work with custom components:

```html
<app-rating formControlName="rating" />
```

`ControlValueAccessor` provides the bridge between:

- form model
- custom component UI

Without it, a custom component cannot fully participate in Angular forms.

## What It Allows Angular To Do

ControlValueAccessor allows Angular to:

- write values to the component
- listen for value changes
- track touched / dirty state
- handle disabled state

## Required Methods

```ts
writeValue(value: unknown): void;
registerOnChange(fn: (value: unknown) => void): void;
registerOnTouched(fn: () => void): void;
setDisabledState?(isDisabled: boolean): void;
```

## Method Meaning

`writeValue`

Angular calls this when the form model writes a value into the component.

`registerOnChange`

Angular gives the component a callback. The component calls it when its internal value changes.

`registerOnTouched`

Angular gives the component a callback. The component calls it when the user touches or blurs the control.

`setDisabledState`

Angular calls this when the form disables or enables the control.

## Minimal Example

```ts
@Component({
  selector: 'app-rating',
  template: `
    <button
      type="button"
      [disabled]="disabled"
      (click)="setValue(1)"
    >
      1
    </button>
  `,
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => RatingComponent),
      multi: true,
    },
  ],
})
export class RatingComponent implements ControlValueAccessor {
  value = 0;
  disabled = false;

  private onChange = (value: number) => {};
  private onTouched = () => {};

  writeValue(value: number): void {
    this.value = value;
  }

  registerOnChange(fn: (value: number) => void): void {
    this.onChange = fn;
  }

  registerOnTouched(fn: () => void): void {
    this.onTouched = fn;
  }

  setDisabledState(isDisabled: boolean): void {
    this.disabled = isDisabled;
  }

  setValue(value: number): void {
    this.value = value;
    this.onChange(value);
    this.onTouched();
  }
}
```

Usage:

```html
<app-rating formControlName="rating" />
```

## When To Use

Use ControlValueAccessor when building reusable form components like:

- custom select
- date picker
- rating component
- phone input
- rich text editor
- file uploader
- toggle group

## Common Mistakes

Do not forget to call `onChange` when value changes.

Do not forget to call `onTouched` when the user interacts with the control.

Do not mutate form state directly from outside the CVA contract.

Do not forget `multi: true` for `NG_VALUE_ACCESSOR`.

## Short Answer

ControlValueAccessor exists to connect custom form components with Angular Forms API. It lets Angular write values into the component, listen for value changes, track touched/dirty state, and handle disabled state. Without it, custom UI components cannot behave like native form controls in reactive or template-driven forms.

## Related Notes

- [[Reactive Forms]]
- [[Template-driven Forms]]
- [[Binding]]
- [[Structural Patterns#Adapter|Adapter]]
