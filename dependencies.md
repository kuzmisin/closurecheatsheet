## goog.provide
file: `app/some.js`
```javascript
goog.provide('app.Some');

/**
 * @constructor
 */
app.Some = function() {

};
```

## goog.require
file: `app/other.js`
```javascript
goog.provide('app.Other');

goog.require('app.Some');
goog.require('goog.dom');

/**
 * @constructor
 * @extends {app.Some}
 */
app.Other = function() {
	goog.base(this);
	this.element = goog.dom.getElement('other');
};
```

## goog.exportSymbol
```javascript
goog.provide('app.start');

app.start = function() {
	// ...
};

// make app.start 'accessible' after ADVANCED_OPTIMIZATIONS
goog.exportSymbol('app.start', app.start);
```

## @export annotation

When using the `@export` annotation, be certain that you use the `--generate_exports` compiler flag.

```javascript
goog.provide('app.start');

/**
 * Make {@code app.start} accessible after {@code ADVANCED_OPTIMIZATIONS}.
 * @export
 */
app.start = function() {
	// ...
};
```
