# Voter

A Steem FOSSbot

**Please note that the bot is still in planning and development and is not yet functional.**

Documentaiton is [available here](/docs/index.md).

## What is this?

_Voter_ is a bot for Steem, built as a Node.js server and intended for deployment on Heroku or compatible.

For more information about the Steem FOSSbot ecosystem, check out [the doc on Steem FOSSbot](/docs/steemfossbot.md) and [our ethos](/docs/ethos.md).

## How it works

Please see the [discussion doc page](/docs/discussion.md) for and overview on how the curation algorithm works and how to use it to create a custom bot, as well as a discussion on bots on Steem in general. For technical details see the [algorithm and metrics doc page](/docs/algorithm.md)

## Installation

_Note, the dashboard is not currently functional, instead you'll find a placeholder webpage._

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy?template=https://github.com/evm2p/steem-fossbot-voter)

1. Create a Heroku account
2. Deploy this project to the Heroku using the Heroku Button above
3. Choose a name for your Voter bot (if you want) and click the _Deploy_ button
4. After the setup process finishes, click on the _Manage App_ button. If you view the server now it will not work as we haven't finished the set up yet.
5. In the newly created Heroku app, go to the _Settings_ tab and click on _Reveal Config Vars button to add the following Config Variables. Make sure to use the key name exactly as it appears here.
	1. **STEEM_USER**, with the value set to your user name, without a preceding "@" symbol.
	2. **POSTING_KEY_PRV**, with the value set to your private Steemit posting key, used to cast votes
	3. **API_KEY**, with the value set to any alphanumeric key you generate to grant access to your bot. Used to authenticate bot actions, such as start bot, as well as third party access.
	4. **EMAIL_ADDRESS** (optional), with the value set to your email address for notifications
6. Each time you set a Config variable it restarts the server, so the change can take effect. If you set an email address, you will receive a notification that your server has started.
7. Open the bot dashboard to access your dashboard and confirm it works correctly. Use the root URL of your app as hosted on Heroku, e.g. https://voter.herokuapp.com

Now you can access your bot's settings using the bot dashboard. You'll be asked for the API key you set above to authenticate every time you perform an operation.

## Usage

_Note, the dashboard is not currently functional, instead you'll find a placeholder webpage._

Open the bot dashboard using your Heroku app root URL, as above. All operations are available through the dashboard.

The operations you can perform are:

- Check bot status and stats
- Modify bot status (i.e. start / stop)
- Edit curation algorithm values and weight
- Run algorithm test

## License and acknowledgements

All original programming is under the CC0 license and so it completely open and free to use in any capacity. It's in the spirit of the project that it is open to all.

The [steem Node.js package](https://www.npmjs.com/package/steem), used to access the Steem API, is available on NPM, and the [source is on GitHub](https://github.com/adcpm/steem).