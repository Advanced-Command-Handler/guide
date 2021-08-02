# SubCommands

SubCommands are a way to have custom behavior when the first argument \(or multiple here\) is a determined string. Each commands can have SubCommands and each SubCommand should be able to have SubCommands. Here it's done differently, the name of a SubCommand can include spaces, it will slice the arguments correctly. 

## Defining SubCommands

SubCommands can be defined for commands, to register a SubCommand you have to overwrite the `registerSubCommands` method of your command class then use the `subCommand` method of the `Command` class.

Example :

{% code title="MyCommand.js" %}
```javascript
class MyCommand extends Command {
    name = 'test';
    
    registerSubCommands() {
        this.subCommand('name', (ctx) => {
            ctx.send('subCommand');
        });
    }
    
    async run(ctx) {
        ctx.send('normal command');
    }
}
```
{% endcode %}

### Passing options to your SubCommand

You can set the same options of a command to your SubCommands by passing an object before the callback :

```javascript
this.subCommand('name', 
    {
        aliases: ['other'],
        description: 'something',
        tags: ['guildOnly']
    },
    (ctx) => {
        // Code goes here     
    }
});
```

{% hint style="info" %}
Note that your SubCommands and SubCommands aliases can contains spaces in them, it will work.
{% endhint %}

{% hint style="warning" %}
If you don't put a description for your SubCommand, it will not be shown into the default help command.
{% endhint %}

## [Documentation](https://advanced-command-handler.github.io/docs/classes/Command.html#subCommand)

