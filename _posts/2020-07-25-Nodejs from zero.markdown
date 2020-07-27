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

## Install Nodejs in Ubuntu <a name="installnodejsinubuntu"></a>
For Ubuntu16.04.x, use snap to install and update: https://github.com/nodesource/distributions/blob/master/README.md#snapinstall
```
$ sudo snap install node --classic --channel=8    
$ sudo snap refresh node --channel=10  
```   

Substituting `8` for the major version you want to install. Both LTS and Current versions of Node.js are available via snapcraft.

The `--classic` argument is required here as Node.js needs full access to your system in order to be useful, therefore it needs snap’s "classic confinement". 

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
