## DOM

### :first, :last
```html
<ul id="menu">
	<li>Item 1</li>
	<li>Item 2</li>
	<li>Item 3</li>
</ul>
```
```javascript
// jQuery
var firstItem = $('#menu li:first')[0];
var lastItem = $('#menu li:last')[0];

// closure
var menuLi = goog.dom.getElementsByTagNameAndClass(
	'li',
	undefined,
	goog.dom.getElement('menu')
);

var firstItem = menuLi[0],
	lastItem =  menuLi[menuLi.length - 1];
```

### .prepend()
```html
<ul id="menu">
	<li>Item 1</li>
	<li>Item 2</li>
	<li>Item 3</li>
</ul>
```
```javascript
// jQuery
$('#menu').prepend('<li>Item 0</li>');

// closure
var menu = goog.dom.getElement('menu');
var newItem = goog.dom.createDom('li', undefined, 'Item 0');
goog.dom.insertChildAt(menu, newItem, 0);
```

## Event

### Click handler

```html
<div id="container" style="width: 100px; height: 100px;"></div>
```

```javascript
// jQuery
$('#container').click(function(e) {
	// ...
});

// closure
goog.events.listen(
	goog.dom.getElement('container'),
	goog.events.EventType.CLICK,
	function(e) {
		// ...
	}
);
```
