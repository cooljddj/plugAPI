#plugAPI

## Status

[![NPM](https://nodei.co/npm/plugapi.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/plugapi/)[![NPM](https://nodei.co/npm-dl/plugapi.png?months=1&height=3)](https://nodei.co/npm-dl/plugapi/)

[![NPM](https://img.shields.io/npm/l/plugapi.svg)](https://github.com/plugCubed/plugAPI/blob/master/LICENSE.md)
[![David](https://img.shields.io/david/strongloop/express.svg)](https://david-dm.org/plugcubed/plugapi)

## About

A generic NodeJS API for creating plug.dj bots.

Originally by [Chris Vickery](https://github.com/chrisinajar), now maintained by [TAT](https://github.com/TATDK) and [The plug³ Team](https://github.com/plugCubed).

**NOTE:** Currently not supporting facebook login.

## How to use
Run the following:

``` javascript
npm install plugapi --production
```

You can choose to instantiate plugAPI with either Sync or Async

**Sync:**

```javascript
var PlugAPI = require('plugapi');
var bot = new PlugAPI({
    email: '',
    password: ''
});

bot.connect('roomslug'); // The part after https://plug.dj

bot.on('roomJoin', function(room) {
    console.log("Joined " + room);
});
```

**Async:**

```javascript
var PlugAPI = require('plugapi');

new PlugAPI({
    email: '',
    password: ''
}, function(bot) {
    bot.connect('roomslug'); // The part after https://plug.dj

    bot.on('roomJoin', function(room) {
        console.log("Joined " + room);
    });
});
```

## Examples
Here are some bots that are using this API.

| Botname                                              | Room                                                            |
| ---------------------------------------------------- | --------------------------------------------------------------- |
| AuntJackie                                           | [Mix-N-Mash](https://plug.dj/mix-n-mash-2)                      |
| [BeavisBot](https://github.com/AvatarKava/BeavisBot) | [I <3 the 80's and 90's](https://plug.dj/i-the-80-s-and-90-s-1) |
| -DnB-                                                | [Drum & Bass](https://plug.dj/drum-bass)                        |
| FlavorBar                                            | [Flavorz](https://plug.dj/flavorz)                              |
| FoxBot                                               | [Approaching Nirvana](https://plug.dj/approachingnirvana)       |
| TFLBot                                               | [The F**k Off Lounge \| TFL](https://plug.dj/thedark1337)        |
| QBot                                                   | [EDM Qluster](https://plug.dj/qluster) |

Have a bot that uses the API? Let us know!

## EventListener
You can listen on essentially any event that plug emits.
```javascript
// basic chat handler to show incoming chats formatted nicely
bot.on('chat', function(data) {
    if (data.type == 'emote')
        console.log(data.from + data.message);
    else
        console.log(data.from + "> " + data.message);
});
```

Here's an example for automatic reconnecting on errors / close events!
```javascript
var reconnect = function() { bot.connect(ROOM); };

bot.on('close', reconnect);
bot.on('error', reconnect);
```
## API
Please Refer to the [Wiki](https://github.com/TATDK/plugapi/wiki) for both the Events and Actions.


## Troubleshooting

If you are having issues connecting, please check if cookies.tmp exists in node_modules/plugapi, if it does delete that and try to connect again.

Please note that at this current time only one account may be used per plugAPI installation due to the cookies.tmp file.

## Contribute
1. Clone repository to empty folder.
2. Cd to the folder containing the repository.
3. Run `npm install` to set up the environment.
4. Edit your changes in the code, and make sure it follows our [Style Guidelines](https://github.com/plugCubed/Code-Style/blob/master/JavaScript%20Style%20Guide.md).
5. Run `grunt` to compile the code and test your changes.
6. After it's bug free, you may submit it as a Pull Request to this repo.

## Misc

#### Multi line chat
Since Plug.dj cuts off chat messages at 250 characters, you can choose to have your bot split up chat messages into multiple lines:

```javascript
var bot = new PlugAPI(auth);

bot.multiLine = true; // Set to true to enable multi line chat. Default is false
bot.multiLineLimit = 5; // Set to the maximum number of lines the bot should split messages up into. Any text beyond this number will just be omitted. Default is 5.
```

#### TCP Server
You can start up a TCP server the bot will listen to, for remote administration

Example:
```javascript
    bot.tcpListen(6666, 'localhost');
    bot.on('tcpConnect', function(socket) {
        // executed when someone telnets into localhost port 6666
    });

    bot.on('tcpMessage', function(socket, msg) {
        // Use socket.write, for example, to send output back to the telnet session
        // 'msg' is whatever was entered by the user in the telnet session
    });
```
