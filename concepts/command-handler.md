# CommandHandler

The CommandHandler namespace is the principle namespace you have to use throughout your project to access every features of the module.

{% hint style="info" %}
`CommandHandler` is a namespace because it simplifies places where you want to have all the functions/variables that the CommandHandler offers, without importing it in every command/event/other.

It also prevents you from creating/launching multiple CommandHandler from one process.
{% endhint %}

## Creating your CommandHandler

In order to initialize your CommandHandler, you have to use the `create` function with the needed options.

```javascript
const {CommandHandler} = require('advanced-command-handler');

CommandHandler.create({
    commandsDir: 'commands',
    eventsDir: 'events',
    
    // these are optionals :
    prefixes: ['!'],
    owners: ['your ID'],
    saveLogsInFile: ['logs.txt']
});
```

{% hint style="info" %}
By default `prefixes` will be `!` and the mention of your bot.

The `owners`property is only used for the `Tag.ownerOnly` tag with the default `message` event.
{% endhint %}

## Launching the CommandHandler

To launch the CommandHandler, use the `launch` method and put the token of your bot, you can also put here `clientOptions`, you can see here an example of this :

```javascript
CommandHandler.launch({
    token: 'the token of your bot',
    
    // this is optional :
    clientOptions: {
        ws: {
            intents: ['GUILDS', 'GUILD_MESSAGES']
        } 
    }
});
```

### Loading Commands & Events

The CommandHandler will automatically load and handle your commands & events, if they are all in the correct path you put.

But you can still use the `.loadXs(directory)` and `.loadX(directory, name)` functions of the CommandHandler namespace at any time \(but it's recommended to use them before launching the CommandHandler.

## [CommandHandler events](https://advanced-command-handler.github.io/docs/modules/CommandHandler.html#CommandHandlerEvents)

## [Documentation](https://advanced-command-handler.github.io/docs/modules/CommandHandler.html)

The CommandHandler class is extending the `EventEmitter` class, which means that there are events that can be hooked. Click on the title of the section to see the list of events.

