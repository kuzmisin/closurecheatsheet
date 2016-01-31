## Listen

### goog.events.listen
```javascript
/**
 * Adds an event listener for a specific event on a DOM Node or goog.events.EventTarget
 *
 * @param {goog.events.ListenableType} src The node to listen to events on.
 * @param {string|Array.<string>} type Event type or array of event types.
 * @param {Function|Object} listener Callback method
 * @param {boolean=} opt_capt Whether to fire in capture phase (defaults to false)
 * @param {Object=} opt_handler Element in whose scope to call the listener.
 * @return {goog.events.Key} Unique key for the listener.
 */
goog.events.listen = function(src, type, listener, opt_capt, opt_handler) {}
```

```javascript
var container = goog.dom.getElement('container');

goog.events.listen(
	container,
	goog.events.EventType.CLICK,
	function(e) {
		// ...
	}
);

goog.events.listen(
	container,
	[goog.events.EventType.MOUSEOVER, goog.events.EventType.MOUSEOUT],
	function(e) {
		// ...
	}
);
```

```javascript
/**
 * @constructor
 */
app.Foo = function() {
	var container = goog.dom.getElement('container');

	goog.events.listen(
		container,
		goog.events.EventType.CLICK,
		this.handleClick_,
		false, // capture phase (means from parent to child)
		this // run in THIS scope
	);
};

/**
 * @param {goog.events.Event} e
 * @private
 */
app.Foo.prototype.handleClick_ = function(e) {
	// ...
};
```

## Unlisten

### goog.events.unlisten
```javascript
/**
 * Removes an event listener which was added with listen().
 * @param {goog.events.ListenableType} src The target to stop listening to events on.
 * @param {string|Array.<string>} type The name of the event.
 * @param {Function|Object} listener The listener function to remove.
 * @param {boolean=} opt_capt
 * @param {Object=} opt_handler
 * @return {?boolean} indicating whether the listener was there to remove.
 */
goog.events.unlisten = function(src, type, listener, opt_capt, opt_handler) {}
```

```javascript
var container = goog.dom.getElement('container');
var clickHandler = function(e) {
	// ...
};

goog.events.listen(
	container,
	goog.events.EventType.CLICK,
	clickHandler
);
// ...
goog.events.unlisten(
	container,
	goog.events.EventType.CLICK,
	clickHandler
);
```

## Event Handler

### goog.events.EventHandler

By extending `EventHandler`, event handlers will have the same lifecycle as the object. (As opposed to `goog.events.listen` which ties event handler lifecycle to the element.) When instances of an `EventHandler` class are disposed, the event handlers are removed, even if the element is still present in the DOM.

This approach also avoids the need for using `goog.bind` on the event method, or having to pass `this` as parameter to `listen()`.

**Do not forget** to call `object.dispose()` for cleanup (destructing object). Dispose method will also remove all listeners.

```javascript
/**
 * Example {@code EventHandler} class.
 * @constructor
 * @extends {goog.events.EventHandler}
 * @final
 */
app.Foo = function() {
	app.Foo.base(this, 'constructor');

	var container = goog.dom.getRequiredElement('container');

	// listen for CLICK -> handled by this.handleClick_ 
	// note: this.listenOnce() is also useful.
	this.listen(
		container,
		goog.events.EventType.CLICK, 
		this.handleClick_
	);

	// listen for MOUSEOVER, MOUSEOUT -> handled by this.handleMouseEvent_
	this.listen(
		container,
		[goog.events.EventType.MOUSEOVER, goog.events.EventType.MOUSEOUT],
		this.handleMouseEvent_
	);
};
goog.inherits(app.Foo, goog.events.EventHandler);


/**
 * Handles mouse being hovered over container.
 * @param {!goog.events.BrowserEvent} e
 * @private
 */
app.Foo.prototype.handleMouseEvent_ = function(e) {
	var type = e.type;  // e.g. goog.events.EventType.MOUSEOVER
	// ...
};


/**
 * Handles container being clicked.
 * @param {!goog.events.BrowserEvent} e
 * @private
 */
app.Foo.prototype.handleClick_ = function(e) {
	// ...
};
```

