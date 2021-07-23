# Default Commands & Events

## Defaults Events

There is for now only one Default Event which is the `message` event. To use it, simply execute the [`useDefaultEvents`](https://advanced-command-handler.github.io/docs/modules/commandhandler.html#setdefaultevents) function from the CommandHandler Namespace.

## Defaults Commands

There is for now only two default Command which are the `help` and `ping` command. To use it, simply execute the [`useDefaultCommands`](https://advanced-command-handler.github.io/docs/modules/commandhandler.html#setdefaultcommands) function from the CommandHandler Namespace.

{% hint style="danger" %}
Both must be used after the `create` function.
{% endhint %}

## Example

{% code title="index.js" %}
```javascript
const {CommandHandler} = require('advanced-command-handler');

CommandHandler
.create({
    commandsDir: 'commands',
    eventsDir: 'events',
})
.useDefaultEvents()
.useDefaultCommands()
.launch({
    token: 'token goes here',
});
```
{% endcode %}

