---
title: Making Generic Discord Bot Menus
categories: code
date: 2018-06-16 08:00:00 -1700
---

# Making Bot Menus!

When developing a Discord bot you might come across the need to make text based menus for your users to navigate through and make choices. Depending on the levels of depth in your menu and the number of commands that require menus it can be tedious and messy to hardcode each menu. Using [Node.js](https://nodejs.org/en/) and the [Eris](https://abal.moe/Eris/) discord bot library I'll demonstrate a generic menu/question making function I've created.


```javascript
const replyHandler = async (menu, responseHander, msg, bot) => {
    let menuMessage = await bot.createMessage(msg.channel.id, menu)

    let menuTimeout = setTimeout(async () => {
        bot.removeListener('messageCreate', handleReply)
        menuMessage.delete('Menu Timeout.')

        let timeoutmsg = await bot.createMessage(msg.channel.id, 'Menu timed out.')
        setTimeout(() => {
            timeoutmsg.delete('Menu Timeout.')
        }, 4000)

    }, 10000)

    const handleReply = async (reply) => {
        if (reply.author.id == msg.author.id) {

            clearTimeout(menuTimeout)
            bot.removeListener('messageCreate', handleReply)

            menuMessage.delete('Menu close.') // delete first question
            responseHandler(reply)
        }
    }

    bot.on('messageCreate', handleReply)
}
```

Alright so now I'll break down the code:

```javascript
 let menuMessage = await bot.createMessage(msg.channel.id, menu)
```
This line is straight forward we await the creation of the menu message the bot will send so that we can easily delete it later.

```javascript
    let menuTimeout = setTimeout(async () => {
        bot.removeListener('messageCreate', handleReply)
        menuMessage.delete('Menu Timeout.')

        ...

    }, 10000)
```

So here we handle the menu timing out if the user hasn't replied in a specified interval, in this case 10 seconds.