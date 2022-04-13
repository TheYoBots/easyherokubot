# Easy Heroku Bot
Uses [hyperbot](https://github.com/hyperchessbot/hyperbot), but has the following improvements:
- Uses Stockfish 14.1 ([@19a90b](https://github.com/official-stockfish/Stockfish/commit/19a90b45bceb69aa62b5c85366343a7d1cfc695f)) and Fairy-Stockfish 14 ([@2f5d9b](https://github.com/ianfab/Fairy-Stockfish/commit/2f5d9ba90e62b832488b01304590bc40209a0694)) when `USE_STOCKFISH_14` is set to `true`. <br/>
Uses NNUE for all variants with Stockfish and Fairy-Stockfish by default. The `USE_NNUE` config var has been disabled.
- Removes all code related to Lc0 as it wasn't stable.
- Cleaned repository, removed all unnecessary scripts/files.

Other than these changes, it should work the same way hyperbot works.

View this [Lichess Blog](https://lichess.org/@/YohaanSethNathan/blog/easy-heroku-bot-improving-hyper-bot/miTywbyC) for more details on Easy Heroku Bot.

To Upgrade to a bot account I suggest you use https://github.com/TheYoBots/BotUpgrader because if you use apps like [hypereasy](https://hypereasy.herokuapp.com/) and [easychess](https://easychess.herokuapp.com/) and your token is revealed in the console logs, so authors of those repositories have access to viewing your token and lichess mentions **never** to share your token publicly.