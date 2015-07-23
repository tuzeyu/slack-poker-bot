## Slack Poker Bot
A bot that turns Slack into a legitimate Texas Hold'em client. Start a game in any channel or private group with 2-10 players. PokerBot will deal hands, direct message players with their hole cards, query players (in order) for their action, determine the winning hand, and (eventually) handle the pot.

![](https://s3.amazonaws.com/f.cl.ly/items/1h0S1x0e2e0t2A1W180u/Image%202015-06-30%20at%2011.07.16%20AM.png)

### Getting Started
1. Create a new [bot integration](https://my.slack.com/services/new/bot)
1. Paste the API token from your integration into `main.js` _OR_
1. Create a `token.txt` file and paste your API token there
1. `npm install`
1. `node src/main.js`
1. To start a game, `@<your_bot_name>: Deal`
1. To end a game, `@<your_bot_name>: Quit game`

### TODO
- [x] Tally up players that are participating
- [x] Basic game logic
- [x] Send player's pocket cards as a DM
- [x] Display actual cards
  - [x] Render the cards as an image attachment
  - [x] Upload to imgur
- [x] Determining the best hand
- [x] Logic for player order
- [x] Programmable bot classes
- [x] Logic for a betting round
  - [x] Cycle through players in order, tallying actions
  - [x] Allow hands to be ended early or with showdown
  - [x] More robust processing of player action
- [ ] Handle split pots
- [ ] Logic for player's chip stacks
- [ ] Deployability (Heroku or whatever)
- [ ] Infinity other things

### AI Players
Although this client was built for managing human players in a Slack channel, it has some support for AI players. To add a bot player:

1. Add a bot class under the `ai/` folder
1. Implement `getAction`, which is called whenever it is the bot's turn
1. Modify the `addBotPlayers` method in `src/bot.js` to add your bot to every game

### Spec All The Things
To run all of the tests, just do:

1. `gulp`

The tests produce legible output that matches what users in Slack would see. This is very helpful when diagnosing a logic bug:
![](https://s3.amazonaws.com/f.cl.ly/items/2T2c472a3M390J2T2t3I/Image%202015-07-23%20at%202.26.12%20PM.png)

### Dependencies
* [RxJS](https://github.com/Reactive-Extensions/RxJS)
The majority of this client is written using `RxJS`. It simplifies many of the complex player polling interactions, that would otherwise be Death By Timers, into very legible code.

* [Imgur](https://github.com/kaimallea/node-imgur) / [Lightweight Image Processor](https://github.com/EyalAr/lwip)
Each card is a separate image, and board images are created on the fly by pasting several cards onto a single canvas (with the help of  `lwip`). The resulting image is than uploaded to `imgur`, which gives us a single URL that can be passed as an attachment to the Slack API. This route was chosen to avoid uploading 311,875,200 images to the cloud, and allows us to modify the card assets easily.

* [Poker Evaluator](https://github.com/chenosaurus/poker-evaluator)
`poker-evaluator` is used for evaluating the winning hand when it comes time to show down. Here it has been extended to calculate the best 5-card hand from any 7-card hand.

* [MochaJS](http://mochajs.org/)
Most of the tricky client logic is backed up by tests, which were written using `MochaJS`.
