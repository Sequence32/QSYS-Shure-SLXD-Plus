> [!WARNING]
> **⚠️ UNTESTED / IN DEVELOPMENT** — This plugin has not yet been tested against real hardware. Use with caution and expect bugs. Feedback and testing reports are welcome.

# QSYS Shure SLX-D Plus Plugin

![Shure SLX-D Plus Receiver](slxphoto.png)

A Q-SYS scripted plugin for controlling and monitoring the **Shure SLX-D Plus** wireless receiver system via TCP.

## Features

- **1–4 channels** configurable via plugin properties
- **Full bidirectional feedback** from the receiver
- **Audio & RF metering** via Shure's `SAMPLE` push protocol
- **Battery monitoring** with estimated run-time display
- **TX link status** and interference detection per channel
- **Automatic reconnection** with stale data clearing
- **Configurable meter update rate** (100ms–2000ms or disabled)

## Controls

### Device Level
| Control | Type | Description |
|---|---|---|
| `connection_led` | LED | Green when connected |
| `connection_state` | Text | Connection status text |
| `model` | Text | Receiver model name |
| `device_id` | Text | Device ID |
| `firmware` | Text | Firmware version |
| `rf_band` | Text | RF band |

### Per Channel (1–4)
| Control | Type | Description |
|---|---|---|
| `chan_name_N` | Text (R/W) | Channel name (max 31 chars) |
| `frequency_N` | Text (R/W) | Frequency in MHz (set via 7-digit kHz, e.g. `0518125`) |
| `group_chan_N` | Text | Group and channel number |
| `audio_gain_N` | Knob (R/W) | Audio gain –18 to +42 dB |
| `audio_mute_N` | Button (R/W) | Audio mute toggle |
| `link_status_N` | LED | Green when transmitter is linked |
| `link_status_txt_N` | Text | "Linked" or "No TX" |
| `tx_model_N` | Text | Linked transmitter model |
| `interference_N` | LED | Lit when critical interference detected |
| `batt_bars_N` | Text | Battery level (e.g. `4/5`) |
| `batt_mins_N` | Text | Estimated battery runtime in minutes |
| `audio_peak_N` | Meter | Audio peak level (dBFS) |
| `rssi_N` | Meter | RF signal strength (dBm) |

## Properties

| Property | Description |
|---|---|
| **IP Address** | IP address of the SLX-D+ receiver |
| **Channel Count** | Number of receiver channels (1–4) |
| **Meter Rate** | Push metering interval: Disabled, 100ms, 200ms, 500ms, 1000ms, 2000ms |

## Protocol Notes

- **TCP Port:** `2202`
- **No authentication required**
- Commands are framed with angle brackets: `< COMMAND channel ATTRIBUTE value >`
- On connect, the plugin sends `< GET 0 ALL >` (device info) and `< GET N ALL >` (per channel)
- Meter data is pushed by the receiver as `< SAMPLE N ALL audPeak audRms rfRssi >` once `METER_RATE` is set
- Frequency values from the device are in Hz (7 digits); displayed as MHz
- Gain values from the device are `000–060`; mapped to `–18 to +42 dB`

## Installation

1. Copy `Shure_SLXD_Plus.qplug` to your Q-SYS Plugins folder:
   `C:\Users\<user>\Documents\QSC\Q-SYS Designer\Plugins`
2. Restart Q-SYS Designer
3. Find the plugin under **Scripted Components** in the component library
4. Set the **IP Address** and **Channel Count** properties
5. Deploy or run emulation — the plugin will auto-connect

## Compatibility

- Q-SYS Designer 9.x and later
- Shure SLX-D Plus receiver (any channel count 1–4)
