---
layout: post
title:  "Node-React-Webpack-Babel"
date:   2017-06-04 11:49:39 +0100
comments: true
categories: Node Javascript React webpack Babel
---




Following the React tutorial on tutorialpoints.com

[link](https://www.tutorialspoint.com/reactjs/reactjs_environment_setup.htm)

Issues in the tutorial:

webpack.config.js should not use './' relative path, as it may give build errors. loaders must be specified as 'babel-loader' instead of 'babel'.  [further materials](https://stackoverflow.com/questions/43049748/invalid-configuration-object-in-webpack)

```
// const path = require('path')

var config = {
   entry: './main.js',
	
   output: {
      path: __dirname,  // path.resolve(__dirname,'',''),
      filename: 'index.js',
   },
	
   devServer: {
      inline: true,
      port: 8080
   },
	
   module: {
      loaders: [
         {
            test: /\.jsx?$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
				
            query: {
               presets: ['es2015', 'react']
            }
         }
      ]
   }
}

module.exports = config;
```

babel installations:  
instal babel packages with option --save as well, save type efforts later troubleshoot.


After `npm start`, if there's version issues try to update npm first, then delete node_modules, and re-install packages. [step explained](https://stackoverflow.com/questions/39959900/npm-start-error-with-create-react-app)

```
rm -rf node_modules
npm install -g npm@latest
npm install
```