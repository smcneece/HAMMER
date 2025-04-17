# 🔨 HAMMER – Home Assistant Moonraker Messaging & Event Relay

> 🔖 Version: **v1.3.7.5**

This project was born out of a simple frustration: I missed being able to watch and share my 3D printer's progress with friends and family in our Discord hobby channel. OctoPrint handled this beautifully, but it doesn’t work with Klipper-based printers. So I built this—starting with Discord alerts, and then expanding into mobile, Alexa, persistent notifications, and more. What began as a fix for one missing feature became a full-featured alert system for any Klipper printer running Moonraker.

Get 3D printer alerts that actually matter! This Home Assistant automation keeps you in the loop with audio, visual, and customizable notifications—so you always know when your print is rolling, pausing, or wrapping up.

---

## 💥 Core Features

- 📢 **Discord Notifications**
- 🗣️ **Alexa Voice Alerts**
- 📱 **Mobile Notifications**
- 🖥️ **Persistent Home Assistant Notifications**
- 📷 **Snapshot & Thumbnail Support**
- 🚹 **Toggle-Based Controls**
- ⏰ **Time-Restricted Alexa Announcements**
- 🎯 **Custom Progress Intervals**
- 🧵 **Filament Used (Per Job + Lifetime)**
- 🧩 **Collapsible UI**

---

## 📸 Example Alerts

### Print Started (Discord)
![Print Start](images/print-start.png)

### Print Finished (Discord)
![Print Finished](images/print-finish.png)

---

## 🧠 TL;DR – Read This Before Filing Issues

- Fully modular — supports multiple printers with unique sensors
- Uses Moonraker integration sensors, not direct polling
- Short prints (<30 mins) may miss progress updates
- Uses fuzzy math to detect progress deltas
- Not a bug. Just how sensor polling works.

---

## 🛠️ Prerequisites

- Home Assistant
- [HACS](https://hacs.xyz/docs/setup/download)
- [Alexa Media Player](https://github.com/custom-components/alexa_media_player/wiki/Configuration)
- [Moonraker Integration](https://github.com/marcolivierarsenault/moonraker-home-assistant)

---

### 🔧 Discord Notification Setup (Optional)

Want notifications in Discord? You gotta set up a webhook and a shell command:

#### Step 1: Create a Discord Webhook
1. Open Discord and go to **Server Settings → Integrations**.
2. Click **New Webhook**.
3. Choose a channel and click **Copy Webhook URL**.

#### Step 2: Add the Shell Command to Home Assistant

In your `shell_commands.yaml`:

```yaml
printer_notify_webhook: >
  curl -X POST -F "payload_json={"content": "{{ message }}"}" -F "file=@/config/www/{{ snapshot_filename }}" https://discord.com/api/webhooks/your_webhook_here
```

Replace the URL with your real webhook. 
Do **not** quote the full URL.

Then add or make sure this exists in `configuration.yaml`:

```yaml
shell_command: !include shell_commands.yaml
```

#### Optional Debugging

To debug the message content instead of sending to Discord, try this:

```yaml
log_discord_payload: >
  echo {{ message }} > /config/www/debug_webhook_payload.txt
```

Change the blueprint shell command to `log_discord_payload` for testing.

---

### 🧰 Installing HAMMER Blueprint

HACS no longer supports adding custom repositories for blueprints directly.  
To install HAMMER, use the built-in manual import option in Home Assistant:

1. Go to **Settings → Automations & Scenes → Blueprints**
2. Click the **Import Blueprint** button in the bottom right
3. Paste this URL:
   ```
   https://raw.githubusercontent.com/smcneece/HAMMER/main/blueprints/automation/smcneece/hammer.yaml
   ```
4. Click **Preview → Import** to complete the setup

---

### 🔔 Staying Updated

HACS will not automatically notify you of new blueprint updates using this install method.

To get update notifications:
- Click the **“Watch”** button (top-right of the GitHub repo)
- Select **"Releases only"** or **"All Activity"**
- Or check the [Releases](https://github.com/smcneece/HAMMER/releases) page manually

---

## 📥 Blueprint Inputs Overview

<-- Blueprint field breakdown here -->

---

## 💡 Coming Soon / Ideas for v1.4+
- More completion stats
- Filament-out detection
- Text-only alert fallback
- Custom Scripts for triggers
- Toggle webcam/thumbnail
- HACS store submission

---

## 🏷️ GitHub Repo Tags
```
home-assistant, blueprint, 3d-printing, moonraker, klipper, discord, alexa, notifications, hacs
```
