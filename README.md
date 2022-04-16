# Easy Heroku Bot
Uses [hyperbot](https://github.com/hyperchessbot/hyperbot), but has the following improvements:
- Uses Stockfish 14.1 ([@19a90b](https://github.com/official-stockfish/Stockfish/commit/19a90b45bceb69aa62b5c85366343a7d1cfc695f)) and Fairy-Stockfish 14 ([@2f5d9b](https://github.com/ianfab/Fairy-Stockfish/commit/2f5d9ba90e62b832488b01304590bc40209a0694)) when `USE_STOCKFISH_14` is set to `true`. <br/>
Uses NNUE for all variants with Stockfish and Fairy-Stockfish by default. The `USE_NNUE` config var has been disabled.
- Removes all code related to Lc0 as it wasn't stable.
- Cleaned repository, removed all unnecessary scripts/files.

Other than these changes, it should work the same way hyperbot works.

View this [Lichess Blog](https://lichess.org/@/YohaanSethNathan/blog/easy-heroku-bot-improving-hyper-bot/miTywbyC) for more details on Easy Heroku Bot.

To Upgrade to a bot account I suggest you use [lichess-bot-upgrader](https://lichess-bot-upgrader.herokuapp.com/) because if you use apps like [hypereasy](https://hypereasy.herokuapp.com/) and [easychess](https://easychess.herokuapp.com/) and your token is revealed in the console logs, so authors of those repositories have access to viewing your token and lichess mentions **never** to share your token publicly.

## How to install on Heroku?

- Import or [Fork](https://github.com/TheYoBots/easyherokubot/fork) this repository to your Github.
- Create a BOT account if you do not already have one. To create one use an account that has not played any games yet, log into this account, then visit [lichess-bot-upgrader](https://lichess-bot-upgrader.herokuapp.com/), Login with Lichess, Authorize and click on 'Upgrade to a bot acount'.  
- Create an API Access Token from [here](https://lichess.org/account/oauth/token/create?scopes%5B%5D=bot:play&scopes%5B%5D=challenge:read&scopes%5B%5D=challenge:write&description=Lichess+Bot+Token) and save your Token as you will be using it later on.
- [Sign up to Heroku](https://signup.heroku.com/), if you have not already and create a new app (location and name doesn't really matter).
- In heroku, in the `Deploy` tab click on `Connect to GitHub` and then click on `search` and select your fork/import of this repository.
- Now scroll down and under `Manual deploy`, click on `deploy` with the `master` branch selected. <br/>
Note: You could also `Enable Automatic Deploys` with the `master` branch selected if you would like each commit you make to get automatically and easily deployed onto your bot. It is your choice whether you'd like to Enable or Disable Automatic Deploys.
- In the Heroku `Settings` tab click on 'Reveal Config Vars' and set the key as `TOKEN` and set its value to your newly created access token ([link](https://lichess.org/account/oauth/token/create?scopes%5B%5D=bot:play&scopes%5B%5D=challenge:read&scopes%5B%5D=challenge:write&description=Lichess+Bot+Token)). then create a new key as `BOT_NAME` and set its value to your bot's lichess username.

**You're now connected to lichess and awaiting challenges! Your bot is up and ready!**

### Using Syzygy tablebases

If you want to use 3-4-5 piece tablebases on Heroku, you have enable container build. To do this you have to [install Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) and in your console type this:
```bash
heroku login # after you type 'heroku login' your browser will open up and you'll have to login to your account

heroku stack:set container --app yourappname # replace 'yourappname' with the name of the name of your heroku app
```

### Setting Up MongoDb

1. **Sign Up** <br>
Visit https://account.mongodb.com/account/register and create an account (recommended to Sign up with Google for your ease). Once you've signed in you will come to a page that says "Deploy a cloud database". Select any one you like here (even a free database will do). After this you will get a few options to select Cloud provider and region. Setting this to defaults and simply Creating the Cluster is usually the safest option. Now that you have created your Cluster you need to set up your database.
2. **Create your first database user** <br>
The first thing you need to do is create your first database user. To do this, in your Cluster page click on `Database Access` and select `Add New Database User`. Once you do that you will have to scroll down and under `Password Authentication` enter a username and password. It is better not to add any special characters or numbers in your username or in your password and you must remember your username and password. I suggest you Auto-generate a password and make your username something simple like `username` itself. <br>
Next you have to scroll down until you see `Built-in Role` and here you must choose the role `Atlas Admin`. Once that is done click `Add User` at the bottom after scrolling down.
3. **Network Access** <br>
Now click on `Network Access` and then select `Add IP Address`. Here you can either Allow access from anywhere or allow only your current IP address or any other IP address. It might not work if Allow access from anywhere is not selected. Once you've added the IP address click on `Confirm`.
4. **Connect Database** <br>
Finally click on `Database` and you will see your Cluster. Click on `Connect` and then select `Connect your Application` and copy your connection string and replace the password with your database user's password and remove all the lines after `/myFirstDatabase? ...`.

Save what you get after Setting Up your database and set `MONGODB_URI` to what you saved (in heroku this is saved as a config var). The Value for `MONGODB_URI` should look something like this : `mongodb+srv://username:youractualpassword@cluster0.bv7tb.mongodb.net`

### Heroku Config Vars
These are the list of Config Vars that can be set:
- **USE_STOCKFISH_14** : Set it to 'true' to use latest version of Stockfish and Fairy-Stockfish (for variants) with NNUE.
- **USE_STOCKFISH_13** : Set it to 'true' to use Stockfish 13 and Multi-Variant Stockfish 13.
- **DISABLE_LOGS** : Set it to 'true' to disable logs, when true logs, position and evaluation will not be shown on home page.
- **SKIP_FEN** : Set it to 'true' to skip calculating fen for current position beyond book depth, when true, fen will only be calculated for positions within book depth, this makes the bot play faster, but the position on the home page will not be shown beyond book depth.
- **SKIP_AFTER_FAILED** : Set it to a positive integer to avoid book lookup after that many failed lookups (default: don't skip).
- **INCREMENTAL_UPDATE** : Set it to 'true' to allow incremental update of position for standard games (by default position is rebuilt from movelist on every move).
- **KEEP_ALIVE_URL** : Set this to the full link of your bot home page (`https://[yourappname].herokuapp.com`, where change [yourappname] to your Heroku app name) if you want your bot to be kept alive from early morning till late night Heroku server time. Keeping a free Heroku bot alive 24/7 is not possible, because a free Heroku account has a monthly quota of 550 hours.
- **ALWAYS_ON** : Requires paid Heroku account, set it to 'true' to keep the bot alive 24/7, you have to set `KEEP_ALIVE_URL` to your bot's full home page link for `ALWAYS_ON` to work (See also the explanation of `KEEP_ALIVE_URL` config var).
- **ALLOW_CORRESPONDENCE** : Set it to 'true' to allow playing correspondence and infinite time control games.
- **CORRESPONDENCE_THINKING_TIME** : Think in correspondence as if the bot had that many seconds left on its clock (default: 120). The actual thinking time will be decided by the engine.
- **GENERAL_TIMEOUT** : Timeout for event streams in seconds (default: 15).
- **ENGINE_THREADS** : Engine Threads uci option (default: 1).
- **ENGINE_HASH** : Engine Hash uci option in megabytes (default: 16).
- **ENGINE_MOVE_OVERHEAD** : Engine Move Overhead uci option in milliseconds (default: 500).
- **ENGINE_SKILL_LEVEL** : Engine Skill Level uci option, can be set from 0 to 20 (default: 20).
- **ALLOW_PONDER** : Set it to 'true' to make the bot think on opponent time.
- **USE_BOOK** : Use opening explorer book.
- **BOOK_DEPTH** : Up to how many plies into the game should the bot use the book, choosing too high book depth is running the risk of playing unsound moves (default: 20).
- **BOOK_SPREAD** : Select the move from that many of the top book moves, choosing to high book spread is running the risk of playing unsound moves (default: 4).
- **BOOK_RATINGS** : Comma separated list of allowed book rating brackets, possible ratings are 1600, 1800, 2000, 2200, 2500 (default: '2200,2500').
- **BOOK_SPEEDS** : Comma separated list of allowed book speeds, possible speeds are bullet, blitz, rapid, classical (default: 'blitz,rapid').
- **LOG_API** : Set it to 'true' to allow more verbose logging, logs are available in the Console of the browser.
- **USE_SCALACHESS** : Set it to 'true' to use scalachess library and multi variant engine.
- **ACCEPT_VARIANTS** : Space separated list of variant keys to accept (default: 'standard'), for non-standard variants `USE_SCALACHESS` has to be set to 'true'. Example: `'standard crazyhouse chess960 kingOfTheHill threeCheck antichess atomic horde racingKings fromPosition'`  
- **ACCEPT_SPEEDS** : Space separated list of speeds to accept (default: 'bullet blitz rapid classical'), to allow correspondence set `ALLOW_CORRESPONDENCE` to 'true'.
- **DISABLE_RATED** : Set it to 'true' to reject rated challenges.
- **DISABLE_CASUAL** : Set it to 'true' to reject casual challenges.
- **DISABLE_BOT** : Set it to 'true' to reject bot challenges.
- **DISABLE_HUMAN** : Set it to 'true' to reject human challenges.
- **GAME_START_DELAY** : Delay between accepting challenge and starting to play game in seconds (default: 2).
- **CHALLENGE_INTERVAL** : Delay between auto challenge attempts in minutes (default: 30).
- **CHALLENGE_TIMEOUT** : Start attempting auto challenges after being idle for that many minutes (default: 60).
- **USE_POLYGLOT** : Set it to 'true' to use polyglot opening book.
- **WELCOME_MESSAGE** : Game chat welcome message (delay from game start: 2 seconds, default: 'coded by @YohaanSethNathan').
- **GOOD_LUCK_MESSAGE** : Game chat good luck message (delay from game start: 4 seconds, default: 'Good luck!').
- **GOOD_GAME_MESSAGE** : Game chat good game message (delay from game end: 2 seconds, default: 'Good game !').
- **DISABLE_SYZYGY** : Set it to 'true' to disable using syzygy tablebases.
- **APP_NAME** : Heroku app name (necessary for interactive viewing of MongoDb book).
- **ABORT_AFTER** : Abort game after that many seconds if the opponent fails to make their opening move (default: 120).
- **DECLINE_HARD** : Set it to 'true' to explicitly decline unwanted challenges (by default they are only ignored and can be accepted manually).
- **USE_SOLUTION** : Set it to 'true' to use antichess solution, depends on http://magma.maths.usyd.edu.au/~watkins/LOSING_CHESS/WEB/browse.php?e2e3 api, may not work in the future.
- **DISABLE_CHALLENGE_RANDOM** : Set it to 'true' to disable challenging random bot (default: enabled).

These are the Heroku Config Vars related to setting an opening book with MongoDb:
- **MONGODB_URI** : Connect URI of your MongoDb admin user. If defined, your latest games or games downloaded from an url (Mongo version 2 only) will be added to the database on every startup. By default this config var is not defined. More instructions on setting up `MONGODB_URI` can be found [here](https://github.com/TheYoBots/easyherokubot#setting-up-mongodb).
- **USE_MONGO_BOOK** : Set it to 'true' to use the MongoDb book specified by `MONGODB_URI`.
- **DISABLE_ENGINE_FOR_MONGO** : Set it to 'true' to disable using engine completely when a MongoDb book move is available (by default the bot may ignore a MongoDb book move at its discretion and use the engine instead for better performance and to allow for more varied play).
- **MONGO_VERSION** : MongoDb book builder version, possible values are 1 (default version, builds a book from bot games as downloaded from lichess as JSON) and 2 (builds a book from bot games as downloaded from lichess as PGN, or from an arbitrary url specified in `PGN_URL`)  
- **PGN_URL** : URL for downloading a multi game PGN file for MongoDb book builder (Mongo version 2 only). More instructions on setting up `PGN_URL` can be found [here](https://github.com/hyperchessbot/hyperbot/wiki/Build-book-from-external-multi-game-PGN-file#build-book-from-external-multi-game-pgn-file).
- **MAX_GAMES** : Maximum number of games to be built by MongoDb book builder.