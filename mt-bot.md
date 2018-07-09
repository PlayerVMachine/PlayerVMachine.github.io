---

layout: page
title: Media Central
permalink: /media-central
image: BcastTower.png

---

Media Central is a bot that provides game statistics, posting/mailbox features, Spotify info, reminders, and notes features with more on the way!

The `|` signifies or, so use one of the options seperated by `|` in a command, and `<>` indicates text that you have to enter

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

<p>Modes: solo, solo-fpp, duo, duo-fpp, squad, squad-fpp</p>
<p>Regions: xbox-as, xbox-eu, xbox-na, xbox-oc, pc-krjp, pc-jp, pc-na, pc-eu, pc-ru, pc-oc, pc-kakao, pc-sea, pc-sa, pc-as</p>

**LoL**:
_Not yet publicly available_

```
m.lol user|summoner <in game name>
```
Get basic League of Legends user stats like rank and LP.

**Game News Webhooks**:
The following commands require the Manage Channels permission to use.

```
m.news set|sub
```
Creates a webhook in the channel the command is used in if a subscription is chosen using reactions
```
m.news unset|unsub
```
Shows current subscriptions and use reactions to cancel them

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
Search for songs, get the top new releases of the week on demand or weekly in your mailbox, suggested playlists.

```
m.spotify search|lookup <song name>
```
Search for a song by name on Spotify, returns top 3 results.
```
m.spotify new|releases <page #>
```
Browse the top 100 new relases 10 at a time using the optional page number.
```
m.spotify album|adetail <new release #>
```
Get a more detailed album view for an album on the top 100 new releases list using its position number
```
m.spotify playlists|plists|pl
```
Get 5 playlist suggestions
```
m.spotify sub|unsub
```
Subscribe or Unsubscribe to the top 10 release list in your mailbox every Friday.


<h5 style="color:#D34043">Notes and Reminders</h5>

Set reminders for yourself that you can view and cancel at anytime.

```
m.remind me|set <reminder text> in <time><unit>
```
Set a reminder, the bot will DM you your reminder at the specified time unit is one of m, h, d, w, M, y from minutes to years default if no unit set is minutes.
```
m.remind view|list
```

Make notes for yourself from anywhere Media Central can read messages.

```
m.nts make|note|create <text> <attachments>
```
Make a note for yourself and GSC will pin it in your DMs with it. Supports message file attachments.
```
m.nts view|list|get
```
Gets a list of your current notes.
```
m.nts delete|rem|unnote <list position>
```
Using the position number of a note from the list command you can delete a note. Unpinning it will also remove it from the list.

<h5 style="color:#D34043">Announcement Type Channels</h5>

The following commands require the Manage Channels Permission to be used.

There are 3 types of channels you can configure: Announcements, Spotify, and Stream.

__Announcements__: Users will be able to subscribe to this channel once set and receive posts sent in this channel to their mailbox, no pinging required!
__Spotify__: This channel will receive a post every Friday with the week's top 10 releases.
__Stream__: This channel will recieve posts everytime a server member goes live on Twitch
```
m.chan set ann|spotify|stream <channel>
```
Sets the specified channel as one of the above types.

```
m.chan unset ann|spotify|streams
```
Unsets the previously configured channel.

<h5 style="color:#D34043">Miscellaneous</h5>

```
m.weather <location> C|F
```
Get the current weather for a location, Fahrenheit is default.
```
m.forecast <location> C|F
```
Get the 3 day forecast for for a location
```
m.clean <1 - 50>
```
Removes up to the specified number of DMs from the bot to help you clean up.
```
m.ping
```
No one asks how the bot is only ping.
```
m.prefix <new prefix>
```
Set a new prefix for the bot, no spaces, 10 characters or less. (Requires the Manage Server permission)

<h5 style="color:#D34043">More community focused features coming soon!</h5>

I have a few things in mind that I'm working on but I'd love to hear from you if you have any suggestions!

Invite Media Central to your server now! [Invite](https://discordapp.com/api/oauth2/authorize?client_id=464529935315370004&permissions=536881152&scope=bot)
<br />
Join the support [server](https://discord.gg/NNFnjFA)
<br />
<br />
Like the bot? Consider
<style>.bmc-button img{width: 27px !important;margin-bottom: 1px !important;box-shadow: none !important;border: none !important;vertical-align: middle !important;}.bmc-button{line-height: 36px !important;height:37px !important;text-decoration: none !important;display:inline-flex !important;color:#ffffff !important;background-color:#FF813F !important;border-radius: 3px !important;border: 1px solid transparent !important;padding: 1px 9px !important;font-size: 23px !important;letter-spacing:0.6px !important;;box-shadow: 0px 1px 2px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;margin: 0 auto !important;font-family:'Cookie', cursive !important;-webkit-box-sizing: border-box !important;box-sizing: border-box !important;-o-transition: 0.3s all linear !important;-webkit-transition: 0.3s all linear !important;-moz-transition: 0.3s all linear !important;-ms-transition: 0.3s all linear !important;transition: 0.3s all linear !important;}.bmc-button:hover, .bmc-button:active, .bmc-button:focus {-webkit-box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;text-decoration: none !important;box-shadow: 0px 1px 2px 2px rgba(190, 190, 190, 0.5) !important;opacity: 0.85 !important;color:#ffffff !important;}</style><link href="https://fonts.googleapis.com/css?family=Cookie" rel="stylesheet"><a class="bmc-button" target="_blank" href="https://www.buymeacoffee.com/playervm"><img src="https://www.buymeacoffee.com/assets/img/BMC-btn-logo.svg" alt="Buying me a coffee"><span style="margin-left:5px">Buying me a coffee</span></a>