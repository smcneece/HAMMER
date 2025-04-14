# 🔨 HAMMER – Home Assistant Moonraker Messaging & Event Relay

> 🔖 Version: **v1.3.7.2**

This project was born out of a simple frustration: I missed being able to watch and share my 3D printer's progress with friends and family in our Discord hobby channel. OctoPrint handled this beautifully, but it doesn’t work with Klipper-based printers. So I built this—starting with Discord alerts, and then expanding into mobile, Alexa, persistent notifications, and more. What began as a fix for one missing feature became a full-featured alert system for any Klipper printer running Moonraker.

Get 3D printer alerts that actually matter! This Home Assistant automation keeps you in the loop with audio, visual, and customizable notifications—so you always know when your print is rolling, pausing, or wrapping up.

---

## 💥 Core Features

- 📢 **Discord Notifications** – send messages with print snapshots via webhook + shell command
- 🗣️ **Alexa Voice Alerts** – announces print started, paused, resumed, or completed, with time window control
- 📱 **Mobile Notifications** – push updates to your phone
- 🖥️ **Persistent Home Assistant Notifications** – keep alerts visible in HA UI
- 📷 **Snapshot & Thumbnail Support** – auto-captures image of print in progress
- 🚹 **Toggle-Based Controls** – turn on/off any notification type from automation inputs
- ⏰ **Time-Restricted Alexa Announcements** – control when Alexa is allowed to speak
- 🎯 **Custom Progress Intervals** – fuzzy logic handles missed % updates (esp. on short jobs)
- 🧵 **Filament Used (Per Job + Lifetime)** – show per-print and lifetime totals (m + mi)

> Optionally create an Alexa **Speaker Group** to broadcast to multiple Echo devices. In Alexa App: **Devices → “+” → Combine Speakers**. [Amazon Guide](https://www.amazon.com/gp/help/customer/display.html?nodeId=G3JBWXPVSLG5A4Y4)

---

## 📸 Example Alerts

### Print Started (Discord)
![Print Start](./images/print-start.png)

### Print Finished (Discord)
![Print Finished](./images/print-finish.png)

---

## 🧠 TL;DR – Read This Before Filing Issues

- This blueprint is **fully modular** – you can run multiple copies for multiple printers as long as:
  - Each printer's sensors use a unique name (like `sensor.kobra_go_progress`, `sensor.neptune_max_progress`)
  - You save a unique automation for each printer

- This blueprint **does not poll the printer directly**. It relies on the Moonraker integration’s sensor updates, which usually happen every 30 seconds.
- Because of that, **progress updates on small jobs (<30 mins)** may be delayed or skipped.
- Progress update logic uses fuzzy math — so if your job jumps from 4% to 10%, you’ll still get the ~5% alert.
- **This is not a bug.** It’s just how often the integration sends data. 

---

## 🛠️ Prerequisites

- Home Assistant running
- [HACS (Home Assistant Community Store)](https://hacs.xyz/docs/setup/download)
- [Alexa Media Player](https://github.com/custom-components/alexa_media_player/wiki/Configuration)
- [Moonraker Integration for Home Assistant](https://github.com/na1vi/home-assistant-moonraker)
- Moonraker sensors:
  - Printer state, filename, progress, print time left, filament used, total time
- Camera entities:
  - Webcam snapshot (`camera_webcam`)
  - G-code preview thumbnail (`camera_thumbnail`)

Before setup, rename your Moonraker printer entity to something clean:
- Go to **Settings > Devices & Services > Moonraker > Devices**
- Click ✏️ pencil icon → rename device (e.g. `Neptune Max`)
- Home Assistant will ask if you want to rename associated entities — do it

---

## ⚙️ Installation


### 🧰 Add This Repo to HACS (First-Time Only)

If this is your first time installing HAMMER, add this repo as a **custom repository** in HACS:

1. In Home Assistant, go to **Settings → Add-ons → HACS**
2. Click the **three-dot menu** (⋮) in the top-right → choose **Custom repositories**
3. Paste in this repo URL:
   ```
   https://github.com/smcneece/HAMMER
   ```
4. Select **Blueprints** as the category
5. Click **Add**, then find HAMMER in the Blueprints section of HACS

After this one-time setup, HACS will track updates automatically when new versions drop.


1. Install all dependencies listed above
2. Install via HACS or manual YAML import:
   - Copy raw URL from: `https://raw.githubusercontent.com/smcneece/HAMMER/main/blueprints/automation/smcneece/hammer.yaml`
3. Create a new automation from the imported file
4. Fill out the required inputs
5. Scroll down to Discord Setup if using Discord
6. Click **Save**

---

## 📢 Discord Setup

1. Open Discord, go to your desired **channel** → **⚙️ Channel Settings**
2. Go to **Integrations > Webhooks** → **New Webhook**
3. Set the name, copy the webhook URL

In `configuration.yaml`, add:

```yaml
shell_command:
  printer_notify_webhook: >
    curl -X POST -F "payload_json={\"content\": \"{{ message }}\"}" \
         -F "file=@/config/www/{{ snapshot_filename }}" \
         https://discord.com/api/YOUR_WEBHOOK_URL_HERE
```

- Replace only the webhook URL
- Store snapshots in `/config/www/` (maps to `/local/`)
- Restart HA after validating config

---

## 📱 Finding Notify Entities (Mobile, Alexa, etc.)

Notify services can be hard to find in HA. Here's the easy way:

1. Go to **Settings > Automations & Scenes**
2. Create a new automation → choose “Start with an empty automation”
3. Under **Actions**, click **Add Action → Call Service**
4. In the **Service** dropdown, search **`notifications`**
5. All `notify.` services will appear (e.g., Alexa, mobile)
6. Know your phone/device name? Even easier — search for that
7. Copy the correct service (e.g. `notify.mobile_app_pixel`, `notify.alexa_media_echo_show`)
8. Cancel or delete the automation

---

## 📥 Blueprint Inputs Overview

### Required:
- **Main Printer Sensor** – current state (e.g. `sensor.neptune_max_current_print_state`)
- **Printer Friendly Name** – for notification text & filenames

### Camera:
- **Webcam Camera** – actual snapshot source
- **Thumbnail Camera** – virtual preview image from Moonraker

### Alexa:
- **Enable Alexa Notifications** – true/false
- **Alexa Notify Entity** – like `notify.alexa_media_living_room`
- **Notification Start/End Time** – prevent 3AM screaming

### Persistent:
- **Enable Persistent Notifications** – toggle
- **Persistent Notification Title** – shown in HA sidebar

### Progress:
- **Update Interval %** – when to send progress alerts (e.g. every 5%)
- **Disable Progress Alerts (Mobile/Persistent)** – disables non-critical % spam

### Mobile:
- **Enable Mobile Notifications**
- **Mobile Notify Entity** – like `notify.mobile_app_shawn_cell`

---

## 💡 Coming Soon / Ideas for v1.4+

- More sensor data in “job completed” message
- Filament-out detection
- Downgrade options for text-only alerts
- Snapshot history / gallery mode
- Toggle to include webcam instead of thumbnail
- HACS-compatible install
- 💬 **Your idea here?** Got a killer feature idea or fun twist? Open an issue or message me!  
- Languages supported: Spicoli, Klingon, Dudebro, 60s Hippie
  *Just kidding... maybe.*


---

## 🏷️ GitHub Repo Tags

This project is tagged with the following topics to improve discoverability:

```
home-assistant, blueprint, 3d-printing, moonraker, klipper, discord, alexa, notifications, hacs
```
