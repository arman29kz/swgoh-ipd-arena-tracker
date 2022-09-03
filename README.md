# swgoh-ipd-arena-tracker

1. Create a https://fly.io account;
2. Install cli tool for your OS https://fly.io/docs/hands-on/installing/
3. `flyctl auth login`. This will take you to a web page to confirm your account. Skip payment option;
4. `fly launch`. Run this from a new folder. it will create an important file in your current folder. Follow the prompts and give it a name (e.g. `december-2015-fleet`). Select region with arrow keys. Any US or EU option is fine.
5. Edit the `fly.toml` that was created and add the following keys under the `[env]` section and remove unnecessary networking conf. The file should look like so:

```
app = "december-2015-fleet"
kill_signal = "SIGINT"
kill_timeout = 5
processes = []

[env]
  ALLY_CODES_URL="http://rotbot.eu/sniper/ac-<random-number>-fleet.json"
  ARENA_TYPE="FLEET"
  DISCORD_WEB_HOOK="https://discord.com/api/webhooks/<webhook>"

[experimental]
  allowed_public_ports = []
  auto_rollback = true

[[services]]
  http_checks = []
  processes = ["app"]
  script_checks = []
  [services.concurrency]
    hard_limit = 25
    soft_limit = 20
    type = "connections"
```

Info how to get the values for all env keys at: https://github.com/iprobedroid/swgoh-arena-tracker#set-environment-variables
or copy from Heroku if bot is already running there.

Do not edit this with notepad or worse -- word. Try Notepadd++ or SublimeText. Each of the 3 lines needs exactly 2 spaces at the beginning. Values must be enclosed in double quotes (e.g. `ARENA_TYPE="FLEET"`)

6. Create a file called `Dockerfile` in the same directory and make this line its entire contents:

```
FROM iprobedroid/swgoh-arena-tracker:beta-24
```

That's it, it should have only one line and nothing else. Once again, use Notepad++ or Sublime or any normal text editor. Or some online editor like codesandbox.io. No Word!

7. run `fly deploy` from the folder with your two files. It will take a couple of minutes to built and start the app. You'll get a message when deploy succeeds.
8. If you were running in Heroku, stop the app there.
9. Test if things are running by completing a battle. If a notification shows up in #sniper-rb, congratulations.

Source - https://rentry.co/swgoh-sniper-bot/
