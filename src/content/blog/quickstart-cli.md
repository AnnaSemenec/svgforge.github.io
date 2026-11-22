---

title: "Quickstart: Get started with the svgforge CLI in 5 minutes"
description: A quick introduction to the svgforge CLI that helps you install the tool, create your first sprite, and see how to use icons from the sprite in your own product.
pubDate: 2026-11-07
draft: true
tags:

- tutorial
- svg

---

In this guide, we'll build a **symbol sprite** using the command line and test its icons on a page via `<use>`.

svgforge also supports other sprite modes: `view`, `stack`, and `defs`. For details, see [Specific mode properties](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md#specific-mode-properties).

## Prerequisites

Node.js 24 or later

## Setup

**1.** Create a project folder to work in. Let's call it `svgforge-test`:

```bash
mkdir svgforge-test
cd svgforge-test
```

**2.** Install the svgforge CLI

```bash
npm install --global @svgforge/svgforge-cli
```

You should see output similar to this:

```text
added 54 packages in 48s
...
```

**3.** Verify the installation:

```bash
svgforge --help
```

If you see the list of commands, the CLI is installed correctly.

## Building the sprite

**1.** Add a few SVG icons to the `svgforge-test` folder. If you want to use icons from another folder, specify its path in the build command below.

**2.** Run the following command in the terminal:

```bash
svgforge --symbol --symbol-render-css --symbol-example --dest=out *.svg
```

This command:

- `--symbol` — selects the `symbol` mode
- `--symbol-render-css` — creates a CSS file
- `--symbol-example` — creates an HTML page with an example
- `--dest=out` — sets the main output directory
- `*.svg` — takes all SVG files from the current folder

To adapt the command to your project, you only need to change two parameters:

- `--dest=out`

  In this example, svgforge writes the generated files to the out folder.

  If you're using svgforge in an existing project, set `--dest` to the directory where you want the generated files to be written.

  For example, this could be `dist` or `public`, depending on your project's structure and build setup.

  `<OUTPUT_DIRECTORY>` is the directory you set in `--dest`. The generated files for the symbol mode are placed in the symbol subdirectory.

- `*.svg`.

  If you want to work with icons in an existing project, replace `*.svg` with a glob pattern that matches the location of your own icons.

  For example, `src/assets/icons/**/*.svg` matches SVG files in the src/assets/icons directory and all its subdirectories.

  ```bash
  svgforge --symbol --symbol-render-css --symbol-example --dest=<OUTPUT_DIRECTORY> 'src/assets/icons/**/*.svg'
  ```

If the output folder contains the generated files, everything is working — you've just built your first sprite. If you specified a directory other than out in --dest, look for the generated files there.

## Checking the sprite

**1.** Open the `out` folder or another folder you set in `--dest`. Inside, you should find the following files:

- `symbol/svg/sprite.symbol.svg`
- `symbol/sprite.css`
- `symbol/sprite.symbol.html`

**2.** Open `sprite.symbol.html` by double-clicking it, or right-click → **Open with** → your browser. A page with the icons from the sprite should open.

If you can see the icons, everything is working perfectly and you can move on.

If the page is empty or the icons are not displayed, your browser may be blocking external SVG references when the page is opened directly. In that case, serve the folder locally:

```bash
npx serve <OUTPUT_DIRECTORY>/symbol
```

In this example, the symbol output folder is `out/symbol`.

Now open the link shown in the terminal in your browser. It will look something like this:

```text
http://localhost:3000/sprite.symbol.html
```

## Using the sprite

Now the icons from the sprite can be used on any HTML page via `<use>`.

**1.** Create a file named `test-use.html` inside the `out/symbol` folder or another symbol output folder you set in `--dest`.

**2.** Paste the following code into the file:

```html
<svg width="32" height="32">
  <use href="svg/sprite.symbol.svg#home"></use>
</svg>
```

`#home` is the icon's identifier in the symbol sprite. By default, it matches the original SVG file name without the extension: `home.svg` → `home`.

Replace `home` with the ID of the icon you want to use.

**To find your icon's identifier:**

- Open the preview page: `http://localhost:3000/sprite.symbol.html`
- Find the icon you need.
- Click the **Copy ID** button next to it — the identifier will be copied to your clipboard.
- Paste it in place of `home` in `<use href="svg/sprite.symbol.svg#...">`.

**3.** If the local server is not running, restart it. Then open the `test-use.html` page in your browser. It will be available at a URL like:

```text
http://localhost:3000/test-use.html
```

The browser should display the icon whose identifier you specified in `<use>`.

## Learn more

- [SVG sprite modes](https://github.com/svgforge/svgforge#output-modes)
- [Command-line reference](https://github.com/svgforge/svgforge-cli/blob/main/docs/command-line.md)
- [Configuration reference](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md)
