# Setting up a development environment

This document assumes that you have some experience as a server side JavaScript developer. We will be talking about command line tools, process environments, Node.js and programming.  

## Requirements

Development of the avatar node or skills for the avatar can be done using any IDE or text editor that can handle JavaScript development. 

To set up a development environment the following software needs to be installed on you machine, before you start the setup:

1. [Node.js LTS version](https://nodejs.org) - JavaScript server runtime LTS (currently v10.15)
2. [Python 2.7](https://www.python.org/)

## Setup

Create a folder where you want to keep all the files related to Avatar development. If you are running the avatar already and have set up a Stellar account etc, we suggest that you keep the development instance of the avatar separate from you main avatar.

Lets call this top level directory `everlife-dev`.

In this directory you unpack the Avatar Node files, into the `elife` directory. These should be provided to you in a `-dev.tar.gz` pack.

After setting up your `everlife-dev` directory and unpacking the Avatar into `everlife-dev/elife`

    cd elife
    
And then start the Avatar:

    npm start


    HOME/everlifeai/0/ # Created when the Node is started the first time
    ├── data
    │   ├── __ssb
    │   ├── level.db
    │   └── stellar
    ├── logs
    ├── skills
        └── # Any skills you install or develop will be placed here


    everlife-dev
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
     
If you experience any problems please have a look at the sections [Developer FAQ](dev-faq.md) and [Support and Troubleshooting](dev-support.md) in this Wiki.

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/getting-started/dev-setup.md)
