## Life cycle
```
         [construct]
         /         \
	[render]     [decorate]
         \         /
		[enterDocument]
              |
            [...]
              |
        [exitDocument]
              |
          [dispose]
```
## Component
```javascript
/**
 * @param {goog.dom.DomHelper=} opt_domHelper
 * @constructor
 * @extends {goog.ui.Component}
 */
app.Component = function(opt_domHelper) {
	goog.base(this, opt_domHelper);
};

goog.inherits(app.Component, goog.ui.Component);

/**
 * Used in render scenario
 * @override
 */
app.Component.prototype.createDom = function() {
	var element = this.getDomHelper().createDom('div');
	// ...
	this.setElementInternal(element);
};

/**
 * Used in decorate scenario
 * @override
 */
app.Component.prototype.decorateInternal = function(element) {
	goog.base(this, 'decorateInternal', element);
	// ...
};

/**
 * Can be component used in decorate scenario
 * @override
 */
app.Component.prototype.canDecorate = function() {
	return true;
}

/**
 * @override
 */
app.Component.prototype.enterDocument = function() {
	goog.base(this, 'enterDocument');

	// this.getHandler().listen()
	// ...
};

/**
 * @override
 */
app.Component.prototype.exitDocument = function() {
	goog.base(this, 'exitDocument');

	// unlisten is not necessary, parent remove all listeners from handler
	// ...
};

/**
 * @override
 */
app.Component.prototype.disposeInternal = function() {
	// ...
	goog.base(this, 'disposeInternal');
};
```

```javascript
var component = new app.Component();
component.render();

// var container = goog.dom.getDocument().body;
// component.render(body);
```

```javascript
var item = goog.dom.getElement('item');
var component = new app.Component();
component.decorate(item);
```
## Control
```javascript
/**
 * @param {goog.ui.ControlContent} content
 * @param {goog.ui.ControlRenderer=} opt_renderer
 * @param {goog.dom.DomHelper=} opt_domHelper
 * @constructor
 * @extends {goog.ui.Control}
 */
app.Control = function(content, opt_renderer, opt_domHelper) {
	goog.base(this, content, opt_renderer, opt_domHelper);

	// this.setId();
	// this.setAllowTextSelection(true);
	// this.setSupportedState(goog.ui.Component.State.ALL, false);
};

goog.inherits(app.Control, goog.ui.Control);

/**
 * @override
 */
app.Control.prototype.enterDocument = function() {
	goog.base(this, 'enterDocument');
	// ...
};

/**
 * @override
 */
app.Control.prototype.exitDocument = function() {
	goog.base(this, 'exitDocument');
	// ...
};

/**
 * @override
 */
app.Control.prototype.disposeInternal = function() {
	// ...
	goog.base(this, 'disposeInternal');
};

/**
 * Default renderer
 */
goog.ui.registry.setDefaultRenderer(
	app.Control, app.ControlRenderer
);
```

## ControlRenderer
```javascript
/**
 * @constructor
 * @extends {goog.ui.ControlRenderer}
 */
app.ControlRenderer = function() {
	goog.base(this);
};

goog.inherits(app.ControlRenderer, goog.ui.ControlRenderer);
goog.addSingletonGetter(app.ControlRenderer);

/**
 * @type {string}
 */
app.ControlRenderer.CSS_CLASS = 'control';

/**
 * @return {string}
 */
app.ControlRenderer.prototype.getCssClass = function() {
	return app.ControlRenderer.CSS_CLASS;
};

/**
 * @override
 */
app.ControlRenderer.prototype.createDom = function(control) {
	var element = control.getDomHelper().createDom('div');
	// ...
	return element;
};

/**
 * @override
 */
app.ControlRenderer.prototype.canDecorate = function(element) {
	return true;
};

/**
 * @override
 */
app.ControlRenderer.prototype.decorate = function(control, element) {
	var element = goog.base(this, 'decorate', control, element);
	// ...
	return element;
};

/**
 * Register control to CSS class name
 */
goog.ui.registry.setDecoratorByClassName(
	app.ControlRenderer.CSS_CLASS,
	function() {
		return new app.Control();
	}
);
```

## Container
```javascript
/**
 * @param {?goog.ui.Container.Orientation=} opt_orientation
 * @param {goog.ui.ContainerRenderer=} opt_renderer
 * @param {goog.dom.DomHelper=} opt_domHelper
 * @constructor
 * @extends {goog.ui.Container}
 */
app.Container = function(opt_orientation, opt_renderer, opt_domHelper) {
	// renderer app.ContainerRenderer.getInstance()

	goog.base(this, opt_orientation, opt_renderer, opt_domHelper);

	// this.setFocusable(false);
};

goog.inherits(app.Container, goog.ui.Container);

/**
 * @override
 */
app.Container.prototype.enterDocument = function() {
	goog.base(this, 'enterDocument');
	// ...
};

// get control based on event target:
// 		var control = this.getOwnerControl(/** @type {Node} */ (e.target));

// get control
//		var control = this.getChild(id);

// remove all children
//		this.removeChildren(true);
```

## ContainerRenderer
```javascript
/**
 * @constructor
 * @extends {goog.ui.ContainerRenderer}
 */
app.ContainerRenderer = function() {
	goog.base(this);
};

goog.inherits(app.ContainerRenderer, goog.ui.ContainerRenderer);
goog.addSingletonGetter(app.ContainerRenderer);

/**
 * @type {string}
 */
app.ContainerRenderer.CSS_CLASS = 'container';

/**
 * @inheritDoc
 */
app.ContainerRenderer.prototype.getCssClass = function() {
	return app.ContainerRenderer.CSS_CLASS;
};

/**
 * @override
 */
app.ContainerRenderer.prototype.createDom = function(container) {
	var element = container.getDomHelper().createDom(
		'div', this.getClassNames(container).join(' ')
	);
	// ...
	return element;
};

/**
 * @override
 */
app.ContainerRenderer.prototype.canDecorate = function(element) {
	return true;
};

/**
 * @override
 */
app.ContainerRenderer.prototype.decorate = function(container, element) {
	var element = goog.base(this, 'decorate', container, element);
	// ...
	return element;
};
```