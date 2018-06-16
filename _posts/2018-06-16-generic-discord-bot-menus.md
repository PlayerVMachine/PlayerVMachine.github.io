---
title: Generic Discord Bot Menus
categories: code
date: 2018-06-16 08:00:00 -1700
---

### Making Bot Menus!

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

So here we handle the menu timing out if the user hasn't replied in a specified interval, in this case 10 seconds. That's done using Node's `setTimeout(callback, delay)` you can read up on timers in Node [here](https://nodejs.org/api/timers.html). Inside the callback we remove the event listener that we will be creating a bit further down, and delete the menu message.

```javascript
    let timeoutmsg = await bot.createMessage(msg.channel.id, 'Menu timed out.')
    setTimeout(() => {
        timeoutmsg.delete('Menu Timeout.')
    }, 4000)
```

You can optionally Send a message letting the user now the menu timed out, that's why it disappeared and then again using setTimeout you can delete that message too. You can include as much or little of this portion as you like, whatever you feel gives the best experience.

```javascript
    const handleReply = async (reply) => {
        if (reply.author.id == msg.author.id) {

            clearTimeout(menuTimeout)
            bot.removeListener('messageCreate', handleReply)

            menuMessage.delete('Menu close.') // delete first question
            responseHandler(reply)
        }
    }
```

Next up we define the callback function that will handle getting the user's response and doing something with it. The reason we need to define the callback function is so that we can remove the event listener once the reply is recieved. `removeListener(eventname, listener)` requires that the handler function (listener) be defined so it can't be an anonymous function. First we check if the message that triggered our event listener came from the user who called the original function and if so we again remove the listener for a reply from the user and clear the timeout we defined above since the user has replied. Then we delete the menu message and pass the reply message to the responseHandler function passed to this function that will do whatever you want to do with the reply. If the message that triggered the listener isn't from the original user than we do nothing and continue to wait for the timeout or a reply from the original user.

```javascript
bot.on('messageCreate', handleReply)
```

Lastly we register the event listener that will listen for messages created and call the handleReply funtion whenever a message is sent and the menu is open.