# Order Bot

A Discord bot with a `/order` slash command that lets users pick a service from a
dropdown, submit their Roblox username + an inventory screenshot, and get a
private ticket channel created automatically for staff to handle.

## What it does

1. User runs `/order`, fills in:
   - **service** — dropdown (edit the list in `config.js`)
   - **roblox_username** — text field
   - **inventory_screenshot** — required image attachment
2. Bot creates a private channel (`#order-username-1`) visible only to that
   user and your staff role, and posts an embed with all the order details.
3. A **Close Ticket** button in the channel locks and deletes it when the
   order is done.

## Setup

### 1. Create the bot application
1. Go to https://discord.com/developers/applications → **New Application**.
2. Go to **Bot** → click **Reset Token** → copy the token (you'll only see it once).
3. Under **Bot**, make sure **Public Bot** is on (or off, if you only want to add it yourself).
4. Go to **OAuth2 → URL Generator**, check the `bot` and `applications.commands`
   scopes, pick permissions (at minimum: Manage Channels, Send Messages, Attach
   Files, View Channels), and use the generated URL to invite the bot to your server.

### 2. Get your IDs
Enable Developer Mode in Discord (User Settings → Advanced → Developer Mode),
then right-click to copy:
- Your **server** → Copy Server ID → `GUILD_ID`
- The **category** you want tickets created under → Copy Category ID → `TICKET_CATEGORY_ID`
- Your **staff role** → Copy Role ID → `STAFF_ROLE_ID`
- Your **application's Client ID** (Developer Portal → General Information) → `CLIENT_ID`

Fill these into `config.js` (GUILD_ID, TICKET_CATEGORY_ID, STAFF_ROLE_ID) and
`.env` (CLIENT_ID).

### 3. Install & configure
```bash
npm install
cp .env.example .env
# then edit .env and paste in your DISCORD_TOKEN and CLIENT_ID
```

Edit `config.js` to set your IDs and customize the `SERVICES` dropdown list
(max 25 entries — a Discord limit).

### 4. Register the slash command
Run this once, and again any time you change the command's options or the
services list:
```bash
npm run deploy
```

### 5. Start the bot
```bash
npm start
```

You should see `Logged in as YourBot#1234` in the console. The `/order`
command will now be available in your server.

## Notes / things you may want to extend
- **Ticket numbering** currently resets when the bot restarts (in-memory
  counter). For production, swap it for a small database or JSON file if you
  need persistent numbering across restarts.
- **Permissions**: the bot's own role needs `Manage Channels` in your server
  (and in the parent category, if permission sync is off) to create ticket
  channels.
- Want a transcript saved before a ticket is deleted, or a max-tickets-per-user
  limit? Both are easy additions to `handleOrderCommand` / `handleCloseTicket`
  in `index.js` — happy to add either if useful.
