# Developer FAQ

# Firewall can block service discovery on MacOS

The Avatar Node uses a micro service library called [Cote](https://github.com/dashersw/cote) for all internal communication. Cote uses network broadcasts to discover services. This doesnt work if the `node` application isn't allowed to receive incoming connections. 

The Avatar Node does a communication check every time it starts and will let you know if something is blocking the communication.

If you, for any reason, don't want to open the MacOS firewall to allow this you can install a local Redis instance to be used for discovery instead:

1. Install Redis. The easiest way to do this is probably to use [Homebrew](https://brew.sh/) and issue the command `brew install redis`.
2. Once installed using Homebrew Redis can be started by `brew services run redis`.
3. Before starting the Avatar set the following environment variable: `COTE_DISCOVERY_REDIS=true`

Now Cote will know to use Redis pub/sub mechanism for discovery instead of network broadcasts.

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/getting-started/dev-faq.md)