## History
```javascript
var history = new goog.History();
history.setEnabled(true);

goog.events.listen(
	history,
	goog.history.EventType.NAVIGATE,
	function(/** goog.history.Event */ e) {
		// e.token
		// e.isNavigation
		// ...
	}
);
```

## Key Handler

### goog.events.KeyHandler

```javascript
var keyHandler = new goog.events.KeyHandler(goog.dom.getDocument().body);

goog.events.listen(
	keyHandler,
	goog.events.KeyHandler.EventType.KEY,
	function(e) {
		// e.repeat
		// e.keyCode goog.events.KeyCodes
		// e.charCode String.fromCharCode()
		// e.altKey
		// e.shiftKey
	}
);
```

More complex example
```javascript
/**
 * @constructor
 * @extends {goog.Disposable}
 */
app.Foo = function() {
	goog.base(this);

	this.keyHandler_ = new goog.events.KeyHandler(goog.dom.getDocument().body);

	this.getHandler().listen(
		this.keyHandler_,
		goog.events.KeyHandler.EventType.KEY,
		this.handleKey_
	);
};

goog.inherits(app.Foo, goog.Disposable);

/**
 * @type {goog.events.EventHandler}
 */
app.Foo.prototype.handler_;

/**
 * @type {goog.events.KeyHandler}
 */
app.Foo.prototype.keyHandler_;

/**
 * Create/return EventHandler which has default SCOPE to THIS
 *
 * @returns {goog.events.EventHandler}
 */
app.Foo.prototype.getHandler = function() {
	return this.handler_ || (this.handler_ = new goog.events.EventHandler(this));
};

/**
 * @override
 */
app.Foo.prototype.disposeInternal = function() {
	if (this.handler_) {
		this.handler_.dispose();
		this.handler_ = null;
	}
	if (this.keyHandler_) {
		this.keyHandler_.dispose();
		this.keyHandler_ = null;
	}

	goog.base(this, 'disposeInternal');
};

/**
 * @param {goog.events.KeyEvent} e
 * @private
 */
app.Foo.prototype.handleKey_ = function(e) {
	// e.repeat
	// e.keyCode goog.events.KeyCodes
	// e.charCode String.fromCharCode()
	// e.altKey
	// e.shiftKey
};
```

## Event
Use unique ID for custom event
```javascript
/**
 * @enum {string}
 */
app.Foo.EventType = {
	START: goog.events.getUniqueId('start'),
	STOP: goog.events.getUniqueId('stop')
};
```

### Custom event
```javascript
/**
 * @param {number=} opt_start
 * @constructor
 * @extends {goog.events.Event}
 */
app.Foo.StartEvent = function(opt_start) {
	goog.base(this, app.Foo.EventType.START);

	if (goog.isDef(opt_start)) {
		this.start = opt_start;
	}
};

goog.inherits(app.Foo.StartEvent, goog.events.Event);

/**
 * @type {number}
 */
app.Foo.StartEvent.prototype.start;
```

## Event Target
Create your own EventTarget object on which you can:

1. listen for events
2. dispatch events
3. be part of structure (like a DOM)

```javascript
/**
 * @constructor
 * @extends {goog.events.EventTarget}
 */
app.EventTarget = function() {
	goog.base(this);
};

goog.inherits(app.EventTarget, goog.events.EventTarget);
```

```javascript
var target = new app.EventTarget();

goog.events.listen(
	target,
	'custom-event',
	function(e) {
		// ...
	}
);

// ...

goog.events.dispatchEvent(
	target,
	'custom-event'
);
```

```javascript
var parent = new app.EventTarget();
var child = new app.EventTarget();

// set parent for child, so event will bubble up (capture) to parent
child.setParentEventTarget(parent);

// listen on parent
goog.events.listen(
	parent,
	'custom-event',
	function(e) {
		// ...
	}
);

// ...

// dispatch on child
goog.events.dispatchEvent(
	child,
	'custom-event'
);
```
