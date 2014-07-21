## Inheritance
```javascript
/**
 * @param {string} foo
 * @constructor
 */
app.Parent = function(foo) {
	this.foo = foo;
};

/**
 * @type {string}
 */
app.Parent.prototype.foo;

/**
 * @param {string} param
 */
app.Parent.prototype.doSomething = function(param) {
	// ...
};
```

```javascript
/**
 * @param {string} foo
 * @param {string} bar
 * @constructor
 * @extends {app.Parent}
 */
app.Child = function(foo, bar) {
	app.Child.base(this, 'constructor', foo); // call parent constructor
	this.bar = bar;
};

goog.inherits(app.Child, app.Parent);

/**
 * @type {string}
 */
app.Child.prototype.bar;

/**
 * @override
 */
app.Child.prototype.doSomething = function(param) {
	app.Child.base(this, 'doSomething', param); // call super doSomething

	// ...
};

```

## Singleton
```javascript
/**
 * @constructor
 */
app.Foo = function() {

};

goog.addSingletonGetter(app.Foo);
```

```javascript
var fooInstance = /** @type {app.Foo} */ (app.Foo.getInstance());
```

## Mixin
```javascript
/**
 * @constructor
 */
app.Mixin = function() {};

/**
 *
 */
app.Mixin.prototype.mixinFunction = function() {

};
```

```javascript
/**
 * @constructor
 */
app.Foo = function() {

};

/**
 *
 */
app.Foo.prototype.fooFunction = function() {

};

goog.mixin(app.Foo.prototype, app.Mixin.prototype);
```

```javascript
var foo = new app.Foo();
foo.fooFunction();
foo.mixinFunction();
```

## Abstract, Interface
```javascript
/**
 * Abstract object
 *
 * @constructor
 */
app.AParent = function() {

};

/**
 * This function must be implemented in child
 */
app.AParent.prototype.someFunction = goog.abstractMethod;
```

```javascript
/**
 * Interface
 *
 * @interface
 */
app.Interface = function() {};

/**
 * Interface function
 */
app.Interface.prototype.interfaceFunction;
```

```javascript
/**
 * Child which extends abstract object and implements interface
 *
 * @constructor
 * @extends {app.AParent}
 * @implements {app.Interface}
 */
app.Child = function() {
	app.Child.base(this, 'constructor');
};

goog.inherits(app.Child, app.AParent)

/**
 * @override
 */
app.Child.prototype.someFunction = function() {
	// ...
};

/**
 * @override
 */
app.Child.prototype.interfaceFunction = function() {
	// ...
};
```

## Disposable
```javascript
/**
 * @constructor
 * @extends {goog.Disposable}
 */
app.Foo = function() {
	app.Foo.base(this, 'constructor');

	// non-disposable object (NOT @extends goog.Disposable)
	this.element = goog.dom.getElement('bar');
	this.button = goog.dom.createDom(goog.dom.TagName.BUTTON);

	// disposable objects (@extends goog.Disposable)
	this.handler = new goog.events.EventHandler(this);

	// register for dispose in disposeInternal()
	// Alternatively: Call goog.dispose(this.handler) in disposeInternal().
	this.registerDisposable(this.handler);
};

goog.inherits(app.Foo, goog.Disposable);

/**
 * Reference to DOM object
 *
 * @type {Element}
 */
app.Foo.prototype.element;

/**
 * Reference to DOM object
 *
 * @type {!Element}
 */
app.Foo.prototype.button;

/**
 * Another disposable object
 *
 * @type {goog.events.EventHandler}
 */
app.Foo.prototype.handler;

/**
 * Clean/dispose all object, references, ...
 *
 * @override
 */
app.Foo.prototype.disposeInternal = function() {
	// cleanup
	this.element = null;  // Preferred for nullable fields (faster).
	delete this.button;  // Necessary for non-null fields.
	this.handler = null;

	// dispose all disposable object registered as this.registerDisposable()
	app.Foo.base(this, 'disposeInternal');
};
```

```javascript
var a = new app.Foo();
// ...
a.dispose();  // Alternatively: goog.dispose(a); which permits null/undefined.
```

Monitoring disposable object
```javascript
// set monitoring mode
goog.Disposable.MONITORING_MODE = goog.Disposable.MonitoringMode.PERMANENT;
//goog.Disposable.MONITORING_MODE = goog.Disposable.MonitoringMode.INTERACTIVE;

// get array of 'undisposed' objects
console.log(goog.Disposable.getUndisposedObjects());
```
