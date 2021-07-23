# Events

Events in the directory `eventsDir` \(set with `CommandHandler.create`\) are automatically loaded, you can see an example of how to create an Event here :

{% code title="ExampleEvent.js" %}
```javascript
const {Event} = require('advanced-command-handler');

module.exports = class ExampleEvent extends Event {
    name = '';
    once = false;
    
    async run(ctx, ...eventsArguments) {
        // Your code goes here.
    }
}
```
{% endcode %}

The property `name` should be equal to the discord.js event you want to use, like `message`. The `eventsArguments` is a list of arguments that will be different with what events you use, see [Client Events](https://discord.js.org/#/docs/main/stable/class/Client).

And you can see a "complete" example of what a command could look like :

{% code title="ReadyEvent.js" %}
```javascript
const {Event, Logger} = require('advanced-command-handler');

module.exports = class ReadyEvent extends Event {
    name = 'ready';
    once = true;
    
    async run(ctx) {
        Logger.event(`Client '${ctx.client.tag}' connected !\nIt is using '${ctx.handler.version}' version.`);
    }
}
```
{% endcode %}

{% hint style="info" %}
Note that setting `once` to `true` is not needed, it will just use the `once` method for `EventEmitter` and the difference on performances is minimal.
{% endhint %}

## [Documentation](https://advanced-command-handler.github.io/docs/classes/event.html)

