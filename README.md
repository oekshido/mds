# MarkdownScript

Markdown powered scripts. [API documentation](api.md).

Note: This package needs to be bundled with
[jailed](https://github.com/asvd/jailed).

## Usage

In HTML:

```html
...
<div id="scriptid"></div>
...
```

In JS:

```javascript
mds.create("scriptid", code, options)
```

## MDS format

Markdown with scripting capabilities. Variables are marked with double curly
braces (`{{}}`). They all follow the same format, `{{type:name:data}}`, where
the type and data follows the table below, the name is unique and the `:data`
is optional. The code itself is written after the markdown, and is separated
with `{{{{`. It needs to be javascript. To get functions like the `Math` module,
you need to include them in the `bindings` argument.

The script must be a function with two arguments, the arguments themselves in an
object, and the return callback. The arguments' names are the mds `name`
attribute (see example below). The output must be a dictionary, with the keys
being the `name` of the output variables in which the value will be displayed.
For simple programs with only one output, the value for it can passed directly
to the callback function, if the output's `name` is `output`. The name of the
function corresponds to the `name` attribute of the `run` variable, which means
one script can have multiple functions.

## Types

| Type      | Description           | Function | Data                         |
| --------- | --------------------- | -------- | ---------------------------- |
| shorttext | Short text input.     | I/O      | Text placeholder.            |
| text      | Textarea.             | I/O      | Text placeholder.            |
| checkbox  | Returns boolean.      | I/O      | Default value (1 or 0).      |
| options   | Selection of strings. | Input    | Comma separated values.      |
| infile    | File input.           | Input    | No Use.                      |
| outmd     | Displays markdown.    | Output   | No Use.                      |
| outraw\*  | Displays html.        | Output   | No Use.                      |
| outfile   | Exports file.         | Output   | _filename_,_anchor tag text_ |
| run       | Button to run script. | Run      | Button Text.                 |

Note: `outraw` is disabled by default, and can only be activated by setting the
`outraw` option to `true`.

## Example

```javascript
# Fibonacci Generator

Enter amount of numbers: {{shorttext:n}}

{{run:runname:Generate!}}

{{outmd:outputname}}

{{{{
function runname({n = "10"}, ret){
	n = Number(n) - 2
	let fib = [0, 1]
	for(let i = 0; i < n; i++){
		fib.push(fib.slice(-2).reduce((a, b) => a + b, 0))
	}
	ret({outputname:fib})
}
```

With shorthand:

```javascript
# Fibonacci Generator

Enter amount of numbers: {{shorttext:n}}

{{run:runname:Generate!}}

{{outmd:output}} // Note: the name must be "output"

{{{{
function runname({n = "10"}, ret){
	n = Number(n) - 2
	let fib = [0, 1]
	for(let i = 0; i < n; i++){
		fib.push(fib.slice(-2).reduce((a, b) => a + b, 0))
	}
	ret(fib) // This will be considered as {output:fib}
}
```

For another example, check the `test/test.html` file.
