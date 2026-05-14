# Paper-es

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

ES modules forked from Paper.js (The Swiss Army Knife of Vector Graphics Scripting)

## Usage

```js
import { Paper } from "https://code4fukui.github.io/Paper-es/Paper.js";

Paper.install(window);
```

Paper.js is based on dist/paper-core.js (without PaperScript)

## Example

```html
<!DOCTYPE html><html lang="ja"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width">
<title>rounded-rectangles - Paper-es</title>
<script type="module">
import { Paper, Tool, Path, Rectangle } from "https://code4fukui.github.io/Paper-es/Paper.js";

Paper.install(window); // -> project, tool, view

window.onload = () => {
	Paper.setup("canvas");

	let mousePoint = view.center;
	view.onMouseMove = (e) => {
		mousePoint = e.point;
	};

	const colors = ["red", "orange", "yellow", "green", "aqua", "blue", "purple"];
	const amount = colors.length * 3;
	const size = 20;
	for (let i = 0; i < amount; i++) {
		const rect = new Rectangle([0, 0], [size, size]);
		rect.center = mousePoint;
		const path = new Path.Rectangle(rect, size / 4);
		path.fillColor = colors[i % colors.length];
		const scale = (1 - i / amount) * 20;
		path.scale(scale);
	}

	const children = project.activeLayer.children;
	view.onFrame = (e) => {
		for (let i = 0; i < children.length; i++) {
			const item = children[i];
			const deltax = (mousePoint.x - item.position.x) / (i + 1);
			const deltay = (mousePoint.y - item.position.y) / (i + 1);
			item.rotate(Math.sin((e.count + i) / 8) * 7);
			if (deltax * deltax + deltay * deltay > 0.1 * .1) {
				item.position.x += deltax;
				item.position.y += deltay;
			}
		}
	}
};
</script>
</head>
<body>
<h1>rounded-rectangles - Paper-es</h1>
<canvas id="canvas" resize></canvas>

<style>
body {
	margin: 0;
	height: 100vh;
	text-align: center;
}
canvas {
	display: block;
	width: 100%;
	height: 80%;
}
</style>
</body>

</html>
```
[Paper-es examples](https://code4fukui.github.io/Paper-es/)

## See Also

If you want to work with Paper.js, simply download the latest "stable" version
from [http://paperjs.org/download/](http://paperjs.org/download/)

- Website: <http://paperjs.org/>
- Questions: <https://stackoverflow.com/questions/tagged/paperjs>
- Discussion forum: <https://groups.google.com/group/paperjs>
- Mainline source code: <https://github.com/paperjs/paper.js>
- Twitter: [@paperjs](https://twitter.com/paperjs)
- Latest releases: <http://paperjs.org/download/>
- Pre-built development versions:
  [`prebuilt/module`](https://github.com/paperjs/paper.js/tree/prebuilt/module)
  and [`prebuilt/dist`](https://github.com/paperjs/paper.js/tree/prebuilt/dist)
  branches.

### Which Version to Use

The various distributions come with two different pre-build versions of
Paper.js, in minified and normal variants:

- `paper-full.js` – The full version for the browser, including PaperScript
  support and Acorn.js
- `paper-core.js` – The core version for the browser, without PaperScript
  support nor Acorn.js. You can use this to shave off some bytes and compilation
  time when working with JavaScript directly.

### Installing Node.js and NPM

Node.js comes with the Node Package Manager (NPM). There are many tutorials
explaining the different ways to install Node.js on different platforms. It is
generally not recommended to install Node.js through OS-supplied package
managers, as the its development cycles move fast and these versions are often
out-of-date.

On macOS, [Homebrew](https://brew.sh/) is a good option if one version of
Node.js that is kept up to date with `brew upgrade` is enough:  
<https://treehouse.github.io/installation-guides/mac/node-mac.html>

[NVM](https://github.com/creationix/nvm) can be used instead to install and
maintain multiple versions of Node.js on the same platform, as often required by
different projects:  
<https://nodesource.com/blog/installing-node-js-tutorial-using-nvm-on-mac-os-x-and-ubuntu/>

Homebrew is recommended on macOS also if you intend to install Paper.js with
rendering to the Canvas on Node.js, as described in the next paragraph.

For Linux, see <https://nodejs.org/download/> to locate 32-bit and 64-bit
Node.js binaries as well as sources, or use NVM, as described in the paragraph
above.

### Installing Paper.js Using NPM

Paper.js comes in three different versions on NPM: `paper`, `paper-jsdom` and
`paper-jsdom-canvas`. Depending on your use case, you need to required a
different one:

- `paper` is the main library, and can be used directly in a browser
  context, e.g. a web browser or worker.
- `paper-jsdom` is a shim module for Node.js, offering headless use with SVG
  importing and exporting through [jsdom](https://github.com/tmpvar/jsdom).
- `paper-jsdom-canvas` is a shim module for Node.js, offering canvas rendering
  through [Node-Canvas](https://github.com/Automattic/node-canvas) as well as
  SVG importing and exporting through [jsdom](https://github.com/tmpvar/jsdom).

In order to install `paper-jsdom-canvas`, you need the [Cairo Graphics
library](https://cairographics.org/) installed in your system:

### Installing Native Dependencies

Paper.js relies on [Node-Canvas](https://github.com/Automattic/node-canvas) for
rendering, which in turn relies on the native libraries
[Cairo](https://cairographics.org/) and [Pango](https://www.pango.org/).

#### Installing Native Dependencies on macOS

Paper.js relies on Node-Canvas for rendering, which in turn relies on Cairo and
Pango. The easiest way to install Cairo is through
[Homebrew](https://brew.sh/), by issuing the command:

    brew install cairo pango

Note that currently there is an issue on macOS with Cairo. If the above causes
errors, the following will most likely fix it:

    PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig/ npm install paper

Also, whenever you would like to update the modules, you will need to execute:

    PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig/ npm update

If you keep forgetting about this requirement, or would like to be able to type
simple and clean commands, add this to your `.bash_profile` file:

    # PKG Config for Pango / Cairo
    export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/opt/X11/lib/pkgconfig

After adding this line, your commands should work in the expected way:

    npm install paper
    npm update

#### Installing Native Dependencies on Debian/Ubuntu Linux

    sudo apt-get install pkg-config libcairo2-dev libpango1.0-dev lib

MIT License — see [LICENSE](LICENSE).