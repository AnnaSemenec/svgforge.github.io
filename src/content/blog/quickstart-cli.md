---

title: "Quickstart: Get started with the svgforge CLI in 5 minutes"
description: A quick introduction to the svgforge CLI that helps you install the tool, create your first sprite, and see how to use icons from the sprite in your own product.
pubDate: 2026-11-07
tags:

- tutorial
- svg

---

In this guide, we'll build a **symbol sprite** using the command line and test its icons on a page via `<use>` — the most modern and widely used approach.

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

**1.** Add a few SVG icons to the `svgforge-test` folder.

**2.** Run the following command in the terminal:

```bash
svgforge --symbol --symbol-render-css --symbol-example --dest=out *.svg
```

This command:

- `--symbol` — selects the `symbol` mode (the most popular one for icons)
- `--symbol-render-css` — creates a CSS file
- `--symbol-example` — creates an HTML page with an example
- `--dest=out` — puts everything into the `out` folder
- `*.svg` — takes all SVG files from the current folder

If the `out` folder appears with the generated files, everything is working — you've just built your first sprite.

## Checking the sprite

**1.** Open the `out/symbol` folder. Inside, you should find the following files:

- `symbol/svg/sprite.symbol.svg`
- `symbol/sprite.css`
- `symbol/sprite.symbol.html`

**2.** Open `sprite.symbol.html` by double-clicking it, or right-click → **Open with** → your browser. A page with the icons from the sprite should open.

If you can see the icons, everything is working perfectly and you can move on.

If the page is empty or the icons are not displayed, your browser may be blocking external SVG references when the page is opened directly. In that case, serve the folder locally:

```bash
npx serve out/symbol
```

Open the following URL in your browser:

```text
http://localhost:3000/sprite.symbol.html
```

## Using the sprite

Now the icons from the sprite can be used on any HTML page via `<use>`.

**1.** Create a file named `test-use.html` inside the `out/symbol` folder.

**2.** Paste the following code into the file:

```html
<svg width="32" height="32">
  <use href="svg/sprite.symbol.svg#home"></use>
</svg>
```

`#home` is the icon's identifier in the symbol sprite. By default, it matches the original SVG file name without the extension: `home.svg` → `home`.

**To find your icon's identifier:**

- Open the preview page: `http://localhost:3000/sprite.symbol.html`
- Find the icon you need.
- Click the **Copy ID** button next to it — the identifier will be copied to your clipboard.
- Paste it in place of `home` in `<use href="svg/sprite.symbol.svg#...">`.

**3.** If the local server is not running, restart it. Then open the following URL in your browser:

```text
http://localhost:3000/test-use.html
```

The browser should display the icon whose identifier you specified in `<use>`.

## Learn more

- [SVG sprite modes](https://github.com/svgforge/svgforge#output-modes)
- [Command-line reference](https://github.com/svgforge/svgforge-cli/blob/main/docs/command-line.md)
- [Configuration reference](https://github.com/svgforge/svgforge/blob/main/docs/configuration.md)
