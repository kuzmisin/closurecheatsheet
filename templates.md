## Soy
+ https://developers.google.com/closure/templates/docs/concepts
+ https://developers.google.com/closure/templates/docs/commands
+ https://github.com/google/closure-templates/tree/master/examples/simple.soy
+ https://github.com/google/closure-templates/tree/master/examples/features.soy

### Basic
```
{namespace app.template}

/**
 * DOC
 */
{template .test}
{@param p1: string}
{@param p2: string}
<div>{$p1}</div> // one line comment
<div>{$p2}</div> /* block comment */
{/template}
```

### Character commands
```
/**
 * Character commands
 */
{template .test}
{sp} = space
{nil} = empty string
{\n} = newline (line feed)
{\r} = carriage return
{\t} = tab
{lb} = left brace
{rb} = right brace
{/template}
```

```
/**
 * RAW text example
 */
{template .test}
{literal}
	RAW text ...
	preformatted
{/literal}
{/template}
```

### Commands
```
{print ...}
{...}    // implied 'print' command
{css ...}
{let ... /}
{let ...}...{/let}
{if ...}...{elseif ...}...{else ...}...{/if}
{switch ...}{case ...}...{default}...{/switch}
{foreach ...}...{ifempty}...{/foreach}
{for ...}...{/for}
{call ... /}
{delcall ... /}
{call ...}{param ... /}{param ...}...{/param}{/call}
{delcall ...}{param ... /}{param ...}...{/param}{/delcall}
{log}...{/log}
{debugger}
{msg ...}...{/msg}
```

```
/**
 * LET example
 */
{template .test}
{let $a}10{/let}
{let $b:10 /}
Value for a = {$a}
Value for b = {$b}
{/template}
```

```
/**
 * LOG example -> `window.console.log(...);`
 */
{template .test}
{@param p1: number}
<div>...</div>
{log}Value of p1 = {$p1}{/log}
{/template}
```

```
/**
 * DEBUGGER example -> `debugger;`
 */
{template .test}
{if ...}
	{debugger}
{/if}
{/template}
```

### Directive (helpers)
```
/**
 * Soy Directives.
 */
{template .test}
{@param p1: any}
{$p1|noAutoescape}
{$p1|escapeCssString}
{$p1|filterCssValue}
{$p1|cleanHtml}
{$p1|escapeHtmlRcdata}
{$p1|escapeHtmlAttribute}
{$p1|escapeHtmlAttributeNospace}
{$p1|filterHtmlAttributes}
{$p1|filterHtmlElementName}
{$p1|escapeJsRegex}
{$p1|escapeJsString}
{$p1|escapeJsValue}
{$p1|filterNormalizeUri}
{$p1|normalizeUri}
{$p1|escapeUri}
{$p1|changeNewlineToBr}
{$p1|insertWordBreaks:1}
{$p1|truncate:1,false}
{/template}
```

```
/**
 * Assume p1 = "<div>content</div>"
 */
{template .test}
{@param p1: html}
{$p1|cleanHtml} // shows only `content`
{/template}
```

```
/**
 * Assume p1 = "Test value"
 */
{template .test}
{@param p1: string}
{$p1|truncate:4,false} // shows `Test`
{$p1|truncate:4,true} // shows `T...`
{/template}
```

### Function
```
/**
 * Soy Functions
 */
{template .test}
{@param p1: any} /** Dummy example parameter. */
{@param p2: any} /** Dummy example parameter. */
{augmentMap($p1, $p2)}
{ceiling($p1)}
{floor($p1)}
{isNonnull($p1)}
{keys($p1)}
{length($p1)}
{max($p1, $p2)}
{min($p1, $p2)}
{randomInt($p1)}
{round($p1)}
{strContains($p1, $p2)}
{/template}
```

### i18n
```
/**
 * MSG-PLURAL example -> `goog.getMsg`, `goog.i18n.MessageFormat()`
 */
{template .test}
{@param p1: int}
{msg desc="Test message"}
	{plural $p1}
		{case 1}
			Message if p1 = 1
		{case 2}
			Message if p1 = 2
		{default}
			Message for all other cases
	{/plural}
{/msg}
{/template}
```

```
/**
 * MSG-SELECT example -> `goog.getMsg`, goog.i18n.MessageFormat()`
 */
{template .test}
{@param p1: string}
{msg desc="Test message"}
	{select $p1}
		{case 'male'}
			Message for male
		{case 'female'}
			Message for female
		{default}
			Message for all other cases
	{/select}
{/msg}
{/template}
```

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
 */
{template .test}
{@param first: number}
{@param second: number}
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
