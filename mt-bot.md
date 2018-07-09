---

layout: page
title: Media Central
permalink: /media-central

---

Media Central is a bot that provides game statistics, posting/mailbox features, Spotify info, reminders, and notes features with more on the way!

The | signifies or, so use one of the options seperated by | in a command, and <> indicates text that you have to enter

<h5 style="color:#D34043">Game Statistics</h5>
**Supported Games**: Overwatch, Rainbow Six Siege, PUBG, League (applying for production ApI key)

**Coming Soon / planned**: World of Warcraft, Diablo 3, Star Craft 2, Wargamming.net Titles, Battlerite, Fortnite

Additionally you can set RSS webhook feeds for news on each game Media Central provide stats

**Overwatch Commands**:

```
m.ow stats <Battle tag> pc|xbl|psn quickplay|competitive
```
Gets a players stats for a given platform and game mode.
```
m.ow medals <Battle tag> pc|xbl|psn quickplay|competitive
```
Gets a list of a user's medales for a game mode.
```
m.ow hero <Battle tag> pc|xbl|psn quickplay|competitive <hero name>
```
Gets a complete list of stats for a user's given hero if they have played them.

**Rainbow Six Siege**:

```
m.r6 cas|casual <username> pc|xbl|psn
```
Gets a user's stats for casual play.
```
m.r6 rnk|ranked <username> pc|xbl|psn
```
Gets a user's stats for ranked play.
```
m.r6 topop|operator <username> pc|xbl|psn
```
Gets a user's stats for their most played operator. Add a number at the end to move trough the list.
```
m.r6 misc|general <username> pc|xbl|psn
```
Gets additional stats on a user unrelated to game mode.

**PUBG**:

```
m.pubg stats <region> <username> <game-mode>
```
Gets a player's statistics for a given game mode and region.

Modes: solo, solo-fpp, duo, duo-fpp, squad, squad-fpp
Regions: xbox-as, xbox-eu, xbox-na, xbox-oc, pc-krjp, pc-jp, pc-na, pc-eu, pc-ru, pc-oc, pc-kakao, pc-sea, pc-sa, pc-as

**LoL**:
_Not yet publicly available_

```
m.lol user|summoner <in game name>
```
Get basic League of Legends user stats like rank and LP.

<h5 style="color:#D34043">Mailbox Module</h5>
You might be asking yourself "what's this?" or maybe "OwO what's this?". With Media Central you can subsribe to users and (if configured) server announcement channels and get their posts delivered to your mailbox which you can check at any time!

```
m.post|send <message text>
```
Send a message to your followers.
```
m.pull|check
```
Check your mailbox for posts from people or channels you follow
```
m.notify
```
Subscribe to the guild's announcement channel if they have one set (must be used in the desired guild).
```
m.unnotify
```
Unsubscribe from the guild's announcement channel if you were subscribed (must be used in the desired guild).
```
m.sub|follow <user>
```
Subscribe to a user to receive their posts if they send any.
```
m.unsub|unfollow <user>
```
Unsubscribe from a user to stop receiving their posts.
```
m.subscriptions|mysubs
```
View a list of your current subscriptions.
<h5 style="color:#D34043">Spotify</h5>


<h5 style="color:#D34043">Notes and Reminders</h5>

Set reminders for yourself that you can view and cancel at anytime.

Make notes for yourself from anywhere Media Central can read messages and it will pin your notes in your DM chat with the bot. You can then refer to pins to see your notes or use the list command. You can also delete notes by unpinning the message or using the unnote command.

<h5 style="color:#D34043">More community focused features coming soon!</h5>

I have a few things in mind that I'm working on but I'd love to hear from you if you have any suggestions!

Invite Media Central to your server now! [Invite](https://discordapp.com/api/oauth2/authorize?client_id=464529935315370004&permissions=536881152&scope=bot)
Join the support server []()