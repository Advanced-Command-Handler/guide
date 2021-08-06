# Home

## What it is

This is a node.js library written in typescript for more than a year that is here to simplify bot creation with automation for commands/events handling, defaults commands/events, plus some utilities classes and functions.

#### [See on GitHub](https://github.com/Advanced-Command-Handler/Advanced-Command-Handler)

#### [Documentation](https://advanced-command-handler.github.io/docs/)

## Installation

To install this module, you need [Node.js](https://nodejs.org/).

Then execute this command :

```
$ npm install --save advanced-command-handler
```

{% hint style="info" %}
Add `@beta` at the end to install the beta version or `@rc` to install the release candidate version if any.
{% endhint %}

For the version `3.0.x` of the library you will need Discord.js v12, it will be updated to v13 in versions `3.1.x`.

## Getting started

First, [create a bot application](https://discordjs.guide/preparations/setting-up-a-bot-application.html#creating-your-bot), then get the token of the application.

Install the dependency and create your main JavaScript \(or TypeScript\) file and write the first lines to setup the bot and the Command Handler and start it.

{% code title="index.js" %}
```javascript
const {CommandHandler} = require('advanced-command-handler');

CommandHandler.create({
    commandsDir: 'commands',
    eventsDir: 'events'
}).launch({
    token: 'the token of my bot'
});
```
{% endcode %}

Then run this file using `Node.js`.

```bash
$ node index.js
```

{% hint style="success" %}
Node.js will run by default files named `index.js` so you can just run this :

```text
$ node
```
{% endhint %}

