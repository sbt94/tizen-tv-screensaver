# Tizen TV Screensaver

A Chromecast-style ambient screen for Samsung Smart TVs (Tizen). It shows beautiful photos, famous paintings or live generative art with a clock, keeps the screen awake, protects against burn-in, and lets you cast photos or text (like a party menu) to the TV from Telegram.

![Live art modes](screenshots/live-art.jpg)

## Features

- **Four background modes**
  - `photos`: curated landscape photography from Wikimedia Commons Featured Pictures, or Unsplash if you add a key
  - `art`: public-domain paintings from the Art Institute of Chicago
  - `mix`: photos and paintings alternating (default)
  - `live`: six animated generative artworks drawn in code (silk flow, aurora, bokeh, sunset ridges, ribbons, galaxy). Works offline.
- **Clock and date** (Hebrew date by default) with photo and artist credits
- **Keeps the screen on** with `tizen.power.request("SCREEN", "SCREEN_NORMAL")`
- **Burn-in protection**: the clock and credits drift slowly, backgrounds rotate, shared content shifts by a few pixels every minute
- **Telegram bot**: send a photo, video or text to your bot and it appears on the TV. No server needed, the TV polls Telegram directly.
- **Remote control support** and automatic fallback to live art when there is no internet

![Telegram casting](screenshots/telegram.jpg)

## Install

### 1. Enable Developer Mode on the TV

1. Open **Apps** on the TV.
2. Type `12345` with the remote. On remotes without number keys, press the **123** button to open an on-screen keypad. On some newer models the option is under **Apps > App Settings**.
3. Turn Developer mode **On**, enter your computer's IP address, and restart the TV by holding the power button for 2 seconds.

### 2. Set up the tools

1. Install [VS Code](https://code.visualstudio.com/) and the **Tizen Extension** and **Tizen TV** extensions.
2. Let the extension install the Tizen SDK, plus **Samsung TV Extensions** and **Samsung Certificate Extension** (version 2.0.73 or later) from the Package Manager.
3. In **Device Manager**, add the TV by its IP address (port `26101`).
4. In **Certificate Manager**, create a **Samsung** certificate for **TV**. Add your TV's DUID, which you can get with:
   ```
   sdb connect <TV_IP>
   sdb shell 0 getduid
   ```

### 3. Configure

```
cp config.example.js config.js
```

Edit `config.js`. Every field is optional:

| Setting | Description |
| --- | --- |
| `BOT_TOKEN` | Telegram bot token from [@BotFather](https://t.me/BotFather) |
| `ALLOWED_CHAT_IDS` | Chat IDs allowed to send content. Leave empty and use `/pair` instead. |
| `MODE` | Starting mode: `mix`, `photos`, `art` or `live` |
| `UNSPLASH_KEY` | Unsplash Access Key from [unsplash.com/developers](https://unsplash.com/developers) |
| `UNSPLASH_QUERY` | Unsplash search query, default `landscape nature` |

`config.js` is in `.gitignore`, so your keys are never committed.

### 4. Build and run

Open the folder in VS Code. In the Tizen panel, check that the project, device and certificate are selected under **Active Targets**, then click **Build Project** and **Run Project**.

To open it later, find **TV Screensaver** under **Apps** on the TV. You can add it to the Home bar from there.

## Remote control

| Button | Screensaver | While showing Telegram content |
| --- | --- | --- |
| ◀ ▶ | Previous / next photo or artwork | Previous / next item |
| ▲ ▼ | Brightness (live mode) | Brightness |
| OK | Show / hide seconds | |
| Green | Switch mode | |
| Play/Pause | Pause auto-rotation | Pause auto-rotation |
| Red / Info | Help and Telegram pairing code | Help |
| Back | Exit | Back to clock |

## Telegram

1. Create a bot with [@BotFather](https://t.me/BotFather) and put the token in `config.js`.
2. The TV shows a 6-digit pairing code under the clock. Send `/pair <code>` to your bot. Pairing is saved on the TV. To pair another phone, press the red button on the remote: it shows a code and accepts new pairings for 5 minutes.
3. Send:
   - **A photo**: shown full screen, with its caption. Send it as a file for full quality.
   - **A video or GIF** (up to 20 MB, a Telegram Bot API limit): plays full screen, muted by default. A single video loops.
   - **Text**: shown as a large card, great for a menu.
   - **Several items**: they rotate every 20 seconds. Videos play to the end before moving on.

| Command | Action |
| --- | --- |
| `/clock` | Back to the screensaver and clear shared items |
| `/show` | Show the shared items again |
| `/next` | Next item |
| `/undo` | Remove the last item |
| `/sound` `/mute` | Turn video sound on or off |
| `/mix` `/photos` `/art` `/live` | Switch background mode |
| `/help` | List commands |

Shared content returns to the clock automatically after 8 hours.

## Security notes

- `config.js` holds your bot token and API key and is ignored by git. The built `.wgt` package contains it too, so do not share your build or upload it anywhere.
- Only paired chats can put content on the TV. The pairing code changes after 5 wrong guesses, each chat gets 5 tries, and once a phone is paired, new pairings need the red button on the remote.
- Anyone with your bot token can control the bot. If it leaks, send `/revoke` to [@BotFather](https://t.me/BotFather) and update `config.js`.

## Troubleshooting

- **Install fails with a certificate error**: the TV's DUID is missing from the certificate. Connect the TV first, then recreate the certificate.
- **The screen still dims**: turn off Eco / energy saving and auto power off in the TV settings.
- **No photos, only live art**: no internet connection. The app retries every 5 minutes.
- **The bot does not respond**: the app must be running on the TV. If the TV shows a webhook message, open `https://api.telegram.org/bot<TOKEN>/deleteWebhook`.
- **Animation stutters on older TVs**: lower `ART_FPS` in `index.html` to 20.

## Credits

- Photos: [Wikimedia Commons](https://commons.wikimedia.org/wiki/Commons:Featured_pictures) Featured Pictures and [Unsplash](https://unsplash.com). Photographer credit is shown on screen.
- Paintings: [Art Institute of Chicago API](https://api.artic.edu/docs/), public-domain works only.

## License

[MIT](LICENSE)
