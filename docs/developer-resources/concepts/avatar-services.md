# Avatar Services

Avatar services provide the mandatory core functionality for the avatar node. The services are required to run the avatar node. The difference between a service and a skill is that a skill is optional and that the avatar can function without it, more like a plugin that extends the system. All services make their API available, and intereacts with other services, through the Cote micro service library.

---

## Communication Manager `elife-commincation-mgr`

Every Everlife avatar needs to communicate with it's owner. It will use a variety of channels - telegram, messenger, web and so on. The communication manager is responsible for downloading and making available these various communication channels. Each communication channel is implemented as a separate plugin which is installed and managed by the communication manager.

### Telegram Channel `elife-telegram` 

Plugin for communication manager to communicate through Telegram.


### QWERT `elife-qwert`

Local chat client, used to communicate directly with the avtar.

---

## Skill Manager `elife-skill-mgr`

An Everlife avatar gets skills, installs, and sets them up ready for work. This code contains the core ability to do that - communicating with the work queue in order to register and obtain work.

---

## AI Service `elife-ai`

This is the brain of your avatar - it understands your chats, schedules work, and responds to you. The avatar node can use a multitude of "brains" to communicate with the user. This allows it to grow smarter and use multiple technologies playing each to it's strengths.

### AIML Brain `ebrain-aiml`

The knowledge base of the avatar. Also see this [information on how to work with the knowledge base](avatar-kb.md)

---

## SBot `elife-sbot`

Core service for avtatr to avatar/hub communication and replication.

---

## Stellar `elife-stellar`

The Stellar service manages the [avatar wallet](avatar-wallet.md) and provides services for interacting with it and the Stellar network.

---

## Level DB `elife-level-db`

Local database storage for the avatar.

---

## Work Queue `elife-work-queue`

As a successful avatar has a lot of tasks to perform, it must take care to schedule them correctly. It also must utilize resources well without overloading. For these reasons, having a work queue is essential.

---

## Package Manager `elife-pkg-mgr`

The EverLife package manager downloads and deploys everlife packages locally so they can be used. 

- - - -
[Suggest an edit for this page](https://github.com/everlifeai/everlifeai.github.io/edit/master/docs/developer-resources/concepts/avatar-services.md)
