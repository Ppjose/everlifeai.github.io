# Coding a simple skill

In this guide we will be coding and adding a custom skill to our avatar. The scope of this guide is to create a custom skill for you own (non hosted) avatar. When we are done we will have extended our own avatar with some custom functionality, only available to us as the owner of the avatar. This guide will not touch on the subject of distributing a skill or selling it to others in a marketplace. 

## Prerequisites 

* Set up your avatar for development according to [the setup instructions](../../getting-started/dev-setup.md).
* You will need som basic JavaScript coding skills to be comfortable following this guide.
* A OS terminal / shell application in which you can issue commands. For MacOS or Linux you typically use the `Terminal`. On Windows you use the `Command Prompt` or you can try out the new [Ubuntu `bash` shell](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) that is included with Windows 10.

Note: We will be primarily using MacOs commands in this guide. However, it should be fairly straightforward to follow along as a Windows or Linux user.

Note: This guide is written for the EverLife Avatar version 0.9.2

## Requirements

Let's start with thinking about what we would like our new skill to do. What functionality would we like to add to our avatar? Typically a skill of this kind extends the way we communicate with our avatar, i.e. it will add a command that the avatar can react and respond to. 

For example, issuing a typical command is to type `/whoami` in any of the communication channels. This will trigger the `eskill-about` to respond with details on your avatar in the same communication channel. A skill can do anything, so that might sound a little limited but let's learn how to walk before we take on the task of writing our own home automation integration.

So, let's create a skill that can give us some more inspiration. How's that for bootstrapping yourself towards greatness? Let our requirements be that if I as an avatar owner gives the command `/inspire_me`, my avatar should give me an inspirational quote back.

## Before you begin - fasten seat belt

We should just check that your avatar setup is working as it should. Start your avatar.

    ./run-mac.sh
   
If everything is installed correctly you should now see the avatar GUI, including the "My Evelife Avatar" chat box icon on the lower left in the avatar gui. ![](avatar-gui-clean.png)

## Coding the skill

To create a skill you will need a minimum of two files and a folder.

Let's create the folder `~/everlifeai/0/skills/eskill-inspire`

    mkdir ~/everlifeai/0/skills/eskill-inspire
    
Warning: If you get an error trying to create the directory you might have a customized skill location. In that case check your directory `~/everlifeai/0` to see if there are any additional subdirectories there in which your `skills` folder is located, and use that.     
    
In that folder create one of the files:

    cd ~/everlifeai/0/skills/eskill-inspire
    touch index.js
 
### Node.js manifest file

Since every skill is a node.js module we need to set up some scaffolding for that module. The `package.json` is the manifest for your skill. This file is a standard NPM file. Use the `npm` command to add the dependencies we will need.

First navigate to the folder we created:

    cd ~/everlifeai/0/skills/eskill-inspire

The use `npm` to create the  `package.json` manifest (second file):

    npm init

This command will ask a number of questions about your project. Here you can go with all the defaults, i.e. just press enter for each question if you don't want to add any additional information. The important part is to set `index.js` as the `entry point`, which is also the default.

Now we should add our dependencies. To develop the simple skill we want to build we need to use the communications library `cote`, this is used by our skill to communicate with the other parts of the avatar architecture. We also want to use the `elife-utils` which is a library designed to help common tasks, we will use it for logging. We also need a way to call out to some web service which can provide us with an inspirational quote, for this we will use the `request` library.

So, now issue this command (still in the same folder), to register and download these dependencies:

    npm add cote request everlifeai/elife-utils

If you now look at the `package.json` in an editor it should look similar to this (dependency versions may vary):

```json
{
  "name": "eskill-inspire",
  "version": "1.0.0",
  "description": "A skill for inspiration",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cote": "^0.20.0",
    "elife-utils": "github:everlifeai/elife-utils",
    "request": "^2.88.0"
  }
}
```

### `index.js` - the code

