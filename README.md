# minui-wifi.pak

A MinUI app that manages wifi connections.

## Requirements

This pak is designed and tested on the following MinUI Platforms and devices:

- `miyoomini`: Miyoo Mini Plus (_not_ the Miyoo Mini)
- `my282`: Miyoo A30
- `my355`: Miyoo Flip
- `rg35xxplus`: RG-35XX Plus, RG-34XX, RG-35XX H, RG-35XX SP
- `tg5040`: Trimui Brick (formerly `tg3040`), Trimui Smart Pro

Use the correct platform for your device.

## Installation

1. Mount your MinUI SD card.
2. Download the latest release from Github. It will be named `Wifi.pak.zip`.
3. Copy the zip file to `/Tools/$PLATFORM/Wifi.pak.zip`.
4. Extract the zip in place, then delete the zip file.
5. Confirm that there is a `/Tools/$PLATFORM/Wifi.pak/launch.sh` file on your SD card.
6. Unmount your SD Card and insert it into your MinUI device.

## Usage

> [!NOTE]
> When wifi is enabled for the Miyoo A30 or Miyoo Mini Plus, MinUI may report an incorrect status for the connection. See [this pull request](https://github.com/shauninman/MinUI/pull/41) for more details.

Browse to `Tools > Wifi` and press `A` to enter the wifi management screen. It will display your current wifi connection status, and allow you to scan for available networks as well as connect to them (with password entry).

### Debug Logging

Debug logs are written to the`$SDCARD_PATH/.userdata/$PLATFORM/logs/` folder.

### Customizing Wifi Credentials

> [!IMPORTANT]
> The Wifi pak exposes a UI for managing wifi networks, so this is only relevant for prepopulating the wifi credentials file.

In the root of your SD card, place a `wifi.txt` file. This file should store network credentials for accessing your wifi networks.

> [!NOTE]
> In previous versions of the pak, this file could also be located at `/Tools/tg5040/Wifi.pak/wifi.txt`. The old path is still supported - though deprecated - and users should migrate to the new path. New versions of the pak will transparently migrate the `wifi.txt` to the root of the SD card.

Format:

- colon (`:`) delimited fields, where the first field is the network name and the second is the password.
- Empty lines and lines beginning with hashes (`#`) are ignored
- Whitespace is stripped from all fields

The following is an example:

```shell
# awesome wifi for home
Minui Rules:shauninman-too

# the previous newline is ignored
madison:CatmillaNumber1
```

### Static IP (optional)

To skip DHCP and assign a fixed IP address, add two extra fields to a network entry: the IP address in CIDR notation and the gateway.

```
SSID:Password:IP/prefix:Gateway
```

Example:

```shell
# home network with static IP
Minui Rules:shauninman-too:192.168.1.100/24:192.168.1.1
```

> [!NOTE]
> Static IP is applied to the **first network** in `wifi.txt` only. Additional networks always use DHCP.

> [!NOTE]
> When using a static IP, **passwords cannot contain colons** (`:`) since the third colon is used as the IP field separator. Networks using DHCP (no IP field) are not subject to this restriction.

> [!NOTE]
> DNS is not configured automatically when using a static IP. If you need name resolution, set up `/etc/resolv.conf` manually on your device.

> [!NOTE]
> Static IP is not supported on `rg35xxplus` via `netplan` when no gateway is provided — include the gateway field to generate a valid route.

#### RG-35XX Plus static IP

On `rg35xxplus`, the static IP is configured directly in the netplan YAML (`dhcp4: false`). No manual steps are required — the same `wifi.txt` format applies.

## Screenshots

<img src="screenshots/main-screen-disconnected.png" alt="Main screen (disconnected)" width="240"> <img src="screenshots/main-screen-connected.png" alt="Main screen (connected)" width="240"> <img src="screenshots/network-scanning.png" alt="Scanning for networks" width="240"> <img src="screenshots/network-list.png" alt="Network list" width="240"> <img src="screenshots/network-connecting.png" alt="Connecting to network" width="240"> <img src="screenshots/network-password.png" alt="Network password entry" width="240"> <img src="screenshots/network-enabling.png" alt="Enabling wifi" width="240">
