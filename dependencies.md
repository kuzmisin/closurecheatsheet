## goog.provide
```app/some.js```
```javascript
goog.provide('app.Some');

/**
 * @constructor
 */
app.Some = function() {

};
```

## goog.require
```app/other.js```
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