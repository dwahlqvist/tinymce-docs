---
layout: default
title: Text Pattern plugin
title_nav: Text Pattern
description: Matches special patterns in the text and applies formats or executed commands on these patterns.
keywords: textpattern textpattern_patterns format cmd
---

The Text Pattern plugin matches special patterns in the text and applies formats, replaces text, or executes commands on these patterns.

The default pattern is similar to markdown syntax, so the user can type `# text` to produce a header or `**text**` to make text **bold**.

**Type:** `String`

##### Example

```js
tinymce.init({
  selector: 'textarea',
  plugins: 'textpattern'
});
```

### Options

This setting affects the execution of the `textpattern` plugin. Text patterns that are matched by the editor can be changed here.

### `textpattern_patterns`

This option allows configuring the text patterns that get matched by the `textpattern` plugin. By default, it has basic markdown patterns.

There are three types of patterns: `inline`, `block`, and `replacement` patterns. Inline patterns have a `start` and an `end` text pattern whereas the block and replacement patterns only have a `start`. A user can specify the formats to be applied to the selection, commands to be executed, or text to be replaced.

When using list commands make sure that the [lists plugin]({{ site.baseurl }}/plugins/lists) is registered for normalized behavior across browsers.

> Note: Formats and commands must be already registered with the editor. See the [formats]({{ site.baseurl }}/configure/content-formatting/#formats) and [commands]({{ site.baseurl }}/api/tinymce/tinymce.editorcommands/) documentation for more information.

**Default Value:**

```js
[
  {start: '*', end: '*', format: 'italic'},
  {start: '**', end: '**', format: 'bold'},
  {start: '#', format: 'h1'},
  {start: '##', format: 'h2'},
  {start: '###', format: 'h3'},
  {start: '####', format: 'h4'},
  {start: '#####', format: 'h5'},
  {start: '######', format: 'h6'},
  {start: '1. ', cmd: 'InsertOrderedList'},
  {start: '* ', cmd: 'InsertUnorderedList'},
  {start: '- ', cmd: 'InsertUnorderedList' }
]
```

#### Inline patterns

Inline patterns must have the following:

* A `start` and an `end`.
* A `format` or a `cmd`.
  * If `cmd` is specified, an optional `value` property is allowed.

This allows for patterns to be used to either apply a format or execute a command, optionally with the given value.

> Note: Inline patterns are executed on either pressing the **spacebar** or the **Enter** key.

##### Example

```js
tinymce.init({
  selector: 'textarea',  // change this value according to your HTML
  plugin: 'textpattern',
  textpattern_patterns: [
    {start: '*', end: '*', format: 'italic'},
    {start: '**', end: '**', format: 'bold'},
    {start: '~', end: '~', cmd: 'createLink', value: 'https://tiny.cloud'}
  ]
});
```

Using the configuration in this example:

* `{start: '*', end: '*', format: 'italic'}` - Entering text between`*` and then pressing the **spacebar** will result in the `italic` format being applied to the text between the `*` symbols.
* `{start: '**', end: '**', format: 'bold'}` - Entering text between`**` and then pressing the **spacebar** will result in the `bold` format being applied.
* `{start: '~', end: '~', cmd: 'createLink', value: 'https://tiny.cloud'}` - This executes `editor.execCommand('createLink', false, 'https://tiny.cloud')`, which will wrap the text between the `~` symbols in a link that points to `https://tiny.cloud`.

#### Block patterns

Block patterns must have the following:

* A `start`
* A `format` or a `cmd`
   * If `cmd` is specified, an optional `value` property is allowed.

The block patterns do not have an `end` property. This allows for patterns to be used to either apply a block format or execute a command, optionally, with the given value.

> Note: Block patterns are only executed on **Enter**, **not** on pressing the **spacebar**.

##### Example

```js
tinymce.init({
  selector: 'textarea',  // change this value according to your HTML
  plugin: 'textpattern lists',
  textpattern_patterns: [
    {start: '#', format: 'h1'},
    {start: '##', format: 'h2'},
    {start: '###', format: 'h3'},
    {start: '####', format: 'h4'},
    {start: '#####', format: 'h5'},
    {start: '######', format: 'h6'},
    {start: '* ', cmd: 'InsertUnorderedList'},
    {start: '- ', cmd: 'InsertUnorderedList'},
    {start: '1. ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'decimal' }},
    {start: '1) ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'decimal' }},
    {start: 'a. ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'lower-alpha' }},
    {start: 'a) ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'lower-alpha' }},
    {start: 'i. ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'lower-roman' }},
    {start: 'i) ', cmd: 'InsertOrderedList', value: { 'list-style-type': 'lower-roman' }}
  ]
});
```

Using the configuration in this example:

* `{start: '#', format: 'h1'}` - Typing `#`, some text, and then pressing `Enter` will convert the text to a `h1` heading.
* Typing `1. `, some text, and then pressing `Enter` will convert the text into an ordered list, with the original text as the first list item, and the new line as the second list item. Since we have specified `value`, this pattern will execute `editor.execCommand('InsertOrderedList', false, { 'list-style-type': 'decimal'})`.

#### Replacements patterns

Replacement patterns must have the following:

* A `start`.
* A `replacement`, which takes a string that can be text or HTML.

Whether a replacement pattern inserts a block or inline element depends on what the `replacement` string is.

> Note: Replacement patterns are executed on either pressing the **spacebar** or the **Enter** key.


##### Example

```js
tinymce.init({
  selector: 'textarea',  // change this value according to your HTML
  plugin: 'textpattern',
  textpattern_patterns: [
    {start: '---', replacement: '<hr/>'},
    {start: '--', replacement: '—'},
    {start: '-', replacement: '—'},
    {start: '(c)', replacement: '©'},
    {start: '//brb', replacement: 'Be Right Back'},
    {start: '//heading', replacement: '<h1 style="color: blue">Heading here</h1> <h2>Author: Name here</h2> <p><em>Date: 01/01/2000</em></p> <hr />'},
  ]
});
```

Using the configuration in this example:

* Typing `---` and then either pressing the **spacebar** or the **Enter** key will insert a horizontal rule block.
* Typing `(c)` and then either pressing the **spacebar** or the **Enter** key will insert an inline copyright symbol.

This is useful for commonly used phrases or symbols and can be leveraged to create content templates. The last pattern is an example of this.
