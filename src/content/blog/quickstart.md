---

title: "Quickstart: Build your first sprite in 5 minutes"
description: A quick introduction to svgforge that helps you install the tool, create your first sprite, and see how to use icons from the sprite in your own product.
pubDate: 2026-11-07
draft: true
tags:

- tutorial
- svg

---

In this guide, we'll build a **symbol sprite** and test its icons on a page using `<use>`.

svgforge also supports other sprite modes: `view`, `stack`, and `defs`. For details, see [Specific mode properties](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md#specific-mode-properties).

## Prerequisites

Node.js 24 or later

## Setup

**1.** Create a project folder to work in. Let's call it `svgforge-project`.

**2.** Download a few SVG icons into that folder.

**3.** Install the svgforge:

```bash
npm install @svgforge/svgforge
```

You should see output similar to this in the terminal:

```
added 38 packages in 1m

12 packages are looking for funding
  run `npm fund` for details
```

## Building the sprite

**1.** Create an `.mjs` file in the project folder (for example, `build-sprite.mjs`) and paste in the following code:

```js
import fs from "node:fs";
import path from "node:path";
import SVGSpriter from "@svgforge/svgforge";

const config = {
	dest: "out-lib",
	mode: {
		symbol: {
			example: true,
		},
	},
};

const spriter = new SVGSpriter(config);

for (const file of fs.globSync("*.svg")) {
	spriter.add(file, null, fs.readFileSync(file, "utf8"));
}

const { result } = await spriter.compileAsync();

for (const mode of Object.values(result)) {
	for (const resource of Object.values(mode)) {
		fs.mkdirSync(path.dirname(resource.path), { recursive: true });
		fs.writeFileSync(resource.path, resource.contents);
	}
}
```

This script uses svgforge to combine SVG icons from your project into a single **symbol sprite** and saves the generated files to the `out-lib` directory.

- `dest: "out-lib"` sets the output directory where the generated files are written.

  In this example, the generated files are written to the `out-lib` directory in the root of your `svgforge-project` folder.

  If you're using svgforge in an existing project, set `dest` to the directory where you want the generated files to be written. For example, this could be `dist` or `public`, depending on your project's structure and build setup.

  `<OUTPUT_DIRECTORY>` is the directory you set in `dest`.

- `fs.globSync("*.svg")` finds SVG files matching the specified glob pattern.
  Replace `*.svg` with the path to your icon directory. For example, `src/assets/icons/**/*.svg` matches SVG files in the `src/assets/icons` directory and all its subdirectories.

Save the file.

**2.** Run the `.mjs` file in the terminal:

```bash
node build-sprite.mjs
```

If you named the file differently in the previous step, replace `build-sprite.mjs` with your file name. For example, if you created `icons.mjs`, run:

```bash
node icons.mjs
```

Two files will appear in the `<OUTPUT_DIRECTORY>/symbol/` folder:

- `sprite.symbol.svg` — the sprite itself
- `sprite.symbol.html` — a preview page (generated because of `example: true`)

If you set `dest: "out-lib"`, the files will appear in the `out-lib/symbol/` folder.

## Checking the sprite

**1.** Start a local server:

```bash
npx serve <OUTPUT_DIRECTORY>
```
If you set `dest: "out-lib"`, run:

```bash
npx serve out-lib
```

**2.** Open in your browser:

```
http://localhost:3000/symbol/sprite.symbol.html
```

If the terminal shows a different port, use that port in the URL.

You should see a confirmation that the sprite is ready. Now the icons from the sprite can be used on any page via `<use>`.

## Using the sprite

**1.** Create an HTML file inside the `<OUTPUT_DIRECTORY>/symbol/` folder. In this example — `out-lib/symbol/`.

Name it `test-use.html`.

**2.** Paste the following code into the file:

```html
<svg width="32" height="32">
  <use href="svg/sprite.symbol.svg#home"></use>
</svg>
```

`#home` is the icon's identifier in the symbol sprite. By default, it matches the original SVG file name without the extension: `home.svg` → `home`.

Replace `home` with the ID of the icon you want to use.

**How to find your icon's identifier:**

- Open the preview page: `http://localhost:3000/symbol/sprite.symbol.html`
- Find the icon you need.
- Click the **Copy ID** button next to it — the identifier will be copied to your clipboard.
- Paste it in place of `home` in `<use href="svg/sprite.symbol.svg#...">`.

Save the file.

**3.** If the local server is not running, restart it. Then open in your browser:

```
http://localhost:3000/symbol/test-use.html
```

The browser should display the icon whose identifier you specified in `<use>`.

## Learn more

- [Using symbol sprites via `<use>`](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md#defs--symbol-mode)
- [Other sprite modes: `view`, `stack`, `defs`](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md#specific-mode-properties)
- [Full sprite configuration documentation](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md)
