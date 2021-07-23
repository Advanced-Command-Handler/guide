# BetterEmbed & Embed Templates

BetterEmbed is a class from the [`discord.js-better-embed`](https://www.npmjs.com/package/discord.js-better-embed) library that I've made, you can see the code [here](https://github.com/Advanced-Command-Handler/Discord.js-Better-Embed).

## Documentation

| Name | Description |
| :--- | :--- |
| `checkSize(field?)` | Check the size of the Embed fields and return the ones that are too long. You can also specify a field to check.  \(_In `advanced-command-handler` before version 3.0, it returns an error if one field is too long._\) |
| `cutIfTooLong(field?)` | Check the size of the Embed fields and cut the fields that are too long. You can also specify a field to check and cut if it has to. |
| `setImageFromFile(link)` | Set the image of the embed using a local file path. |
| `setThumbnailFromFile(link)` | Set the thumbnail of the embed using a local file path. |
| `throwIfTooLong(field?)` | Check the size of the Embed fields and throw an error if any is too long. You can also specify a field to check. |

This is a class for creating an Embed Object, but a bit simpler like this :

```javascript
// MessageEmbed:
const {MessageEmbed} = require('discord.js');
const embed = new MessageEmbed();
embed.setImage('url');
embed.setAuthor('name', 'icon_url');
embed.setTimestamp();
embed.setFooter(client.user.username, client.user.displayAvatarURL());

// Cutting the text if too long:
embed.setDescription(embed.description.slice(0, 2048));

// BetterEmbed:
const {BetterEmbed} = require('advanced-command-handler');
const embed = BetterEmbed.fromTemplate('basic', {
    client,
});
embed.setImage('url');
embed.setAuthor('name', 'icon_url');

embed.cutIfTooLong();
```

## Using templates

Templates are a good way of automating and simplifying your code. You can use them like this:

```javascript
const {BetterEmbed, templates} = require('advanced-command-handler');
templates.funny = {
    title: '${client.user.username} says that you are funny !',
    ...templates.color, // Adding the default 'color' template
};

const embed = BetterEmbed.fromTemplate('funny', {client: message.client});
message.channel.send(embed);
```

This can simplify your embeds declarations and let you have more automation. Like if you use the `color` templates in your templates, if you change the template, every embed will have a different color without changing anything to them! So feel free to add as many templates as you need.

### Default templates

```javascript
const templates = {
    basic: {
        footer: {
            text: '${client.user.username}',
            iconURL: '${client.user.displayAvatarURL()}',
        },
        timestamp: new Date(),
    },
    color: {
        color: '#4b5afd',
    },
    get complete() {
        return {
            ...this.basic, // includes the 'basic' template
            ...this.color, // includes the 'color' template
            title: '${title}',
            description: '${description}',
        };
    },
    get image() {
        return {
            ...this.complete, // includes the 'complete' template
            image: {
                url: '${image}',
            },
        };
    },
}
```

{% hint style="warning" %}
You can overwrite default templates but these are used in the `advanced-command-handler` module so it could break things if you change them completely.

You can for example change the `color` property of the `color` template without any issue. 
{% endhint %}

