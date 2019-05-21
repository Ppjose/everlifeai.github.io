# Setting up a development environment

This document assumes that you have some experience as a server side JavaScript developer. We will be talking about command line tools, process environments, Node.js and programming.  

## Requirements

Development of the avatar node or skills for the avatar can be done using any IDE or text editor that can handle JavaScript development. 


To set up a development environment the following software needs to be installed on you machine, before you start the setup:

1. Node.js - JavaScript server runtime. 
2. [Yarn](https://yarnpkg.com/) - JavaScript package manager.
3. [Python 2.7](https://www.python.org/)

## Setup

Create a folder where you want to keep all the files related to Avatar development. If you are running the avatar already and have set up a Stellar account etc, we suggest that you keep the development instance of the avatar separate from you main avatar.

Lets call this top level directory `everlife-dev`.

In this directory you unpack the Avatar Node files, into the `elife` directory. You can find packaged versions of Avatar Node releases in the [repository `everlife-node-releases`](https://github.com/everlifeai/everlife-node-releases).

**Attention:** Before staring the avatar you need to set the environment variable `ELIFE_HOME` to point to the full path of the `everlife-dev`folder. Make sure this is always set when you are starting you development instance, since it controls where the `data` and `skills` folders are created/expected.

After setting up your `everlife-dev` directory and unpacking the Avatar into `everlife-dev/elife`

    cd elife
    
And then start the Avatar for the first time to initialize the bundled skills etc., choosing the start script depending on which platform you are on:

    # MacOS
    export ELIFE_HOME=/Users/myuser/myprojects/everlife-dev
    ./run-mac.sh

,

    # Linux
    export ELIFE_HOME=/Users/myuser/myprojects/everlife-dev
    ./run-linux.sh
    
or

    # Windows
    set ELIFE_HOME=C:\Users\myuser\myprojects\everlife-dev
    ./run-win.cmd
    

After going through any additional steps you are prompted to perform the resulting directory structure will look like this (as of Avatar Node version 0.8).

    everlife-dev
    ├── data        # Created when the Node is started the first time
    │   ├── __ssb
    │   ├── level.db
    │   └── stellar
    ├── elife       # Unpackaged from the Node release 
    │   ├── arch
    │   ├── helpers
    │   ├── logs
    │   ├── qwert
    │   └── services
    │       ├── elife-ai
    │       │   └── brains
    │       │       └── ebrain-aiml
    │       ├── elife-communication-mgr
    │       │   ├── channels
    │       │   │   ├── elife-qwert
    │       │   │   └── elife-telegram
    │       │   └── logs
    │       ├── elife-level-db
    │       ├── elife-sbot
    │       ├── elife-skill-mgr
    │       │   ├── logs
    │       │   └── skills              
    │       │       └── # Bundled skills are installed here
    │       └── elife-stellar
    └── skills      # Created when the Node is started the first time
        └── # Any skills you install or develop will be placed here
     
If you experience any problems please have a look at the sections [Developer FAQ](dev-faq.md) and [Support and Troubleshooting](dev-support.md) in this Wiki.

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/getting-started/dev-setup.md)
