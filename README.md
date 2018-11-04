<p align="center">
  <img src="https://i.imgur.com/J0jv1Sz.png" width="240" height="240" alt="critters-webpack-plugin">
  <h1 align="center">Critters</h1>
</p>

> Critters is a Webpack plugin that inlines your app's [critical CSS] and lazy-loads the rest.

## critters-webpack-plugin [![npm](https://img.shields.io/npm/v/critters-webpack-plugin.svg?style=flat)](https://www.npmjs.org/package/critters-webpack-plugin)

It's a little different from [other options](#similar-libraries), because it **doesn't use a headless browser** to render content.  This tradeoff allows Critters to be very **fast and lightweight**. It also means Critters inlines all CSS rules used by your document, rather than only those needed for above-the-fold content. For alternatives, see [Similar Libraries](#similar-libraries).

Critters' design makes it a good fit when inlining critical CSS for prerendered/SSR'd Single Page Applications. It was developed to be an excellent compliment to [prerender-loader](https://github.com/GoogleChromeLabs/prerender-loader), combining to dramatically improve first paint time for most Single Page Applications.

## Features

-   Fast - no browser, few dependencies
-   Integrates with [html-webpack-plugin]
-   Works with `webpack-dev-server` / `webpack serve`
-   Supports preloading and/or inlining critical fonts
-   Prunes unused CSS keyframes and media queries
-   Removes inlined CSS rules from lazy-loaded stylesheets

## Installation

First, install Critters as a development dependency:

```sh
npm i -D critters-webpack-plugin
```

Then, import Critters into your Webpack configuration and add it to your list of plugins:

```diff
// webpack.config.js
+const Critters = require('critters-webpack-plugin');

module.exports = {
  plugins: [
+    new Critters({
+      // optional configuration (see below)
+    })
  ]
}
```

That's it! Now when you run Webpack, the CSS used by your HTML will be inlined and the imports for your full CSS will be converted to load asynchronously.

## Usage

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Critters

Create a Critters plugin instance with the given options.

**Parameters**

-   `options` **Options** Options to control how Critters inlines CSS.

**Examples**

```javascript
// webpack.config.js
module.exports = {
  plugins: [
    new Critters({
      // Outputs: <link rel="preload" onload="this.rel='stylesheet'">
      preload: 'swap',

      // Don't inline critical font-face rules, but preload the font URLs:
      preloadFonts: true
    })
  ]
}
```

### Critters

All optional. Pass them to `new Critters({ ... })`.

**Parameters**

-   `options`  

**Properties**

-   `external` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Inline styles from external stylesheets _(default: `true`)_
-   `inlineThreshold` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Inline external stylesheets smaller than a given size _(default: `0`)_
-   `pruneSource` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Remove inlined rules from the external stylesheet _(default: `true`)_
-   `mergeStylesheets` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Merged inlined stylesheets into a single <style> tag _(default: `true`)_
-   `preload` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Which [preload strategy](#preloadstrategy) to use
-   `noscriptFallback` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Add `<noscript>` fallback to JS-based strategies
-   `inlineFonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Inline critical font-face rules _(default: `false`)_
-   `preloadFonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Preloads critical fonts _(default: `true`)_
-   `fonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Shorthand for setting `inlineFonts`+`preloadFonts`-   Values:
    -   `true` to inline critical font-face rules and preload the fonts
    -   `false` to don't inline any font-face rules and don't preload fonts
-   `keyframes` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Controls which keyframes rules are inlined.-   Values:
    -   `"critical"`: _(default)_ inline keyframes rules used by the critical CSS
    -   `"all"` inline all keyframes rules
    -   `"none"` remove all keyframes rules
-   `compress` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Compress resulting critical CSS _(default: `true`)_

### PreloadStrategy

The mechanism to use for lazy-loading stylesheets.
_[JS]_ indicates that a strategy requires JavaScript (falls back to `<noscript>`).

-   **default:** Move stylesheet links to the end of the document and insert preload meta tags in their place.
-   **"body":** Move all external stylesheet links to the end of the document.
-   **"media":** Load stylesheets asynchronously by adding `media="not x"` and removing once loaded. _[JS]_
-   **"swap":** Convert stylesheet links to preloads that swap to `rel="stylesheet"` once loaded. _[JS]_
-   **"js":** Inject an asynchronous CSS loader similar to [LoadCSS](https://github.com/filamentgroup/loadCSS) and use it to load stylesheets. _[JS]_
-   **"js-lazy":** Like `"js"`, but the stylesheet is disabled until fully loaded.

Type: (default | `"body"` \| `"media"` \| `"swap"` \| `"js"` \| `"js-lazy"`)

## Similar Libraries

There are a number of other libraries that can inline Critical CSS, each with a slightly different approach.  Here are a few great options:

-   [Critical](https://github.com/addyosmani/critical)
-   [Penthouse](https://github.com/pocketjoso/penthouse)
-   [webpack-critical](https://github.com/lukeed/webpack-critical)
-   [webpack-plugin-critical](https://github.com/nrwl/webpack-plugin-critical)
-   [html-critical-webpack-plugin](https://github.com/anthonygore/html-critical-webpack-plugin)
-   [react-snap](https://github.com/stereobooster/react-snap)

## License

[Apache 2.0](LICENSE)

This is not an official Google product.

[critical css]: https://www.smashingmagazine.com/2015/08/understanding-critical-css/

[html-webpack-plugin]: https://github.com/jantimon/html-webpack-plugin
