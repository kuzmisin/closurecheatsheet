## Soy
+ https://code.google.com/p/closure-templates/source/browse/trunk/examples/simple.soy
+ https://code.google.com/p/closure-templates/source/browse/trunk/examples/features.soy

### Call function outside SOY
```javascript
goog.provide('app.helpers');

/**
 * @param {Object.<string, *>=} opt_data
 * @param {(null|undefined)=} opt_ignored
 * @return {string}
 * @notypecheck
 */
app.helpers.sum = function(opt_data, opt_ignored) {
	return "" + (opt_data.first + opt_data.second);
};
```

```
{namespace app.templates}

/**
 * Calling function outside SOY
 * @param first
 * @param second
 */
{template .test}
<div>
	Sum <em>{$first}</em> and <em>{$second}</em> is{sp}
	<strong>
		{call app.helpers.sum}
			{param first: $first /}
			{param second: $second /}
		{/call}
	</strong>
</div>

<div>
	Sum <strong>{call app.helpers.sum data="all" /}</strong>
</div>
{/template}
```

```javascript
var template = app.templates.test({
	first: 1,
	second: 5
});
```

## goog.soy.renderElement
```javascript
var templateData = {
	first: 1,
	second: 5
};

goog.soy.renderElement(
	goog.dom.getElement('container'), // element with innerHTML
	app.templates.test, // template
	templateData
);
```