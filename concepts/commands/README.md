# Commands

Commands are the principal objects of this library, you can see here how to create a command and the default values of each options :

{% code title="ExampleCommand.js" %}
```javascript
const {Command} = require('advanced-command-handler');

module.exports = class ExampleCommand extends Command {
    name = '';
    aliases = [];
    category = '';
    description = '';
    usage = '';
    cooldown = 0;
    tags = [];
    channels = [];
    clientPermissions = [];
    userPermissions = [];

    async run(ctx) {
        // your code goes here
    }
}
```
{% endcode %}

## Example

And you can see a "complete" example of what a command could look like :

{% code title="KickCommand.js" %}
```javascript
const {Command, getThing, Tag} = require('advanced-command-handler');

module.exports = class KickCommand extends Command {
    name = 'kick';
    aliases = ['k', 'eject'];
    description = 'Kicks the person you want.';
    usage = 'kick <user> [reason]';
    cooldown = 10;
    tags = [Tag.guildOnly];
    clientPermissions =  ['KICK_MEMBERS'];
    userPermissions = ['KICK_MEMBERS'];
    
    async run(ctx) {
        const member = await getThing('member', ctx.args[0]);
        if (member) member.kick(ctx.args.slice(1).join(' '));
    }
}
```
{% endcode %}

{% hint style="danger" %}
**You have to put the command into a category folder into your commands folder like in the examples & tests.**  
 This will also automatically set the `category` property of the command. And this is why the property is not defined in this example.
{% endhint %}

## Important Notes

Do not use the `run(ctx)` command if you use a custom `message` event, use instead the `execute(ctx)` method because it will return the result of the `validate(ctx)` method that returns an error in multiple cases

### Errors from validation

| Case | The data it returns |
| :--- | :--- |
| When the user is still in a cooldown. | The remaining time to wait for the user. |
| When the command has a list of channel and is not executed in one. | Nothing |
| When the client is missing permissions defined for the command. | The missing permissions in a sorted array. |
| When the user is missing permissions defined for the command. | The missing permissions in a sorted array. |
| When the tags are not all satisfied. | The tags not satisfied. |

This is returned in an object containing the error message, the error type \(from an enum\) and the data.

## [Documentation](https://advanced-command-handler.github.io/docs/classes/command.html)

