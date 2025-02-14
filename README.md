# alarmo-card <!-- omit in TOC -->
[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs)  


- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Updating](#updating)
- [Configuration](#configuration)
- [Options](#options)
  - [State configuration](#state-configuration)
- [Say thank you](#say-thank-you)

## Introduction
This is a Lovelace card for Home Assistant that can be used to control your Alarmo security system.

The card works on top of the [alarmo custom component](https://github.com/nielsfaber/alarmo). You *will* need it this as well.

This card is only suitable for controlling entities (of type `alarm_control_panel`) which are generated by the Alarmo integration, you cannot use it with any other alarm integration in HA.

Preview:

![alt text](https://github.com/nielsfaber/alarmo-card/blob/main/screenshots/alarmo-card.gif?raw=true "demonstration video")

## Features

The alarmo-card is similar to the [Lovelace alarm panel card](https://www.home-assistant.io/lovelace/alarm-panel/), but brings some extra functions:

* A countdown timer is displayed when the alarm is in arming or pending state. It shows you how much time you have left for leaving the house / disarming the alarm.
* A message is displayed when the arming of the alarm failed or the alarm has triggered, with an overview of the sensor(s) involved.
* The card gives you feedback when a wrong code was entered.
* You can resize the buttons

## Installation
<details>
<summary>click to show installation instructions </summary>

HACS installation:

Note: Ensure you have a www folder created as in config/www or the installation will succeed but fails silently
1. Click the Orange + button bottom right and search for Alarmo Card
2. Click on "Install" under the new card that just popped up.

Note: Ensure to install [`https://github.com/nielsfaber/alarmo`](https://github.com/nielsfaber/alarmo) and add the integration in order for the card to work properly. 

Manual installation

1. Download the latest release of `alarmo-card.js` [here](https://github.com/nielsfaber/alarmo-card/releases) and place it into `www/alarmo-card`.

2. Use the GUI; Configuration -> Lovelace Dashboards -> Resources Tab to add `/local/alarmo-card/alarmo-card.js?v=0`, or add a reference to the card in the resources section of `configuration.yaml`:

```yaml
resources:
  - url: /local/alarmo-card/alarmo-card.js?v=0
    type: module
```
</details>

## Updating

<details>
<summary>click to show updating instructions </summary>

Updating via HACS:
HACS should auto-remind you in the HACS tab when an update is available.

Updating manually:

Use `git pull` for manual installation updates.

Since most browsers will cache the Lovelace card code, you can force a refresh of  the browser by editing the entry in the `resources:` section in  `ui-lovelace.yaml`, by updating the version to `?v=(n+1)` (where `n` the current value).


</details>

---


## Configuration

For setting up the card, you need to assign the correct entity to the card.
All settings regarding delay times, arming modes, code settings are automatically detected.

Example YAML configuration:
```yaml
type: 'custom:alarmo-card'
entity: alarm_control_panel.alarmo
```

Configuration using UI mode:
1. Go to the Lovelace page where you want to add the card
2. Click "Edit Dashboard" on the right (under the button with the 3 dots)
* Click the "Add card" button on the bottom
* Choose "Custom: Alarmo Card" and pick the correct entity.

## Options
| Name                | Type    | Requirement  | Description                                                                                                    | Default            |
| ------------------- | ------- | ------------ | -------------------------------------------------------------------------------------------------------------- | ------------------ |
| type                | string  | **Required** | `custom:alarmo-card`                                                                                           |                    |
| entity              | string  | **Required** | Alarm_control_panel entity                                                                                     |                    |
| name                | string  | Optional     | Displayed name (next to icon)                                                                                  | (Take from entity) |
| keep_keypad_visible | boolean | Optional     | Keep the keypad always visible, also when no code input is required.<br>Only useful if numerical code is used. | `false`            |
| button_scale        | number  | Optional     | Scaling factor to apply to the size of the buttons in the card (between 1.0 and 2.5)                           | `1.0`              |
| use_clear_icon      | boolean | Optional     | Show icon (instead of text) in keypad for clearing code input.<br>Only useful if numerical code is used.       | `false`            |
| show_messages       | boolean | Optional     | Show diagnostic messages in the card when alarm is triggered or cannot be armed.                               | `true`             |
| states              | object  | Optional     | Customize the display of states in the card.<br>See [state configuration](#state-configuration).            |                    |

### State configuration 
State configuration allows users to modify the displayed texts and buttons (where applicable) in the card corresponding to certain states.

Note that the alarm entity may not support all `armed_xxx` states. States which are not supported will not show up in the card and cannot be configured.

| Name         | Type    | Applicable states                                                                                                            | Description                                                                    | Default                   |
| ------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------- |
| hide         | boolean | `armed_away`<br>`armed_home`<br>`armed_night`<br>`armed_vacation`<br>`armed_custom_bypass`                                                       | Hides the button corresponding to the state.                                   | `false`                   |
| button_label | string  | `disarmed`<br>`armed_away`<br>`armed_home`<br>`armed_night`<br>`armed_vacation`<br>`armed_custom_bypass`                                         | Overwrites the text on the button.<br>Only useful if the button is not hidden. | (Use translation from HA) |
| state_label  | string  | `disarmed`<br>`triggered`<br>`arming`<br>`pending`<br>`armed_away`<br>`armed_home`<br>`armed_night`<br>`armed_vacation`<br>`armed_custom_bypass` | Overwrites the text displayed in the card when the alarm is in this state.     | (Use translation from HA) |

**Example of using state configuration**

The example below hides the `armed_home` button (although the alarm supports it), and renames the `armed_away` / `disarmed` options.

```yaml
type: custom:alarmo-card
entity: alarm_control_panel.alarmo
states:
  armed_away:
    hide: false
    button_label: "Turn on"
    state_label: "Enabled"
  armed_home:
    hide: true
  disarmed:
    hide: false
    button_label: "Turn off"
    state_label: "Disabled"
# ... rest of card config
```
Result:

![example result](https://github.com/nielsfaber/alarmo-card/blob/main/screenshots/state-config-example.png?raw=true "example result")

---

## Say thank you
If you want to make donation as appreciation of my work, you can do so via PayPal or buy me a coffee. Thank you!

<a href="https://www.paypal.com/donate/?business=CLL4T6Y8ACXNN&no_recurring=0&item_name=Thank+you+for+supporting+my+work+on+the+Alarmo+project%2E+Your+donation+is+much+appreciated%21&currency_code=EUR" target="_blank"><img src="https://pics.paypal.com/00/s/YzlhMzI2ZjYtZDQxMi00NzNiLThmZTktOTk3MmEyYTA2Zjc0/file.PNG" width="150" /></a>
<a href="https://www.buymeacoffee.com/vrdx7mi" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png"></a>
