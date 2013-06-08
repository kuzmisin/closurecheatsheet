For more examples visit [compiler annotation](https://developers.google.com/closure/compiler/docs/js-for-compiler)

## Usage
```javascript
/**
 * @param {type} a
 * @return {type}
 */
function foo(a) {
	return a;
}
```

```javascript
var b = /** @type {type} */ (a);
```

```javascript
foo(/** @type {type} */ (a));
```

```javascript
/**
 * @type {type}
 */
Foo.prototype.a;
```

```javascript
/**
 * @type {*} Whatever
 */
Foo.prototype.a;

/**
 * @type {string|number|undefined} String or number or undefined
 */
Foo.prototype.b;

/**
 * @type {?string} String or null
 */
Foo.prototype.c;
```

```javascript
/**
 * @param {!string} a Required parameter
 * @param {string=} opt_b Optional parameter
 */
Foo.prototype.bar = function(a, opt_b) {

}
```

## Primitive types
```javascript
/** @type {number} */
/** @type {string} */
/** @type {boolean} */
/** @type {undefined} */
/** @type {null} */
```

## Const
```javascript
/**
 * @const
 * @type {number}
 */
Foo.A = 1;

/**
 * @const
 * @type {string}
 */
Foo.B = 'text';
```

## Array
```javascript
/**
 * @param {Array}
 */

/**
 * @param {Array.<*>} Array of unknown/whatever items
 * @param {Array.<string>} Items in array are strings
 * @param {Array.<Object>} Items in array are Objects
 */

/**
 * @param {Array.<Array.<string>>} Items in array are Arrays of strings
 */
```

## Object
```javascript
/**
 * @param {Object}
 */

/**
 * @param {Object.<number, string>} Object with number keys and string values
 */
```

## Function
```javascript
/**
 * @param {Function}
 */

/**
 * @param {function(): boolean} Param is a function which return boolean
 * @param {function(number): boolean} Function with number as parameter and return boolean
 */
```

## Html
```javascript
/** @type {Element} */
/** @type {Document} */
/** @type {DocumentFragment} */
/** @type {Node} */
/** @type {NodeList} */
/** @type {Window} */

/** @type {HTMLAnchorElement} */
/** @type {HTMLDocument} */
/** @type {HTMLElement} */
/** @type {HTMLFormElement} */
/** @type {HTMLImageElement} */
```

## Typedef
```javascript
/**
 * @typedef {{
 *      id: number,
 *      direction: string
 *  }}
 */
Foo.NAVIGATE_INFO;

/**
 * @type {Foo.NAVIGATE_INFO}
 */
Foo.prototype.info;
```

```javascript
/**
 * Function returning "typedef". Property "style" can be string or null.
 *
 * @return {{link: string, href: string, style: (string|null)}}
 */
Foo.prototype.getLink = function() {
	// ...
	return {
		link: 'Link',
		href: 'http://...',
		style: null
	};
}
```

## Enum
```javascript
/**
 * @enum {string}
 */
Foo.Api = {
	GET: '/get',
	PUT: '/put'
}

/**
 * @enum {number}
 */
Foo.Direction= {
	UP: 1,
	DOWN: 2
}
```

## VarArgs
```javascript
/**
 * @param {...string} var_args
 */
Foo.prototype.insertAll = function(var_args) {
	// arguments, arguments.length
};
```

## OOP
```javascript
/**
 * @interface
 */
Foo = function() {};

/**
 * @param {number} a
 */
Foo.prototype.bar = function(a) {};
```

```javascript
/**
 * @constructor
 * @implements {Foo}
 */
app.Some = () {

};

/**
 * @param {number} a
 */
app.Some.prototype.bar = function(a) {

};

```

```javascript
/**
 * @constructor
 * @extends {app.Some}
 */
app.Other = () {
	goog.base(this);
};
```
