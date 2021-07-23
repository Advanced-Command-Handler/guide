# Logger

`Logger` is a class in the `utils` folder to help you logging things. It has multiple **static** methods that you can use.

## Example

```javascript
const {Logger} = require('advanced-command-handler');
Logger.error(`${Logger.setColor('orange')} is not allowed.`, 'PermissionError');
```

Give the following result in the console \(screen made on `WebStorm`\).  
 ![screen](https://i.ibb.co/XbvYDH0/image.png)

**Every number is yellow by default.**

## Properties

### Colors

Colors are set out in a static public object in the class, so you can change them.

These are the current colors :

{% code title="Logger.ts" %}
```javascript
const colors = {
    red: '#b52825',
    orange: '#e76a1f',
    gold: '#deae17',
    yellow: '#eeee23',
    green: '#3ecc2d',
    teal: '#11cc93',
    blue: '#2582ff',
    indigo: '#524cd9',
    violet: '#7d31cc',
    magenta: '#b154cf',
    pink: '#d070a0',
    brown: '#502f1e',
    black: '#000000',
    grey: '#6e6f77',
    white: '#ffffff',
    default: '#cccccc',
};
```
{% endcode %}

### Levels

Each logs have levels that are primarily used for ignoring logs, see [Ignoring logs](logger.md#ignoring-logs).

{% code title="Logger.ts" %}
```javascript
enum LogLevel {
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
{% endcode %}

## Ignoring logs

There are multiple way to ignore logs.

You can ignore whole multiple levels of logs like this :

```javascript
Logger.LEVEL = LogLevel.INFO
// this will not be logged because 'log' level is lower than 'info' level.
Logger.log('something');
// this will be logged
Logger.info('something');
```

You can also ignore logs by titles using `ignores` property, the **title** is the second argument you put in every logs methods, it will always be defaulted to the name of the log level.

```javascript
Logger.ignores.push('myTitle');
// This will not be logged because title is 'myTitle'
Logger.warn('this is a message', 'myTitle');
```

Finally you can ignore logs by titles and by levels.

```javascript
Logger.ignores.push(['something', LogLevel.LOG]);
// This will not be logged because title is 'something' and level is 'log'
Logger.log('bla bla', 'something');
// This will be logged because level is not 'log'
Logger.warn('bla bla', 'something');

// You can also use strings for defining the name of the level
Logger.ignores.push(['something', 'COMMENT']);
```

## Saving logs

You can add files where to save logs using the `CreateCommandHandlerOptions#savingFiles` property when creating your CommandHandler.

```javascript
const {CommandHandler} = require('advanced-command-handler');

CommandHandler.create({
    commandsDir: 'commands',
    eventsDir: 'events',
    saveLogsInFile: ['a.txt', 'logs.txt']
}).launch({
    token: 'the token of my bot'
});
```

{% hint style="info" %}
If the files are not found, it will try to create them, else it will append logs to them.
{% endhint %}

## [Documentation](https://advanced-command-handler.github.io/docs/classes/logger.html)