Next, lets create and edit the `index.js` file (in the same directory) and add the code for our skill. Since what we want to build is not very complicated the actual code that does the work of getting the quote will be only a few lines. Then we need to wire it in so that our avatar can call our code, this will require some "boilerplate" code that will be very similar when you build other skills.

So, let's get to it! How can we get an inspirational quote? Lets use the <http://quotes.rest> service.

```javascript
// Require the library for making requests.
const request = require('request');

// Get a quote form the quotes service and return it using callback style. I.e. the argument `cb` is a callback function that is
// invoked once we get the response with the quote from the remote service.   
function getQuote(cb) {
    request('http://quotes.rest/qod.json', function (error, response, body) {
        if (!error && response.statusCode === 200) {
            const quote = JSON.parse(body).contents.quotes[0];
            cb(quote);
        } else {
            if (error) u.showErr(error);
            cb(null, "Sorry couldn't get a quote for you just right now.");
        }
    })
}

```

The code above retrieves a quote. Now we need to wire it up so that it is accessible for our avatar. All the modules making up the avatar communicates through microservices. The wiring includes three steps:

1. We need to publish a microservice that can receive commands from the avatar communication service.
2. We need to let the avatar communication service know that we want to receive commands.
3. When we receive the command `/inspire_me` we need to respond with a call to an existing microservice passing a quote as the message.

#### **Step 1** - publishing a service to receive commands

We use `cote` to define a new microservice that other modules/components can call. In this step the service will not do anything, we just add a "TODO" comment that will be fixed in step 3. 

```javascript
const cote = require('cote')({statusLogsEnabled:false});

// We defined a message key to identify our service uniquely.
let msKey = 'eskill-inspire';

function startMicroservice() {
    const svc = new cote.Responder({
        name: 'Everlife Inspiration Service',
        key: msKey,
    });
    // TODO: Add a handler for the service in step 3.
}
```

#### **Step 2** - letting the communications manager know we want to receive commands

We use again use `cote` to create a `Requester` that is used to call services published by other modules/components, in this case the `everlife-communication-svc` which is published by the communication manager (see [Communication Manager API documentation](../../reference/microservices.md#communication-manager)).

```javascript
const cote = require('cote')({statusLogsEnabled:false});
const u = require('elife-utils');

let msKey = 'eskill-inspire';

const commMgrClient = new cote.Requester({
    name: 'elife-inspire -> CommMgr',
    key: 'everlife-communication-svc',
});

function registerWithCommMgr() {
    commMgrClient.send({
        type: 'register-msg-handler',
        mskey: msKey,
        mstype: 'msg',
        mshelp: [
            { cmd: '/inspire_me', txt: 'Show me today`s inspirational quote.' }
        ],
    }, (err) => {
        if (err) u.showErr(err)
    });
}
```

#### **Step 3** - handle commands and respond to them

First we define a little function that can send a reply to an avatar command, calling this function will cause the message passed to be displayed to the user (us!). The second parameter is the command request which is needed in order for the communication manager to know which command was replied to.

```javascript
function sendReply(msg, req) {
    req.type = 'reply';
    req.msg = msg;
    commMgrClient.send(req, (err) => {
        if (err) u.showErr(err)
    })
}
```

Now we can extend the `startMicroservice()` function with a handler for incoming commands:

```javascript
function startMicroservice() {
    const svc = new cote.Responder({
        name: 'Everlife Inspiration Service',
        key: msKey,
    });

    // Register command handler 
    svc.on('msg', (req, cb) => {
        // Return directly if command is empty.
        if(!req.msg) return cb();
        // If command matches our expected command, respond to it.
        if(req.msg.trim() === "/inspire_me") {
            // Use the callback to let communication manager know that we recognized 
            // the command and will handle it.
            cb(null, true);
            // Call the quote service and send the reply.
            getQuote((quote, err) => {
                if (!err) {
                    sendReply(`"${quote.quote}"\n\t - ${quote.author}`, req)
                } else {
                    // If an error occurred, pass the error as the reply instead. 
                    sendReply(err, req)
                }
            })
        } else {
            // The message did not match our command.
            return cb();
        }
    })
}
```

#### Putting all the pieces together

If we put it all together and add a `main()` function that performs step 1 and 2, it should look like this:

```javascript
'use strict'
const cote = require('cote')({statusLogsEnabled:false});
const u = require('elife-utils');
const request = require('request');

