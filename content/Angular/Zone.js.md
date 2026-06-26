**Zone.js** is a library Angular used to know **when [[questions]] work finishes**, so Angular can run **change detection** and update the UI.

Examples of async work:

```js
setTimeout()  
Promise.then()  
click events  
HTTP requests  
setInterval()
```

Zone.js patches these APIs and tells Angular:
> “Something async happened. Maybe the UI changed. Run change detection.”

```
User clicks button / HTTP finishes / timer finishes
↓  
Zone.js detects async task  
↓  
Angular runs change detection  
↓  
Template updates
```

#### NgZone

Angular gives you `NgZone` to control this behavior.

`runOutsideAngular` - Run code outside Angular
Good for:

```
scroll  
mousemove  
resize  
animations  
third-party libraries  
charts  
maps
```

`run()` - brings execution back into Angular, so UI can update.
Example: 

```js
this.ngZone.runOutsideAngular(() => {
  window.addEventListener('scroll', () => {
    if (window.scrollY > 500) {
      this.ngZone.run(() => {
        this.showButton = true;
      });
    }
  });
});
```


#### Zone pollution

**Zone pollution** means too many unnecessary async events trigger change detection.

Example:

```js
window.addEventListener('mousemove', () => {  
	// fires many times
});
```

If this runs inside Angular zone, Angular may run change detection too often.

Fix:

```js
this.ngZone.runOutsideAngular(() => {  
	document.addEventListener('mousemove', handler);
});
```


## [[Zoneless]] Angular

Modern Angular is moving away from Zone.js.

In Angular v20, you can enable zoneless with:

```js
bootstrapApplication(AppComponent, {  providers: [provideZonelessChangeDetection()]});
```

Angular docs say `provideZonelessChangeDetection()` configures the app not to use Zone.js for scheduling change detection.

Angular docs also state that **Angular v21+ is zoneless by default**.