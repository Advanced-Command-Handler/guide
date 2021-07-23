# CommandContext

The CommandContext is a class used in the  `run` method of the commands to include all the properties you need, it also includes a lot of getters and useful methods to shorten your code.

```javascript
// from a command
async run(ctx) {
    ctx.send(`${ctx.commandName} executed by ${ctx.user} in ${ctx.channel} !`);
    ctx.react('👍');
    ctx.send('I will now destroy your message in 10 seconds...');
    ctx.deleteMessage(10000);
}
```

## [SubCommandContext](https://advanced-command-handler.github.io/docs/classes/SubCommandContext.html)

From within [SubCommands](subcommands.md) the `ctx` argument in the callback is an instance of the `SubCommandContext` class which includes some other fields related to the SubCommand itself.

## [Documentation](https://advanced-command-handler.github.io/docs/classes/CommandContext.html)