function main() {
    startMicroservice();
    registerWithCommMgr();
}

let msKey = 'eskill-inspire';

const commMgrClient = new cote.Requester({
    name: 'elife-inspire -> CommMgr',
    key: 'everlife-communication-svc',
});

function sendReply(msg, req) {
    req.type = 'reply';
    req.msg = msg;
    commMgrClient.send(req, (err) => {
        if (err) u.showErr(err)
    })
}

function registerWithCommMgr() {
    commMgrClient.send({
        type: 'register-msg-handler',
        mskey: msKey,
        mstype: 'msg',
        mshelp: [
            { cmd: '/inspire_me', txt: 'Show me today`s inspirational quote.' }
        ],
    }, (err) => {
        if (err) u.showErr(err)
    });
}

function startMicroservice() {
    const svc = new cote.Responder({
        name: 'Everlife Inspiration Service',
        key: msKey,
    });

    svc.on('msg', (req, cb) => {
        if(!req.msg) return cb();
        if(req.msg.trim() === "/inspire_me") {
            cb(null, true);
            getQuote((quote, err) => {
                if (!err) {
                    sendReply(`"${quote.quote}"\n\t - ${quote.author}`, req)
                } else {
                    sendReply(err, req)
                }
            })
        }
        else {
            return cb();
        }
    })
}

function getQuote(cb) {
    request('http://quotes.rest/qod.json', function (error, response, body) {
        if (!error && response.statusCode === 200) {
            const quote = JSON.parse(body).contents.quotes[0];
            cb(quote);
        } else {
            if (error) u.showErr(error);
            cb(null, "Sorry couldn't get a quote for you just right now.");
        }
    })
}

main();
```

## Building the skill

The "building" phase means to download the required javascript dependencies and make the code ready to run together with the rest of the avatar processes. Building is done by issuing the command:

    cd ~/everlifeai/0/skills/eskill-inspire
    npm install
    
The important thing here is that you are located in the folder of the skill you want to build.

## Load the skill and test it

Once you have built the skill in the skills folder it will be automatically started when the avatar is restarted. There is also a shortcut you can use to reload the skills without restarting the avatar.

    pm2 restart elife-communication-mgr elife-skill-mgr
    
Now you should be able to issue the new command `/inspire_me` to your avatar and get a response: ![](eskill-inspire-chatbox.png)

## Testing and tweaking the skill

Development is an iterative process. You write some code, try it out and then adjust it. It is often helpful to be able to reload any changes quickly. A simple way to reload your skill without having to restart the GUI is to use this command:
    
    pm2 restart elife-communication-mgr eskill-inspire

It restarts the communication manager and our skill. This reloads all the skill code.

Note: If you change any dependencies, i.e. the `package.json` file, you will need to rebuild the skill with `npm install` before you reload it.

### Checking the logs for the skill

Install the PM2 command line tools by following [this dev trick](../../getting-started/dev-tricks.md#using-pm2). Now you can use the following command to show the logs for your specific skill only. This can be extremely helpful when doing development.

    pm2 logs eskill-inspire

## Further things to try out

1. Get your skill to display a random quote instead of "Quote of the Day" Hint: You need to get an API key from [https://theysaidso.com/api/](https://theysaidso.com/api/) to access some other rest endpoints. 
2. Add some parameters to the command to let you select a quote from the different categories available.

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/dev-guides/coding-a-skill/coding-a-skill.md)
