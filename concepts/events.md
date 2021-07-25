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

{% code title="GuildMemberAddEvent.js" %}
```javascript
const {Event, Logger} = require('advanced-command-handler');

module.exports = class GuildMemberAddEvent extends Event {
    name = 'ready';
    
    async run(ctx, member) {
        member.user.send(`Hi ! Welcome to our guild ${member.guild.name}, we now have **${member.guild.memberCount}** members !`);
        Logger.event(`A member has joined the guild '${member.guild.name}'.`, ctx.eventName);
    }
}
```
{% endcode %}

{% hint style="info" %}
Note that setting `once` to `true` is not needed, it will just use the `once` method for `EventEmitter` and the difference on performances is minimal.
{% endhint %}

## [Documentation](https://advanced-command-handler.github.io/docs/classes/Event.html)

