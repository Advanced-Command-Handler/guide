# Commands Templates

As Commands are now classes, you can now easily create "templates", templates are just children of the `Command` class with added properties & methods for your different cases.

You can create templates just by creating abstract children classes of the `Command` class and use them in your code, you don't need to register them or anything. 

## Default Templates

There are 2 defaults templates.

1. `SlowCommand` : Adds `startWait(message)` and `stopWait(message)` methods that react and removes the reaction from the "wait" emoji which is defined by the `waitEmoji` property, and you can overwrite it to have a custom emoji. It is 🕰 by default.
2. `ImageCommand` : Adds `sendLocalImage(options)` for sending an image from a local path and `sendImageEmbed(options)` for sending an embed containing an image, both have also options for description and the channel where to send the message.

## Example of Usage

{% code title="MyCommand.js" %}
```javascript
const mySlowTask = require('./mySlowTask.js');

module.exports = class MyCommand extends SlowCommand {
    name = 'test';
    
    async run(ctx) {
        const message = ctx.send("Please wait a bit while I'm performing the task.");
        await this.startWait(message);
        await ctx.deleteMessage();
        
        const result = await mySlowTask();
        await this.stopWait(message);
        await message.edit(result);
    }
}
```
{% endcode %}

