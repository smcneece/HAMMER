# 🔨 HAMMER – Home Assistant Moonraker Messaging & Event Relay

This project was born out of a simple frustration: I missed being able to watch and share my 3D printer's progress with friends and family in our Discord hobby channel. OctoPrint handled this beautifully, but it doesn’t work with Klipper-based printers. So I built this—starting with Discord alerts, and then expanding into mobile, Alexa, persistent notifications, and more. What began as a fix for one missing feature became a full-featured alert system for any Klipper printer running Moonraker.

Get real-time 3D printer alerts that actually matter! This Home Assistant blueprint keeps you in the loop with loud, visual, and customizable notifications—so you always know when your print is rolling, pausing, or wrapping up.

### 💥 Core Features

- 📢 **Discord Notifications** (via shell command & webhook) – send messages with print snapshots
- 🗣️ **Alexa Voice Alerts** – announces print started, paused, resumed, or completed, with time window control
- 📱 **Mobile Notifications** – push text updates to your phone
- 🖥️ **Persistent Home Assistant Notifications** – keep alerts visible until dismissed
- 📷 **Snapshot & Thumbnail Support** – auto-captures images of your print in progress
- 🛉️ **Toggle-Based Controls** – turn on/off any type of notification directly from blueprint inputs
- ⏰ **Time-Restricted Alexa Announcements** – control when Alexa is allowed to speak
- 🎯 **Custom Progress Intervals** – get progress updates at any % values you want (1, 5, 19, 43... your rules)
- 🧵 **Filament Used (Per Job + Lifetime)** – show filament used for the job and lifetime totals in meters & miles

> You can optionally create an Alexa **Speaker Group** to broadcast across multiple Echo devices. In the Alexa app, go to **Devices > "+" > Combine Speakers** and create a group. [Official Guide](https://www.amazon.com/gp/help/customer/display.html?nodeId=G3JBWXPVSLG5A4Y4)

---

## 🔧 Prerequisites

Before getting started, make sure your Moonraker entities have **unique and meaningful names**. This blueprint assumes you've renamed your printer's sensor entities to something recognizable (like `sensor.neptune_max_current_print_state`).

To rename them:

1. Go to **Settings > Devices & Services**.
2. Find the **Moonraker** integration and click on it.
3. Click where it says **1 device** (or however many you have).
4. Click the ✏️ **pencil icon** at the top-right.
5. Change the device name to something like `Neptune Max`, then click **Update**.
6. Home Assistant will ask if you'd like to rename the associated entities—click **yes**, but be warned: this may break existing automations or dashboards that use the old names.

- Home Assistant installed and running
- [HACS (Home Assistant Community Store)](https://hacs.xyz/docs/setup/download)
- [Alexa Media Player](https://github.com/custom-components/alexa_media_player/wiki/Configuration) (via HACS)
- [Moonraker Integration for Home Assistant](https://github.com/na1vi/home-assistant-moonraker) (via HACS)
- Camera entities for snapshot (`camera_webcam`) and G-code preview (`camera_thumbnail`)
- Moonraker sensors for:
  - Printer state
  - Filename
  - Progress
  - Time remaining
  - Layer info
  - Filament used
  - Total filament
  - Total time

---

## ⚙️ Installation

1. Install all prerequisites listed above
2. Click the button below to import the blueprint directly into Home Assistant:

   [![Open your Home Assistant instance and show the blueprint import dialog](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?repository_url=https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME&filepath=blueprint.yaml)

   *(This will open your Home Assistant instance and prompt you to import the blueprint.)*

3. Create a new automation from the blueprint
4. Fill in the required fields and optional inputs
5. If you want Discord notifications, scroll down to the Discord Setup section below.
6. Once all fields are filled out to your satisfaction, click **Save** and give your automation a name. This creates an automation in your list of automations. You can create more automations for different Moonraker printers if needed.

---

## 📣 Discord Setup

Discord’s built-in `notify` service does **not** support sending both text and images in a single payload. That’s why this blueprint uses a custom `shell_command` and external `curl` call instead.

To send notifications to Discord, you’ll need to create a webhook for a specific text channel on your server. You must have the right permissions (usually server admin or channel manage permissions).

### 🔗 How to Get a Discord Webhook URL

1. Open **Discord** and navigate to the **server** and **channel** you want to use.
2. Click the ⚙️ **gear icon** next to the channel name to open **Channel Settings**.
3. Go to the **Integrations** tab, then click **Webhooks**.
4. Click **New Webhook**.
5. Give it a name and assign it to the correct channel (if not already set).
6. Click **Copy Webhook URL** and save it somewhere safe.

### 🛠️ Defining Your Shell Command

> 💡 If you've never used `shell_command:` before, just add it to the bottom of your `configuration.yaml`. Most Home Assistant installs support this by default. However, some limited environments (like certain cloud-hosted or containerized versions) might block shell access.

> ⚠️ **Be very careful editing this command.** Getting this working required a lot of trial and error. Only update the webhook URL—leave the rest exactly as shown.

Add the following to your `configuration.yaml`:

```yaml
shell_command:
  printer_notify_webhook: >
    curl -X POST -F "payload_json={"content": "{{ message }}"}"          -F "file=@/config/www/{{ snapshot_filename }}"          https://discord.com/api/YOUR_WEBHOOK_URL_HERE
```

- Replace `YOUR_WEBHOOK_URL_HERE` with your actual webhook URL from Discord.
- After saving changes to `configuration.yaml`, go to the **Developer Tools** section in the sidebar, click **CHECK CONFIGURATION**, and make sure it reports:

"Configuration will not prevent Home Assistant from starting!"

Only then should you click **RESTART**.
- The snapshot image must be stored in `/config/www/`, which maps to `/local/` in Home Assistant.
- The `message` and `snapshot_filename` variables are passed in from the automation.

---

## 🯩 Input Reference

### Required Inputs

- **Main Printer Sensor**: Your printer’s current state sensor (e.g. `sensor.neptune_max_current_print_state`)
- **Printer Friendly Name**: Used in notifications and filenames

### Camera Inputs

- **Webcam Camera**: Live camera used for snapshot capture
- **Thumbnail Camera**: Virtual camera from Moonraker that shows the G-code preview

### Alexa Settings

- **Enable Alexa Notifications**: Toggle Alexa support
- **Alexa Notification Entity**: Alexa notify service (e.g. `notify.alexa_media_echoclockdot`)
- **Notification Start/End Times**: Time window for Alexa announcements

### Persistent Notifications

- **Enable Persistent Notifications**: Show print status in HA notification area
- **Notification Title**: Optional title override

### Progress Settings

- **Progress Update Intervals**: Use the slider to set your default percentage, or enter a number manually

### Mobile Notifications

- **Enable Mobile Notifications**: Toggle phone alerts
- **Mobile Notification Entity**: Set your mobile notify service (e.g. `notify.mobile_app_yourdevice`)

---

## 🧪 To-Do / Future Ideas

- Example screenshots or notification previews
