## goog.array
### goog.array.forEach
```javascript
app.Foo = function() {
	var dataToAdd = [1, 2, 3];
	this.bar = 0;

	goog.array.forEach(dataToAdd, function(item) {
		this.bar += item;
	}, this);

	// this.bar 6
};

/**
 * @type {number}
 */
app.Foo.prototype.bar;
```


## goog.is...
```javascript
// @return true/false
goog.isArrayLike(val);
goog.isDef(val);
goog.isDefAndNotNull(val);
goog.isNull(val);
goog.isNumber(val);
goog.isFunction(val);
goog.isObject(val);
goog.isString(val);
```

## goog.json

### goog.json.parse
```javascript
/**
 * Safe parse
 *
 * @param {*} s The JSON string to parse.
 * @return {Object} The object generated from the JSON string.
 */
goog.json.parse = function(s) {}
```

### goog.json.serialize
```javascript
/**
 * Serializes an object or a value to a JSON string.
 *
 * @param {*} object The object to serialize.
 * @param {?goog.json.Replacer=} opt_replacer
 * @throws Error if there are loops in the object graph.
 * @return {string} A JSON string representation of the input.
 */
goog.json.serialize = function(object, opt_replacer) {}
```

### goog.json.unsafeParse
```javascript
/**
 * @param {string} s The JSON string to parse.
 * @return {Object} The object generated from the JSON string.
 */
goog.json.unsafeParse = function(s) {}
```

## goog.object

### goog.object.getValues
```javascript
var obj = {
	"key1": "value1",
	"key2": "value2"
};

var arr = goog.object.getValues(obj); // ["value1", "value2"]
```

### goog.object.setIfUndefined
```javascript
var obj = {
	"key1": "value1",
	"key2": "value2"
};

var set1 = goog.object.setIfUndefined(obj, 'key1', 'value11111'); // value1
var set2 = goog.object.setIfUndefined(obj, 'key3', 'value3'); // value3
// obj {key1: "value1", key2: "value2", key3: "value3"}
```

## goog.string

### goog.string.padNumber
```javascript
goog.string.padNumber(1.5, 2, 2)); // 01.50
goog.string.padNumber(1.5, 2, 0)); // 02
```

### goog.string.trim
```javascript
goog.string.trim('    string-to-trim     '); // string-to-trim
```

goog.string.htmlEscape
```javascript
goog.string.htmlEscape(
	'<a href="href">Link</a>' // &lt;a href=&quot;href&quot;&gt;Link&lt;/a&gt;
);

```

### goog.string.regExpEscape
```javascript
goog.string.regExpEscape('Strange (character)'); // Strange \(character\)
```

### goog.string.whitespaceEscape
```javascript
goog.string.whitespaceEscape(
	'Text\nwith whitespace', // Text<br />with &nbsp;whitespace
	true
);
```

## goog.userAgent
```javascript
if (goog.userAgent.IE) {
	// run away
};

/**
 * Condition will be true for IE < 9.0
 * @see goog.string.compareVersions
 */
if (goog.userAgent.IE && goog.userAgent.compare(9, goog.userAgent.VERSION) == 1) {
	// run away even faster
}

/**
 * Condition will be true for IE >= 10.0
 */
if (goog.userAgent.IE && goog.userAgent.isVersionOrHigher(10)) {
	// ...
}
```

## goog.typeOf
```javascript
var a = ...;
goog.typeOf(a);
// 'array', 'object', 'function', 'null', 'string', 'number', 'boolean', 'undefined'
```
