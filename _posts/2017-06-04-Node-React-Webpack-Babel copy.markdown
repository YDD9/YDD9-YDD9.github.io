---
layout: post
title:  "Node-React-Webpack-Babel"
date:   2017-06-04 11:49:39 +0100
comments: true
categories: Node Javascript React webpack Babel ES6 
---




Following the React tutorial on [tutorialpoints.com](https://www.tutorialspoint.com/reactjs/reactjs_environment_setup.htm)

Issues you may run into while following the tutorial:

webpack.config.js configuration.output.path: The provided value "./" is not an absolute path! './' relative path somehow has build error, needs to be __dirname  
module.loaders.loader must be specified as 'babel-loader' instead of 'babel'.  [further materials](https://stackoverflow.com/questions/43049748/invalid-configuration-object-in-webpack)

```
// concate path
// const path = require('path')
// path.resolve(__dirname,'folder1','file.txt')

var config = {
   entry: './main.js',
	
   output: {
      path: __dirname,  // current path,
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


After `npm start`, if there's error regarding npm version, you can update npm first, then delete node_modules folder, and install your packages again. [step explained](https://stackoverflow.com/questions/39959900/npm-start-error-with-create-react-app)
  
```
rm -rf node_modules
npm install -g npm@latest
npm install
```

[what is mount in react ?](https://stackoverflow.com/questions/31556450/what-is-mounting-in-react-js)  

[JS Bundler vs JS Task Runner](https://www.youtube.com/watch?v=wy3Pou3Vo04)  
first 14min of the video
* Bundler: Browserify, Webpack
    To compile module system(ES6, ECMAScript2015)
    To concat, minify into one file
* Task Runner: Gulp, Grunt

There's overlapping between bundler and task runner, but they are designed to more focus on their own strength.

### Webpack:
module bundling system 
platform independent
work with Node(npm)
mainly to transpile(compile) JS modules, but you need to config needed loader in webpack.config.js , i.e.:
    loader Babel for react.js

### demo:
15min - min of video

```
# make your app folder
mkdir myApp
cd myApp

# create package.json
npm init

npm install --save jquery
npm install --save-dev webpack
npm install --save-dev babel-core babel-loader babel-preset-2015

# [webpack.config.js creation](https://segmentfault.com/a/1190000004457636) and run
# 28min - 41min of video

```


[How to Uninstall Node.js from Mac OSX](http://stackabuse.com/how-to-uninstall-node-js-from-mac-osx/)  
`homebrew uninstall node` otherwise:  
Note that not all of the directories listed here may exist on your system depending on your install method.

    Delete node and/or node_modules from /usr/local/lib
    Delete node and/or node_modules from /usr/local/include
    Delete node, node-debug, and node-gyp from /usr/local/bin
    Delete .npmrc from your home directory (these are your npm settings, don't delete this if you plan on re-installing Node right away)
    Delete .npm from your home directory
    Delete .node-gyp from your home directory
    Delete .node_repl_history from your home directory
    Delete node* from /usr/local/share/man/man1/
    Delete npm* from /usr/local/share/man/man1/
    Delete node.d from /usr/local/lib/dtrace/
    Delete node from /usr/local/share/doc/
    Delete node.stp from /usr/local/share/systemtap/tapset/
    Delete node from /opt/local/bin/
    Delete node_modules from /opt/local/lib/
    Delete node from /opt/local/include/

This list should include just about all the references to Node on your system. Keep in mind there may be more. Please let me know if you find any others (and how you installed Node originally)!

Generally, don't recommand install node via homebrew, then you can avoid [error](https://gist.github.com/DanHerbert/9520689)


[Debug reactApp with VScode Steps](https://medium.com/@auchenberg/live-edit-and-debug-your-react-apps-directly-from-vs-code-without-leaving-the-editor-3da489ed905f)  
1. [download](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) VScode extension debugger for Chrome  
2. config the VScode launch.json   
            {
            "type": "chrome",
            "request": "launch",
            "name": "Launch index.html",
            "url": "http://localhost:8080",
            "webRoot": "${workspaceRoot}"
            }   
3. `npm start` start the reactApp from BASH   
4. F5 in VScode to start live debug   
