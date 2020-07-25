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
6. [require and module.exports](#requireandmoduleexports)



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
