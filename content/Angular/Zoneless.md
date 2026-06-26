Zoneless Angular means Angular no longer uses Zone.js to automatically detect async operations.

Instead of:

```
Something async happened
↓
Zone.js notices it↓Angular runs change detection
```

Angular becomes:

```
Signal changed
Input changed
Template event happened
Async pipe emittedmark
ForCheck() called
↓
Angular updates UI
```

This gives **more explicit, predictable, and performant change detection**. 
<span style="color:#ff5555;">Angular's official zoneless guide recommends removing Zone.js entirely in zoneless apps. <br/> Zone.js is expencive.</span>

## Why move away from Zone.js?

Zone.js:

- Patches browser APIs.
- Triggers change detection after every async operation.
- Can cause unnecessary checks.
- Adds bundle size.
- Makes debugging stack traces harder.

Zoneless:

✅ Smaller bundle  
✅ Better performance  
✅ More predictable updates  
✅ Cleaner stack traces  
✅ Better interoperability with external libraries

# How does UI update without Zone.js?
Angular listens to explicit triggers.

## 1. Signals

```js
readonly count = signal(0);
increment() {  
	this.count.update(v => v + 1);
}
```

Template:

```js
{{ count() }}
```

>Signal change automatically updates the view.

---

## 2. Input changes

```js
@Component({})export class ChildComponent {  
	@Input() user!: User;
}
```

When parent provides a new value:

```HTML
<app-child [user]="user"></app-child>
```

Angular updates Child.

---

## 3. Template events

```HTML
<button (click)="increment()">
```

Template events trigger change detection.

---

## 4. Async Pipe

```js
users$ = this.http.get<User[]>('/users');
```

```js
@for(user of users$ | async; track user.id) {}
```

When Observable emits, Angular updates the UI.

---

## 5. markForCheck()

When Angular doesn't know data changed:

```js
constructor(
	private cdr: ChangeDetectorRef
) {}

setTimeout(() => {  
	this.count++;  
	this.cdr.markForCheck();
}, 1000);
```

`markForCheck()` tells Angular:

> Something changed, refresh this component tree.

# [[Change Detection]] Strategies in Angular

There are two main change detection strategies:
1. **Default**
2. **OnPush**

# 1. Default Strategy

```js
@Component({  
	changeDetection: ChangeDetectionStrategy.Default
})
```
This is the default behavior.

Whenever Angular runs change detection, it checks:

- The component itself
- All its children

### Triggered by

- User events (`click`, `input`)
- HTTP responses
- Timers (`setTimeout`)
- Promise resolutions
- Observable emissions
- Any async task (with Zone.js)

# 2. OnPush Strategy

```js
@Component({  
	changeDetection: ChangeDetectionStrategy.OnPush
})
```
Angular skips checking the component unless [[#How does UI update without Zone.js?|something important happens.]]

---

## Immutable vs Mutable Updates
