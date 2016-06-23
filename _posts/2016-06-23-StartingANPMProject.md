---
layout: post
title: Starting A NPM Project
---

Simple notes for me on what seems to be a bare bones npm project using webpack/babel.

```bash
npm install if-env --save
npm install babel --save
npm install express --save
npm install babel-preset-es2015

# is this really needed???
npm install babel-register --save
npm install webpack
echo '{ "presets": ["es2015"] }' > .babelrc
```
You need a webpack.config.js file as well.  Depending on the purpose of the project, you may need a webpack.server.js as well.  So far I've been doing only server-side (API/GraphQL) work and that doesn't require 2 configs and 2 processes.

Here's the package.json scripts snippet:
```JSON
"scripts": {
    "start": "if-env NODE_ENV=production && npm run start:prod || npm run start:dev",
    "start:dev": "npm run build && node ./bin/server.bundle.js",
    "start:prod": "npm run build && node ./bin/server.bundle.js",
    "build:server": "webpack",
    "build": "npm run build:server"
  }
```

That assumes your webpack.config.js has this setup:

```javascript
var webpack = require('webpack');
var path = require('path');
var fs = require('fs');
var nodeModules = {};
fs.readdirSync(path.resolve(__dirname, 'node_modules'))
    .filter(function (x) {return ['.bin'].indexOf(x) === -1})
    .forEach(function (mod) { nodeModules[mod] = 'commonjs ' + mod; });
    
module.exports = {
  name: 'server',
  target: 'node',
  entry: [
    './index.js'
  ],
  output: {
    path: 'bin',
    filename: 'server.bundle.js',
    publicPath: ''
  },
  externals: nodeModules,
  resolve: {
    root: [
      path.resolve('ADD_THIS_FOLDER_TO_NODE_MODULES_PATH')
    ]
  },
  module: {
    loaders: [
      { include: /\.json$/, loaders: ["json-loader"] },
      { test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader' }
    ]
  },
  
  plugins: process.env.NODE_ENV === 'production' ? [
    new webpack.optimize.DedupePlugin(),
    new webpack.optimize.OccurrenceOrderPlugin(),
    new webpack.optimize.UglifyJsPlugin(),
    new webpack.BannerPlugin('require("source-map-support").install();',
      {
        raw: true,
        entryOnly: false
      })
  ]:[
    new webpack.BannerPlugin('require("source-map-support").install();',
    {
      raw: true,
      entryOnly: false
    })
  ],
}
```
