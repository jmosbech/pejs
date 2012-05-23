# PEJS

PEJS is pre-compiled EJS with a inheritance, blocks and file support that works both in the client and on the server.  
It's available through npm:

	npm install pejs

## Disclamer

This module and documentation is still a work in progress so stuff might break and change.

## Usage

PEJS is easy to use:

``` js
var pejs = require('pejs');
var template = pejs('./templates'); // the template dir (defaults to .)

template.compile('example.ejs', function(err, render) {
	// compiles test.html into a rendering function
	console.log(render());
});
template.parse('example.ejs', function(err, src) {
	// parses the template and compiles it down to portable js
	// this means it works in the client!
	console.log(src);
});
```

## Classic EJS

PEJS templates has your usual EJS syntax with `<%` and `%>`. Read more about EJS [here](http://embeddedjs.com/)

* inline code: `<% var a = 42; %>`
* insert: `<%- data %>`
* escape: `<%= data %>`

## Blocks

PEJS expands the original EJS syntax by letting you declare blocks using the `<%{` syntax.  
A block is basically a partial template that optionally can be loaded from a file.

* declare block: `<%{{ blockName }}%>`
* declare file block: `<%{ 'filename.html' }%>`
* override block: `<%{ blockName %>hello block<%} %>`

In general all block can be loaded from a file instead of being defined inline by providing a filename:

* declare block: `<%{{ myBlock 'example.ejs' }}%>`
* override block: `<%{ myOverrideBlock 'example.ejs' }%>`

If you want include a block using a different set of locals than in you current scope you pass these as the last argument to the block.

* declare block: `<%{{ myBlock {newLocalsArg:oldLocalsArg} }}%>`
* override block: `<%{ 'example.ejs', newLocalsHere }%>`

## Inheritance

Using blocks it's easy to implement template inheritance.  
Just declare a `base.html` with some anchored blocks:

	<body>
		Hello i am base
		<%{{ content }}%>
	</body>

Then a `child.html` that renders `base.html`

	<%{ 'base.html' }%>
	<%{ content %>
		i am inserted in base
	<%} %>

To render the example just render `child.html`

``` js
template.compile('child.html', function(err, render) {
	console.log(render());
});
```

The above outputs:

	<body>
		Hello i am base
		i am inserted in base		
	</body>

## License

MIT