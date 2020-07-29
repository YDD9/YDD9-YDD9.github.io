---
layout: post
title:  "Nodejs from zero"
date:   2020-07-25 21:49:39 +0100
comments: true  
categories: web
---

`Introduction to NodeJS` on edX from Microsoft:DEV283x

# Table of contents
1. [Install Nodejs in Ubuntu](#installnodejsinubuntu)
2. [Start and exit node from console](#startandexitnodefromconsole)
3. [Install MongoDB in Ubuntu](#installmongodbinubuntu)
4. [Start MongoDB](#startmongodb)
5. [Configuring npm](#configuringnpm)
6. [require and module.exports](#requireandmodule.exports)
7. [Core module fs](#coremodulefs)
8. [Understand Event Emitters](#understandeventemitters)
9. [HTTP Client with Core http](#httpclientwithcorehttp)
10. [HTTP server with core http](#httpserverwithcorehttp)
11. [Introduction to npm](#introductiontonpm)
12. [Package.json](#package.json)
13. [Callbacks concept](#callbacksconcept)

## Install Nodejs in Ubuntu <a name="installnodejsinubuntu"></a>
For Ubuntu16.04.x, use snap to install and update: https://github.com/nodesource/distributions/blob/master/README.md#snapinstall
```
$ sudo snap install node --classic --channel=8    
$ sudo snap refresh node --channel=10  
```   

Substituting `8` for the major version you want to install. Both LTS and Current versions of Node.js are available via snapcraft.

The `--classic` argument is required here as Node.js needs full access to your system in order to be useful, therefore it needs snap's "classic confinement". 

You can use the `refresh` command to switch to a new channel at any time. Once switched, snapd will update Node.js for the new channel you have selected.

## Start and exit node from console <a name="startandexitnodefromconsole"></a>
It's also called Node.js REPL where 9+23 will be evaluated as addition and print out the result, and function can be defined too.
```
$ node
> 9+23
32
> let f = ()={return 1}
undefined
>f()
1
> process.exit()
$
$ node -e "console.log(process.versions)"
{
  node: '14.4.0',
  v8: '8.1.307.31-node.33',
  uv: '1.37.0',
  zlib: '1.2.11',
  brotli: '1.0.7',
  ares: '1.16.0',
  modules: '83',
  nghttp2: '1.41.0',
  napi: '6',
  llhttp: '2.0.4',
  openssl: '1.1.1g',
  cldr: '37.0',
  icu: '67.1',
  tz: '2019c',
  unicode: '13.0'
}
```
Or simpler just type `.exit` or type `ctrl + c` twice.

## Install MongoDB in Ubuntu <a name="installmongodbinubuntu"></a>
install instruction for latest MongoDB https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition
```
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

## Start MongoDB <a name="startmongodb"></a>
Directories
    If you installed via the package manager, the data directory `/var/lib/mongodb` and the log directory `/var/log/mongodb` are created during the installation.

    By default, MongoDB runs using the `mongodb` user account. If you change the user that runs the MongoDB process, you must also modify the permission to the data and log directories to give this user access to these directories.

Configuration File
    The official MongoDB package includes a configuration file (`/etc/mongod.conf`). These settings (such as the data directory and log directory specifications) take effect upon startup. That is, if you change the configuration file while the MongoDB instance is running, you must restart the instance for the changes to take effect.

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#run-mongodb-community-edition
```
$ sudo systemctl start mongod
$ mongo
> exit
bye
$
```

## Configuring npm <a name="configuringnpm"></a>
Locate the path to npm's directory. It might differ depending on the OS. Execute the command below to find your path:
```
npm config get prefix
```
For many POSIX systems, this will be /usr/local.
DANGER: If the path is just /usr, change the default folder to a new one as described in the npm documentation.

Change the owner of npm's directories to the name of the current user (your username!):
```
sudo mkdir -p $(npm config get prefix)/{lib/node_modules,bin,share}
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}

npm i -g node-static
static -h
```
This changes the permissions of the sub-folders used by npm and some other tools (lib/node_modules, bin, and share).

## require and module.exports <a name="requireandmodule.exports"></a>
Node provides a built-in module mechanism which works with the `require()` method and the `module.exports` global object. To demonstrate how `require` and `module.exports` work, let's say we have two files `account-service.js` and `utility.js`.

The `utility.js` has some generic methods and objects which we use in many projects and applications. In this example, we will import those generic methods into `account-service.js`.

Here's the code of `utility.js` in which we expose code to `account-service.js` (or any other program) by assigning it to a special global `module.exports`:
```
module.exports = function(numbersToSum) {
  let sum = 0, 
    i = 0, 
    l = numbersToSum.length;
    while (i < l) {
        sum += numbersToSum[i++]
    }
    return sum
}
```
The main program (account-service.js) imports the utility module and executes it to find out the total balance:
```
const sum = require('./utility.js')

let checkingAccountBalance = 200
let savingsAccountBalance = 1000
let retirementAccountBalance = 20000

let totalBalance=sum([checkingAccountBalance, savingsAccountBalance, retirementAccountBalance] )
console.log(totalBalance)
```
The `account-service.js` can be run from the same folder where the file is located with node `account-service.js`. The code will import the `utility.js` as `const sum` and invoke `sum()`. Thus, the result will be output of the total balance.

Using `require()` with local files
To use `require()` with local files, specify the name string (the argument to `require()`) of the file you are trying to import. In general, start the name string with a . to specify that the file path is relative to the current folder of the node.js file or a .. to specify that the file path is relative to the parent directory of the current folder. For example, `const server = require('./boot/server.js')` imports a file named server.js which is in a folder named boot that is in the current folder relative to the code file in which we write `require()`.

Using `require()` with npm or core modules/packages
To use `require()` with an npm or core module/package, enter the module/package name as the name string. There should not be . or .. in the name string. For example, `const express = require('express')` imports a package named express. The package is in the node_modules folder in the root of the project if it's an installed npm package, and in the system folder if it's a core Node module (exact location depends on your OS and how you installed Node).

## Core module fs <a name="coremodulefs"></a>
* fs.readFile(): reads files asynchronously
* fs.writeFile(): writes data to files asynchronously
Reading from files is done via the core fs module. There are two sets of reading methods: asynchronous (recommended) and synchronous. In most cases, developers should use async methods, such as fs.readFile because this method won't block the event loop:
```
const fs = require('fs')
const path = require('path')
fs.readFile(path.join(__dirname, '/data/customers.csv'), {encoding: 'utf-8'}, function (error, data) {
  if (error) return console.error(error)
  console.log(data)
})
```
To write to the file, execute the following:
```
const fs = require('fs')
fs.writeFile('message.txt', 'Hello World!', function (error) {
  if (error) return console.error(error)
  console.log('Writing is done.')
})
```
The path.join() method is used to create paths that are platform independent. On Windows paths are separated using a \, while on POSIX (Unix, macOS) paths are separated by a /. You can combine path.join with __dirname (equals to path.dirname())to use an absolute path instead of a relative one. From /Users/mjr executing `node example.js`.
```
console.log(__dirname);
// print: /Users/mjr
console.log(path.dirname(__filename));
// print: /Users/mjr
```

# Understand Event Emitters <a name="understandeventemitters"></a>
Node.js core API is based on asynchronous event-driven architecture in which certain kind of objects called emitters periodically emit events that cause listener objects to be called. Event emitters is a core module for Node developers to implement the observer pattern. The observer pattern has the following: an observer/listener, an event and an event emitter.

The flow goes like this:

  - A class is created with class
  - A class inherits from the EventEmitter class using extends
  - An instance of an object is created from the class with new
  - An observer (a.k.a. event listener) is created with .on(eventName, eventHandler)
  - An event is emitted with emit() and the event handler in the observer is executed

simple.js:
```
const EventEmitter = require('events')

class Job extends EventEmitter {}
job = new Job()

job.on('done', function(timeDone){
  console.log('Job was pronounced done at', timeDone)
})

job.emit('done', new Date())
job.removeAllListeners()  // remove  all observers
```
The result will be:
`Job was pronounced done at xxxx
`
Multiple Event Triggers
Events can be triggered/emitted multiple times. For example, in knock-knock.js the knock event is emitted multiple times.

knock-knock.js:
```
const EventEmitter = require('events')

class Emitter extends EventEmitter {}
emitter = new Emitter()

emitter.on('knock', function() {
  console.log('Who\'s there?')
})

emitter.on('knock', function() {
  console.log('Go away!')
})

emitter.emit('knock')
emitter.emit('knock')
```
The result will be:
```
Who's there?
Go away!
Who's there?
Go away!
```
Executing Observer Code Only once

`emitter.once(eventName, eventHandler)` will execute observer code just once, no matter how many time this particular event was triggered.

knock-knock-once.js:
```
const EventEmitter = require('events')

class Emitter extends EventEmitter {}
emitter = new Emitter()

emitter.once('knock', function() {
  console.log('Who\'s there?')
})


emitter.emit('knock')
emitter.emit('knock')
```
The result will be:

`Who's there?`

Modular Events
The observer pattern is often used to modularize code. A typical usage is to separate the event emitter class definition and the event emission into its own module but allow the observers to be defined in the main program. This allows us to customize the module behavior without changing the module code itself.

In job.js, we use a generic job module that emits a done event after 700ms. However, in weekly.js, we can customize the event handler of the observer to do whatever we want once a done event is triggered.

job.js:
```
const EventEmitter = require('events')
class Job extends EventEmitter {
  # constructor is given by es6(ECMAScript6 module) module, it is similar as __init__ in class definition in Python
  constructor(ops) {
    super(ops)
    this.on('start', ()=>{ #(3)
      this.process() # event listener will call process()
    })
  }
  # after define the variables for class, now start to code the methods in the class. process.setTimeout is given in Nodejs
  process() {
     setTimeout(()=>{
      // Emulate the delay of the job - async!
      this.emit('done', { completedOn: new Date() }) $(4)
    }, 700)
  }
}

module.exports = Job
```
weekly.js:
```
var Job = require('./job.js')
var job = new Job()

job.on('done', function(details){
  console.log('Weekly email job was completed at', #(2)
    details.completedOn)
})

job.emit('start') #(1)
```
result of `node weekly.js` has 700ms delay after execution:

`Weekly email job was completed at 2017-10-27T18:31:25.503z
`
When you run weekly.js, the custom logic pertaining to the done event will be executed from weekly.js. This way the creators of the job.js module can keep the module flexible. They don't have to hard code any logic for the done event in the module. Consumers of the module job.js, people who write weekly.js, have the power to customize the behavior for the done event, and not only for that event. Event emitters can have multiple events: in the middle, at the start, in the end, etc. They can be called (emitted or triggered) multiple times and with data (passed as the second argument to emit() as can be seen in job.js). Furthermore, there are methods to list or remove event listeners (observers) or to specify the execution of an event listener (observer) just once (.once() method).

## HTTP Client with Core http <a name="httpclientwithcorehttp"></a>
In web development we often want to make HTTP requests to other services, and Node provides a core module http to make such requests. This module uses the event emitter pattern. The idea is that you get a small chunk of the overall response (usually a single line of the overall payload/body) during each data event. You can process the data right away (preferred for large data) or save it all together in a buffer variable for future use once all the data has been received (preferred for JSON).


Take a look at the example in http-get-no-buff.js in which each new line (chunk) of the response is printed back to the terminal with console.log:

http-get-no-buff.js:
```
const http = require('https')
const url = 'http://nodeprogram.com'
http.get(url, (response) => {
  let c = 0
  response.on('data', (chunk) => { 
    c++
    console.log(chunk.toString('utf8'))
  })
  response.on('end', () => {
    console.log(`response has ended with ${c} chunk(s)`)
  })
}).on('error', (error) => {
  console.error(`Got error: ${error.message}`)
})
```
The result:
```
$ node http-get-no-buff.js
<html>
...
...
</html>
response has ended with 6 chunk(s)
```
The result of running this script will be the home page HTML from http://nodeprogram.com. It might be hard to notice with a naked eye, but the result will be printed as the request is happening, not all at once in the end of the request.

If you want to wait for the entire response, simply create a new variable as a buffer variable (rawData) and save the chunks (parts of the response, usually lines in the payload/body) into it (rawData).


HTTP Client for JSON
To process JSON, developers must get the entire response, otherwise the JSON format won't be valid. For this reason, we use the buffer variable rawData. When the response has ended, we parse the JSON. Take a look at the code in http-json-get.js:
```
const https = require('https')
const url = 'https://gist.githubusercontent.com/azat-co/a3b93807d89fd5f98ba7829f0557e266/raw/43adc16c256ec52264c2d0bc0251369faf02a3e2/gistfile1.txt'
https.get(url, (response) => {
  let rawData = ''
  let c = 0
  response.on('data', (chunk) => { 
    rawData += chunk 
    ++c
  })
  response.on('end', () => {
    try {
      const parsedData = JSON.parse(rawData)
      console.log(parsedData, c)
    } catch (e) {
      console.error(e.message) # error such as json parsing
    }
  })
  response.on('error', (error)=>{
    console.error('second error', error) # error such as 400 or 404 
  })
}).on('error', (error) => {
  console.error(`Got error: ${error.message}`) # error such as wrong url, port close or no connection, error on the https.get level
})
```
The result of running this script would be the parsed JSON object. The parsing needs to happen inside of the try/catch to handle any failures that may occur due to mal-formated JSON input.

HTTP client POST request
So far we have been using GET requests to receive data from a server. GET requests are able to be used to receive data but you can not send data with a GET request. In order to send a body of data with a request you must use a POST request. POST requests are generally used to upload data or to send data to be processed and returned.

The http core module methods allow you to specify what type of request you want to make. To do so, first create an options object and set the method attribute to the desired request type ('POST', 'GET', etc.). Then, use the options object as the first argument when calling http.request().

The following code from http-post.js uses an options object to specify that it is trying to make a POST request:
```
const http = require('https')
const postData = JSON.stringify({ foo: 'bar' })

const options = {
  hostname: 'mockbin.com',
  port: 80,
  path: '/request?foo=bar&foo=baz',
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
    'Content-Length': Buffer.byteLength(postData)
  }
}

const req = http.request(options, (res) => {
  res.on('data', (chunk) => {
    console.log(`BODY: ${chunk}`)
  })
  res.on('end', () => {
    console.log('No more data in response.')
  })
})

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`)
})

req.write(postData) # start the post request
req.end()           # end the post request
```
As a result, the script will send the data to the server (mockbin.com) in a POST request, and output the response of the request.

## HTTP server with core http <a name="httpserverwithcorehttp"></a>
Although Node.js can be used for a wide variety of tasks, it's used primarily for building web applications. Node.js thrives in networking as a result of its asynchronous nature and built-in modules such as net and http. Node is great for building fast and efficient web servers.

Here's a quintessential Hello World example in which we create a server object, define the request handler (function with req and res arguments), pass some data back to the recipient, and start up the whole thing (server.js):
```
const http = require('http')
const port = 3000
http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'})
  res.end('Hello World\n')
}).listen(port)

console.log(`Server running at http://localhost:${port}/`)
```
Let's break it down a bit. The following loads the core http module for the server:
```
const http = require('http')
```
This snippet below creates a server with a callback function which contains the response handler code:
```
http.createServer((req, res) => {
```
To set the right header and status code, use the following:
```
  res.writeHead(200, {'Content-Type': 'text/plain'})
```
To output Hello World with the line end symbol, use:
```
  res.end('Hello World\n')
```
The req and res arguments have all the information about a given HTTP request and response respectively. In addition, req and res can be used as streams (see previous section).

To make the server accept requests, use the following:
```
}).listen(3000)
```
In the terminal, navigate to the directory in which you have server.js and run the following command to start your server:
```
node server.js
```
Now that the server is running, you should be able to navigate to localhost:3000 in a browser and you should see Hello World. To shut down the server, enter Ctrl + c in the terminal.

The callback function in the createServer method is called each time there's an incoming request to this server.

You can also use curl to get the response from the server in the terminal. To do so, keep the terminal / command prompt window open to keep the server running. In another tab or window of your terminal / command prompt, run the following curl request:
```
curl -i http://localhost:3000
```
The curl request should return the header and content that is sent from the server.

To have the server automatically restart and take the changes immediately, just install `npm i -g node-dev@latest` and start server `node-dev server.js`

HTTP Server Request
The HTTP server request object (do not confuse this with the client request object) has all the information about the incoming request to our server. Some examples include headers, URL, HTTP method names and of course the request body (payload). Here's the list of main properties:

  * request.headers: Information about incoming requests headers such as Connection, Host, Authorization, etc (see list here)
  * request.method: Information about the incoming requests methods such as GET, POST, PUT, DELETE, OPTIONS, HEAD, etc.
  * request.url: Information about the incoming request URL, such as /accounts, /users, /messages, etc.
All values are accessible in the request handler callback. For example, you can print the values like this:
```
const http = require('http')
const port = 3000
http.createServer((request, response) => {
  console.log(request.headers)
  console.log(request.method)
  console.log(request.url)
  response.writeHead(200, {'Content-Type': 'text/plain'})
  response.end('Hello World\n')
}).listen(port)
```
The result of this script will depend on what requests are coming. Each request will trigger the output of its headers, method, and the URL.

Processing Incoming Request Body in the Server
To process the request body, use the same event emitter pattern as with the request client. Listen to the data event and collect the incoming payload using a buffer variable (buff). Take a look at server-request-body.js:
```
const http = require('http')
const port = 3000
http.createServer((request, response) => {
  console.log(request.headers)
  console.log(request.method)
  console.log(request.statusCode)
  console.log(request.url)
  if (request.method == 'POST') {
    let buff = ''
    request.on('data', function (chunk) {
      buff += chunk  
    })
    request.on('end', function () {
      console.log(`Body: ${buff}`)
      response.end('\nAccepted body\n\n')
    })
  } else {
    response.writeHead(200, {'Content-Type': 'text/plain'})
    response.end('Hello World\n')
  } 
}).listen(port)
```
The result of this program (server-request-body.js) would be a server which accepts POST requests and prints the request body in the server logs.

HTTP Response
The HTTP response is what enables us to send data back to the clients from our Node servers.

response.writeHead is a method that is used to set the status code and create HTTP headers. Two most common HTTP headers are Content-Type and Content-Length:
```
response.writeHead(200, {
  'Content-Length': body.length,
  'Content-Type': 'text/plain' })
```
The response itself is created with the write() method which adds data to the response. Another method is end() which finishes the response (which in turn will finish the incoming request). You can set the statusCode attribute to change the status code of the response (200, 400, 500, etc.).

The following example demonstrates the various response methods:
```
const http = require('http')
const port = 3000
http.createServer((request, response) => {
  response.writeHead(404, {
    'Content-Length': body.length,
    'Content-Type': 'text/plain' })
  response.statusCode = 200
  response.write('Hello')
  response.end(' World\n')
}).listen(port)

console.log(`Server running at http://localhost:${port}/`)
```

## Introduction to npm <a name="introductiontonpm"></a>
npm can be configured in multiple ways:

  * flags
  * environment variables
  * .npmrc files
  * npm config CLI
The npm config CLI is the easiest way, so let's cover it and take a look at a few examples.

To list current configs:
```
npm config list 
npm config ls // list configs
```
To list global configs:
```
npm config --global list 
npm config -g ls
```
There are many configurations. For example proxy or registry are the most common ones especially if you are working at a big company that has a corporate proxy and a private (self-hosted) npm registry.

To set any config use use npm config set <key> <value>. For example, to set the registry value use:
```
npm config set registry "http://registry.npmjs.org/"
```
You can read an individual setting value. For example to read the registry value use npm config get registry:
```
npm config get registry
```
To remove the setting (config), there's a npm config delete <key> command. For example, to remove email:
```
npm config delete email
```

There are two ways to install a module via npm:

1) Locally: most of your projects' dependencies which you import with require(), e.g., express, request, hapi. They go into the node_modules directory of your local project
```
npm install module-name
npm i module-name
```
2) Globally: command-line tools only (mostly), e.g., mocha, grunt, slc. They go into /usr/local
```
npm install --global module-name
npm i -g module-name
```
The i is just an alias to install. There's no difference. Use i to save time typing.

Some frameworks offer CLI, but most of them belong to the local category. Don't try to install express with -g!

The node_modules folder is where dependencies are stored. It's a local folder which must be in the root (first level sub-folder) of your project. node_modules is your friend because it allows for almost no conflicts between different versions of the same dependencies unlike Java, Ruby, Python which prefer global installation over Node's local. Node reduces conflicts because each conflicting dependency will be nested and this will avoid conflicts between different versions of the same dependencies.

Installing Packages
Here are the valid ways in which a Node developer can install an npm module.

Basic installation:
```
npm install express
```
Exact version installation:
```
npm install express@4.2.0
```
Latest version installation, which can be useful when you already have this module but want to upgrade to the latest module:
```
npm install express@latest
```
Explicit save into into package.json dependencies (--save or -S) or devDependencies (--save-dev or -D):
```
npm install express --save
npm install express -S
npm install mocha --save-dev
npm install mocha -D
```
In npm version 5, npm will automatically save so npm i express will be the same as npm i -S express. We recommend using the default behavior of npm version 5 which is to save package information into package.json.

By default, npm will add ^ to the version when you use npm v5 or --save. The ^ symbol is dangerous for applications because it means go get the latest version if there's one. It's best to avoid ^. Using the exact flag will do just that:
```
npm install express --exact
npm install express -E
```
You can combine flags and install more than one dependency in one command:
```
npm i react react-dom babel babel-core -ED
```
Lastly, when you will need to install a tool like npm itself (or upgrade it) you will use the global installation:
```
npm i -g npm@latest
npm install grunt --global
```
If you see an error about permissions, you'll need to change the system folder which npm uses to the appropriate permissions or just use root access with sudo:
```
sudo npm install grunt -g
```
Semantic versioning consists of using three digits which have certain meaning. For example, in semver 4.2.0, 4 is major, 2 is minor and 0 is patch. Major is for major releases which most often break existing code. Minor are for small releases which can break some code but most often are okay. Patch is for small fixes which should not change the main interface and should not break your applications.

The key word here is should because semantic versioning is not enforced. It's purely a human convention and not all modules and projects in the FOSS follow it.

Listing and Removing Modules
To list what modules are installed, run npm ls from your root project location (where you have package.json and node_modules). It will display a tree of dependencies of this current project.

To list all globally installed modules, run `npm ls -g`.

To remove an npm module use the rm command:
```
npm rm mysql
```
To remove a global module, apply the global flag:
```
npm rm mysql -g
```

## Package.json <a name="package.json"></a>
The package.json is the project manifest file. It has all the meta data about the project such as the descriptions, license, location, dependencies, scripts to build, launch and run. Consider this example which has a few dependencies:
```
{
  "name": "my-cool-app",
  "version": "0.1.0",
  "description": "A great new application",
  "main": "server.js",
  "dependencies": {
    "express": "~4.2.0",
    "ws": "~0.4.25"
  },
  "devDependencies": {
    "grunt": "~0.4.0"
  }
}
```
In most cases, it's easy to tell what modules are required and what are the main commands and files to execute just by looking at the package.json file.

Package.json is required for npm modules.

Main Properties
Module packaging in Node is done using a package.json file. There are many options that can be configured:

  * name
  * version number
  * dependencies
  * license
  * scripts
  * etc

Creating package.json
To create a package.json file, run npm init command and answer the questions that appear:
```
$ npm init

This utility will walk you through creating a package.json
file.  It only covers the most common items, and tries to
guess sane defaults.

See `npm help json` for definitive documentation on these
fields and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package
and save it as a dependency in the package.json file

Press ^C at any time to quit
name: (my-package-name)
```
If you are okay with the default answers to these questions, then you can skip the questions and answer yes to all of them automatically by using -y flag, as in `npm init -y`.

Private Modules
The private attribute prevents accidental publishing
```
{
  "name" : "my-private-module",
  "version": "0.0.1",
  ...
  "private": true,
  ...
}
```
When to use -g for global installations?
Only use -g for command-line tools which you run from the Terminal /Command Prompt. They usually have bin in package.json:
```
{
  "name": "stream-adventure",
  "version": "4.0.4",
  "description": "an educational stream adventure",
  "bin": {
    "stream-adventure": "bin/cmd.js"
  },
  "dependencies": {
    ...
```
In other words, anything which you plan to import with require() must be local in node_modules NOT in global.

## Callbacks concept <a name="callbacksconcept"></a>
Callback is an asynchronous equivalent for a function. A callback function is called at the completion of a given task. Node makes heavy use of callbacks. All the APIs of Node are written in such a way that they support callbacks.

For example, a function to read a file may start reading file and return the control to the execution environment immediately so that the next instruction can be executed. Once file I/O is complete, it will call the callback function while passing the callback function, the content of the file as a parameter. So there is no blocking or wait for File I/O. This makes Node.js highly scalable, as it can process a high number of requests without waiting for any function to return results.

Create a text file named input.txt with the following content:
```
Tutorials Point is giving self learning content
to teach the world in simple and easy way!!!!!
```
Blocking Code Example
```
var fs = require("fs");
var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("Program Ended");
```
Now run `node main.js` to see the result output:
```
Tutorials Point is giving self learning content
to teach the world in simple and easy way!!!!!
Program Ended
```
Non-Blocking Code Example
Create a text file named input.txt with the following content.
```
Tutorials Point is giving self learning content
to teach the world in simple and easy way!!!!!
```
Update main.js to have the following code:
```
var fs = require("fs");
# callback functino(err, data) https://nodejs.org/api/fs.html#fs_fs_readfile_path_options_callback
fs.readFile('input.txt', function (err, data) {
   if (err) return console.error(err);
   console.log(data.toString());
});

console.log("Program Ended");
```
Now run the main.js to see the result:
```
$ node main.js
Verify the Output.

Program Ended
Tutorials Point is giving self learning content
to teach the world in simple and easy way!!!!!
These two examples explain the concept of blocking and non-blocking calls.
```
The first example shows that the program blocks until it reads the file and then only it proceeds to end the program.

The second example shows that the program does not wait for file reading and proceeds to print "Program Ended" and at the same time, the program without blocking continues reading the file.

Thus, a blocking program executes very much in sequence. From the programming point of view, it is easier to implement the logic but non-blocking programs do not execute in sequence. In case a program needs to use any data to be processed, it should be kept within the same block to make it sequential execution.