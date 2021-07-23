# v2-to-v3-migration-guide

## Migration from v2 to v3

The v3 of the CommandHandler introduced a lot of new features I wanted to add for a long time, it is a complete rewrite from scratch for the commands & events part and the v2 code is completely incompatible, so this is the guide to migrate your code.

## How to install it

Library now requires at least `Node.js` v16, which is LTS. Just select the beta version from npm.

```text
npm i -s advanced-command-handler@beta
```

## Breaking changes

### AdvancedClient

**AdvancedClient\#hasPermission**

This method has been removed because of the method being not coherent with the class itself.

```diff
- client.hasPermission(message, 'SEND_MESSAGES');

+ message.guild?.me?.hasPermission('SEND_MESSAGES, {checkOwner: false, checkAdmin: false});
```

### BetterEmbed

**BetterEmbed\#checkSize**

```diff
- embed.checkSize(field?);

+ embed.throwIfTooLong(field?);
```

> Note : `checkSize` method still exists, but now just returns an object containing the fields too long.

### CommandHandler

**Methods**

Defaults functions have new names that are more coherent :

```diff
- setDefaultCommands()
- setDefaultEvents()

+ useDefaultEvents()
+ useDefaultCommands()
```

**Events**

Launch event emits now the `LaunchCommandHandlerOptions`.

```diff
- CommandHandler.on('launch', () => { //code });

+ CommandHandler.on('launch', (options /*: LaunchCommandHandlerOptions*/) => { //code });
```

### Command

Commands are created in a very different way, using now a subclass, methods for defining SubCommands, a `CommandContext` argument instead of a custom one depending on your `message` event etc.

Example :

```javascript
// before
module.exports = new Command(
    {
        name: 'ping',
        tags: ['guildOnly'],
        description = 'Get the ping of the bot.';
        userPermissions: ['MANAGE_MESSAGES'],
        category: 'utils',
    }, async (handler, message, args) => {
        const msg = await message.channel.send('Ping ?');
        const botPing = handler.client?.ws.ping;
        const apiPing = msg.createdTimestamp - message.createdTimestamp;
        await msg.edit(`Bot Latency: **${botPing}**ms\nAPI Latency: **${apiPing}**ms`);
    }
);

// now
module.exports = class PingCommand extends Command {
    name = 'ping';
    category = 'utils';
    description = 'Get the ping of the bot.';
    tags = ['guildOnly'];
    userPermissions = ['MANAGE_MESSAGES'];

    async run(ctx) {
        const msg = await ctx.reply('Ping ?');
        const botPing = ctx.client.ping; // using client.ping getter, see further down
        const apiPing = msg.createdTimestamp - ctx.message.createdTimestamp;
        await msg.edit(`WS Ping: **${botPing}**ms\nAPI Latency: **${apiPing}**ms`);
    }
}
```

So you will have to change all of your commands. This choice was taken by me, so you can define methods in your classes that will do special things, see on the [Added]() section.

### DeleteMessageOptions

`DeleteMessageOptions#options` has been removed, as a message cannot be deleted with a reason in the new Discord API.

```diff
- command.deleteMessage({message, {timeout: 1000}});

+ command.deleteMessage({message, timeout: 1000});
```

> Note  
>  You can also do this to delete the message of the command :
>
> ```javascript
> ctx.deleteMessage(1000);
> ```

### Event

It's the same thing as commands, now using classes to have improved behaviors.

Example :

```javascript
// before 
module.exports = new Event(
    {
        name: 'ready'
    },
    async (handler) => {
        console.log(`Bot is ready, username : '${handler.client?.user?.username}'`);
    }
);

// now
module.exports = class ReadyEvent extends Event {
    name = 'ready';

    async run(ctx) {
        console.log(`Bot is ready, username : '${ctx.client?.username}'`); // using client.username getter, see further down
    }
}
```

> Note: Notice that in both case, syntax is shorter and more readable.

### Logger

**Logger\#test**

```diff
- Logger.test(`debug ${setColor(LogType.test, 'message')}`);

+ Logger.debug(`debug ${setColor(LogType.debug, 'message')}`);
```

`test` has been renamed to `debug` because it's a more logic feature & name, and it was more used for debugging than testing

`LogType` is now a TS enum rather than an object, it's for having better type checking and better code.

## Added

### AdvancedClient

**Getters**

```diff
+ client.id
+ client.mention
+ client.ping
+ client.tag
+ client.username
```

These are just shortcuts of the normal code to get them.

### BetterEmbed

**BetterEmbed\#setImageFromFile**

`setImageFromFile(path)` lets you set the image of the embed from a local file.

**BetterEmbed\#setThumbnailFromFile**

`setThumbnailFromFile(path)` lets you set the thumbnail of the embed from a local file.

### Command

As you now have to extend the `Command` class to define a command, there are some included new classes with pre-defined methods, see [ImageCommand]() & [SlowCommand](), you should also do this for commands with a lot of same behavior.

Commands can now have an overwritten `registerSubCommands` method for defining SubCommands. They also have a new `subCommand` method for registering a new SubCommand.

