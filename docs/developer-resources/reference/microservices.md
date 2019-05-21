# Micro services APIs

Note: This section is currently being expanded and is not yet complete. The information relates to avatar node version 0.8.

All the avatar components, service, skills and plugins for services use the [Cote microservice library](http://cote.js.org/) to communicate. Cote handles service discovery and message passing.

Cote uses a requester/responder model where communication is made up of (JSON) messages being passed between requester and responder endpoints. To partition communication a different `key` is used for each service. To distinguish between different messages a `type` attribute is used, included in each message.

# Communication Manager

Key: `everlife-communication-svc`

## Messages

### Add a channel

    {
        type: "add-channel"
        pkg: <string> - Repository to install channel from.
    }


### Register a message handler

    {
        type: "register-msg-handler"
    }
    
### Handle a message from owner

    {
        type: "message"
        msg: <string> - The message.
    }    
   
**Returns:** An error message if the request could not be handled.
**Response:** The avatar response is sent as an async message to the active communication channel. 

### Handle a message from another user/avatar (not the owner)

    {
        type: "not-owner-message"
        msg: <string> - The message.
    }
  
### Handle reply from avatar

    {
        type: "reply"
        msg: <string> - Message from avatar to user.
    }  
  
**Returns:** An error message if the request could not be handled.
   
# Skill Manager

Key: `everlife-skill-svc`

### Install skill package

    {
        type: "msg"
        msg: <string> - Command for skill manager, format `/install <skill package>`
    }
    
### Add skill service

    {
        type: "add"
        pkg: <string> - Package name.
    }

# Everlife AI Service

Key: `everlife-ai-svc`

# Everlife AIML Brain

Key: `ebrain-aiml`

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/reference/microservices.md)
