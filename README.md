# Forked from [inline-manifest-webpack-plugin](https://github.com/szrenwei/inline-manifest-webpack-plugin) to support HtmlWebpackPlugin v.4

Inline Manifest Webpack Plugin
===================

This is a [webpack](http://webpack.github.io/) plugin that inline your manifest.js with a script tag to save http request. Cause webpack's runtime always change between every build, it's better to split the runtime code out for long-term caching.


Installation
------------
Install the plugin with npm:
```shell
$ npm i inline-manifest-webpack-plugin -D
```

Basic Usage
-----------

This plugin need to work with [webpack v4](https://github.com/webpack/webpack) (for webpack v3 and below, pls use [version 3](https://github.com/szrenwei/inline-manifest-webpack-plugin/tree/v3.0.1)) and [HtmlWebpackPlugin v3](https://www.npmjs.com/package/html-webpack-plugin) :

__Step1__: split out the runtime code
```javascript
// the default name is "runtime"
optimization: {
    runtimeChunk: 'single'
 }

// or specify another name
optimization: {
    runtimeChunk: {
        name: 'another name'
    }
 }

```
__Step2__: add plugins:
```javascript
// this plugin need to put after HtmlWebpackPlugin
[
    new HtmlWebpackPlugin(),
    new InlineManifestWebpackPlugin()
]

or

[
    new HtmlWebpackPlugin(),
    // if you changed the runtimeChunk's name, you need to sync it here
    new InlineManifestWebpackPlugin('another name')
]

```
__Done!__ This will replace the external script with inline code.

__One more thing__

if you use `inject: false` in your `HtmlWebpackPlugin`, you can access the runtime code like this:
```javascript
<%= htmlWebpackPlugin.files.runtime %>

<% for (var chunk in htmlWebpackPlugin.files.chunks) { %>
<script src="<%= htmlWebpackPlugin.files.chunks[chunk].entry %>"></script>
<% } %>
```
