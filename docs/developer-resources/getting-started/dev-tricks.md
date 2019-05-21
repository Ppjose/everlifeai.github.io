# Developer Tricks and Tips

This section contains tips and tricks for developers working with the EverLife.AI software.

## Using PM2

[PM2](http://pm2.keymetrics.io/) is the process monitor used by the avatar to run it's services and skills. The PM2 runtime is bundled with the avatar but to use the `pm2` command as a tool it is convenient to have it installed globally. Use the following commmand to do that:

    pm2 install pm2 -g

### Using PM2 to monitor the logs

After installing PM2 you can use this command to monitor the logs in real time:

    pm2 logs
    
See the [PM2 documentation on log management](http://pm2.keymetrics.io/docs/usage/log-management/) for more details on how to do different kinds of log filtering etc.

### Configuring PM2 for automatic reload on code changes

PM2 also has a mode where it monitors the source of a service or skill and automatically restarts it when this source code is changed. This can drastically improve the turn-around time when developing and testing a skill or service. PM2 calls this "watch" mode.

Using the `list` command you can view the status of all processes managed by PM2 and also see if the sources are being watched at the moment.

To enable "watching" of your skill (here called `eskill-mytest`) or service use the commands below to stop it and then start it in watch mode:

    pm2 stop eskill-mytest
    pm2 start eskill-mytest --watch
    
To cancel "watching" you use the following command:

    pm2 stop eskill-mytest --watch
   

## Debugging a skill

Since skills and services are developed using JavaScript and the node.js runtime any IDE with support for node.js debugging should be able to handle breakpoints, watches etc. If you have specific information and tips for your IDE, [let us know](../../contact.md) and we will include that information here.

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/getting-started/dev-tricks.md)
