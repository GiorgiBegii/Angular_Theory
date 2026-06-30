# Reactive Forms

Reactive Forms are Angular's model-driven approach for building forms.

The form model is created and managed in TypeScript, not mainly in the template.

Reactive Forms are useful for:

- complex forms
- dynamic forms
- custom validation
- async validation
- strong typing
- unit testing
- enterprise forms
- forms with RxJS/state integration

## Core Classes

Reactive Forms are built with:

- `FormControl`
- `FormGroup`
- `FormArray`
- `FormBuilder`
- `Validators`
- `AbstractControl`

## FormControl

`FormControl` represents one form field.

```ts
name = new FormControl('', { nonNullable: true });
```

It tracks:

- value
- validation status
- touched / untouched
- dirty / pristine
- disabled / enabled

## FormGroup

`FormGroup` represents a group of controls.

```ts
form = new FormGroup({
  name: new FormControl('', { nonNullable: true }),
  email: new FormControl('', { nonNullable: true }),
});
```

Template:

```html
<form [formGroup]="form">
  <input formControlName="name" />
  <input formControlName="email" />
</form>
```

## FormArray

`FormArray` represents a dynamic list of controls.

```ts
phones = new FormArray([
  new FormControl('', { nonNullable: true }),
]);
```

Use it when users can add or remove form rows.

## Validation

Sync validators run immediately.

They return:

- `null` when value is valid
- `ValidationErrors` when value is invalid

```ts
email = new FormControl('', {
  nonNullable: true,
  validators: [Validators.required, Validators.email],
});
```

Common built-in validators:

- `Validators.required`
- `Validators.minLength`
- `Validators.maxLength`
- `Validators.email`
- `Validators.pattern`
- `Validators.min`
- `Validators.max`

Custom sync validator:

```ts
export function forbiddenNameValidator(control: AbstractControl): ValidationErrors | null {
  return control.value === 'admin'
    ? { forbiddenName: true }
    : null;
}
```

Usage:

```ts
name = new FormControl('', {
  nonNullable: true,
  validators: [forbiddenNameValidator],
});
```

## Async Validators

Async validators return a Promise or Observable.

They are useful for server-side checks like:

- email already exists
- username already exists
- invite code is valid

```ts
export function uniqueEmailValidator(api: UserApi): AsyncValidatorFn {
  return control => {
    return api.emailExists(control.value).pipe(
      map(exists => exists ? { emailTaken: true } : null)
    );
  };
}
```

Usage:

```ts
email = new FormControl('', {
  nonNullable: true,
  validators: [Validators.required, Validators.email],
  asyncValidators: [uniqueEmailValidator(this.api)],
});
```

Angular runs sync validators first.

Async validators run only if sync validation passes.

## Validation Strategy

The validation strategy is defined when creating the control or group.

With `FormControl`, validators are usually configured in the options object:

```ts
email = new FormControl('', {
  nonNullable: true,
  validators: [Validators.required],
  asyncValidators: [uniqueEmailValidator(this.api)],
  updateOn: 'blur',
});
```

`updateOn` controls when validation runs:

- `change` - default, validate on every value change
- `blur` - validate when input loses focus
- `submit` - validate when form is submitted

Older constructor style:

```ts
new FormControl('', syncValidators, asyncValidators);
```

## Value Changes

Reactive Forms expose streams.

```ts
this.form.valueChanges.subscribe(value => {
  console.log(value);
});
```

Common RxJS usage:

```ts
this.search.valueChanges.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(query => this.api.search(query))
);
```

## setValue vs patchValue

`setValue` requires the full form shape.

```ts
this.form.setValue({
  name: 'Ana',
  email: 'ana@test.com',
});
```

`patchValue` updates only provided fields.

```ts
this.form.patchValue({
  name: 'Ana',
});
```

## Typed Forms

Typed Forms add strong TypeScript typing to form controls.

With typed forms, form values, controls, and `patchValue` / `setValue` operations are type-safe at compile time.

Untyped:

```ts
const form = new FormGroup({});
```

Values are basically `any`.

Typed:

```ts
const form = new FormGroup<{
  name: FormControl<string>;
}>({
  name: new FormControl('', { nonNullable: true }),
});
```

Benefits:

- better IntelliSense
- fewer runtime mistakes
- safer refactoring
- compile-time validation
- clearer form model

Typed forms reduce runtime bugs and improve developer experience in large Angular codebases.

## Reactive vs Template-Driven Forms

Reactive Forms:

- form model is defined in TypeScript
- explicit and scalable
- easier to test
- better for dynamic validation
- better for large production apps
- integrates naturally with RxJS

Template-driven forms:

- form logic mainly lives in template
- uses directives like `ngModel`
- less boilerplate
- good for small/simple forms

## Angular Architecture Notes

For scalable forms:

- keep form creation readable
- move complex validators into separate functions
- use typed forms
- avoid huge components
- use `valueChanges` carefully
- unsubscribe or use `takeUntilDestroyed` for manual subscriptions
- prefer reactive forms for complex workflows

## Short Answer

Reactive Forms are model-driven forms where structure, validation, and state are managed in TypeScript using `FormControl`, `FormGroup`, and `FormArray`. Typed Forms add TypeScript types to form controls and values, making `setValue`, `patchValue`, and form access safer at compile time. Reactive Forms are preferred for complex, dynamic, testable, and enterprise-level forms.

## Related Notes

- [[What is Angular]]
- [[RxJS]]
- [[RxJS#Debounce|Debounce]]
- [[RxJS#switchMap|switchMap]]
- [[ControlValueAccessor]]
- [[Template-driven Forms]]