**Command\#execute**

`execute(ctx)` is a new internal method that should not be overwritten, otherwise command will not be properly executed with SubCommands.

> Note : This method should on the other hand be used for custom `message` events.

**Command\#nameAndAliases**

`nameAndAliases` is a new getter to get an array containing the name & the aliases of the command if any.

**Command\#registerSubCommands**

`registerSubCommands()` is a new empty method than have to be overwritten for defining SubCommands.

**Command\#run**

`run(ctx)` is the method to overwrite to define the behavior of the command when executed.

**Command\#subcommand**

`subCommand(name, options?, callback)` is a new method for defining a SubCommand, it has to be used in the `registerSubcommands` method, if you register a SubCommand in the `run` method, the SubCommand will not be executed the first time. Example :

```javascript
class MyCommand extends Command {
    registerSubcommands() {
        this.subCommand('test', (ctx) => ctx.send('success !'));
    }
}
```

**Command\#subCommands**

`subCommands` is a new property containing all the SubCommands of the command.

**Command\#validate**

`validate(ctx)` is a new method to test all the other validation methods, it returns an object with the details of the error and the error data \(like message content\). It is used by the `execute` method.

### CommandContext

CommandContext is a new class used across many methods in the [Command]() class.

See on the [documentation](https://advanced-command-handler.github.io/classes/commandcontext.html) what properties & methods this class include.

### CommandHandler

**CommandHandler\#getCommandAliasesAndNames**

`CommandHandler.getCommandAliasesAndNames()` is a new function returning in one array the name & all the aliases of all commands.

**CommandHandler\#getPrefixFromMessage**

`CommandHandler.getPrefixFromMessage(message)` is a new function returning the prefix \(if found\) used in the message.

**CommandHandler\#unloadCommand**

`CommandHandler.unloadCommand(name)` is a new function, unloading the following command if found.

**CommandHandler\#unloadEvent**

`CommandHandler.unloadEvent(name)` is a new function unloading the following event and unbinding it to the client if found.

### Defaults

There is now a new default `help` command.

See their

### Event

**Event\#run**

`run(ctx)` is the method to overwrite to define the behavior of the event when fired.

### ImageCommand

`ImageCommand` is a new subclass of the class `Command` that should be used for commands returning embeds with an image or local images.

It adds `sendLocalImage(options)` & `sendImageEmbed(options)` methods, see on the [documentation](https://advanced-command-handler.github.io/classes/imagecommand.html) how to use them.

### Logger

#### LogLevel

`LogLevel` is a new TS enum to define levels of logs, so you can ignore or test Levels.

```typescript
export enum LogLevel {
    OFF = 0,
    ERROR = 1,
    WARNING = 2,
    INFO = 3,
    EVENT = 4,
    LOG = 5,
    DEBUG = 6,
    COMMENT = 7,
    ALL = 7,
}
```

**Logger\#LEVEL**

`Logger.LEVEL` is a new property to ignore some logs that are lower than the level.  
 Example :

```javascript
Logger.LEVEL = LogLevel.INFO
// this will not be logged because 'log' level is lower than 'info' level.
Logger.log("something");
// this will be logged
Logger.info("something");
```

**Logger\#ignore**

`Logger.ignore` is an array for defining ignored titles and even titles with a lower level than the one set.  
 Example :

```javascript
Logger.ignores.push('myTitle');
// This will not be logged because title is 'myTitle'
Logger.warn('this is a message', 'myTitle');

Logger.ignores.push(['something', LogLevel.LOG]);
// This will not be logged because title is 'something' and level is 'log'
Logger.log('bla bla', 'something');
// This will be logged because level is not 'log'
Logger.warn('bla bla', 'something');
```

**Logger\#savingFiles**

`Logger.savingFiles` is an array for defining in which files the logs should be emitted in, logs are appended, the file is not overwritten.

### SlowCommand

`SlowCommand` is a new subclass of the class `Command` that should be used for commands where the user has to wait.

It adds a `waitEmoji` property that is by default equals to `⌛` and the `startWait(message)` & `stopWait(message)` methods to add then remove the emoji.

### SubCommand

`SubCommand` is a new subclass of the class `Command`, it's useful for handling commands with different behavior of one command, it's not similar as arguments that are not currently handled.

SubCommand has the same field as `Command` class, except that its run function uses a `SubCommandContext` instead of a `CommandContext`.

### SubCommandContext

`SubCommandContext` is a new class extending `CommandContext`, used in `run` functions of SubCommands.

See on the [documentation](https://advanced-command-handler.github.io/docs/classes/subcommandcontext.html) what properties & methods this class include.

### Types

There are some new useful types exported.

```typescript
export type Constructor<T extends {} = {}> = new (...args: any[]) => T;
export type MaybeCommand = Constructor<Command> | {default: Constructor<Command>} | {[k: string]: Constructor<Command>};
export type MaybeEvent = Constructor<Event> | {default: Constructor<Event>} | {[k: string]: Constructor<Event>};
```

They are primarily used internally, but I think that it would be useless to export them.

