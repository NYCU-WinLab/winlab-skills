---
name: prplos
description: Use when working with prplOS / prplmesh devices (often WiFi 7 reference platform APs from MaxLinear, Genexis, SoftAtHome). Triggers on "prplOS", "prplmesh", "ubus call WiFi", "TR-181", "amx", "beerocks", "configure AP via console", "WinLab AP", "winlab 的 AP", "AP 設定", "prplos 設定". Covers serial console workflow, TR-181/amx datamodel, network/WiFi configuration, scanning, channel selection, and the non-obvious traps (configure.sh override, hostapd `multi_ap` semantics, default route + MASQUERADE wiped by `gateway_mode`, KeyPassPhrase case sensitivity, killing wld removes the WiFi datamodel).
---

# prplOS Operations Skill

Hands-on field manual for configuring prplOS / prplmesh-based access points via serial console. Captures the TR-181 datamodel API surface, the daemon topology (`wld` ↔ `beerocks` ↔ `hostapd`), and the specific traps that bite anyone touching this firmware for the first time.

All requirements use RFC 2119 keywords (MUST, SHOULD, MAY).

## Platform Topology — Know Before You Touch

```
                   ┌───────────────────────────────┐
USB-Serial console │  /dev/cu.PL2303G-USBtoUART*   │ 115200 8N1
                   └───────────────┬───────────────┘
                                   │
                          BusyBox ash (root, no password by default)
                                   │
   ┌─────────────────────┬─────────┼─────────────┬──────────────────────┐
   │                     │         │             │                      │
   ▼                     ▼         ▼             ▼                      ▼
ubus / ubus-cli       wld      hostapd      beerocks_*              configure.sh
(JSON-RPC bus)    (wireless   (per-radio  (prplmesh           (/etc/script/root-ap/
                   driver     hostapd     controller +         configure.sh —
TR-181 datamodel   abstraction).conf      agent)               hardcoded SSIDs!)
exposed as           generates                                      ↑
WiFi.* / IP.* /      runtime                                        │
DNS.* / Routing.*    config                                  RUNS ONCE on AP
                                                              role provisioning,
                                                              writes its OWN
                                                              SSID/password
```

Key insight: **the TR-181 datamodel is the API. `ubus call <Object> _set` writes parameters; `wld` regenerates `/tmp/wlanX_hapd.conf` and reloads hostapd.** But `beerocks_controller` (EasyMesh) and `configure.sh` may push their own values on top, fighting your changes.

## Console Connection

- **MUST** use 115200 baud, 8N1, no flow control. Other rates return garbage or silence.
- **MUST** install the Prolific PL2303 driver via `brew install --cask prolific-pl2303` and approve the system extension in *System Settings → General → Login Items & Extensions → Driver Extensions*. State sequence on macOS Tahoe: `[activated waiting for user]` → enable toggle → `[activated enabled]` → `/dev/cu.PL2303G-USBtoUART*` appears.
- **SHOULD** drive the console programmatically with Python `pyserial` instead of `screen`, so output is captured and verifiable. Reference helper:

```python
import serial, time
PORT = "/dev/cu.PL2303G-USBtoUART140"  # name varies per device
def ap_run(cmds, idle=1.0, mw=20):
    s = serial.Serial(PORT, 115200, timeout=0.2)
    s.write(b"\r\n"); time.sleep(0.3)
    while s.read(2048): pass  # drain
    for c in cmds:
        s.write(c.encode() + b"\r\n")
        # read until idle
        end = time.time() + mw; last = time.time(); buf = b""
        while time.time() < end:
            chunk = s.read(1024)
            if chunk: buf += chunk; last = time.time()
            elif time.time() - last > idle: break
        print(f"$ {c}\n{buf.decode('utf-8','replace')}")
    s.close()
```

- **MAY** use `screen /dev/cu.PL2303G-USBtoUART* 115200` for interactive sessions. Not recommended for automation — output cannot be captured cleanly.
- **MUST** verify whoami/prompt before issuing commands (`root@prplOS:/#`). prplOS reference firmware ships with **no root password** — `# WARNING! There is no root password defined on this device!` is normal but a security concern.

## TR-181 / amx Datamodel Cheat Sheet

prplOS exposes a TR-181 datamodel (broadband forum standard) over ubus, named under `Device.WiFi.*`, `Device.IP.*`, etc. Two CLI flavors:

| Tool | Use For | Example |
|------|---------|---------|
| `ubus call <Path> _set '{"parameters":{...}}'` | Write parameters atomically | `ubus call IP.Interface.2.IPv4Address.1 _set '{"parameters":{"IPAddress":"10.0.0.1"}}'` |
| `ubus call <Path> _add '{"parameters":{...}}'` | Add a new instance to a multi-instance object | `ubus call DNS.Client.Server _add '{"parameters":{"DNSServer":"8.8.8.8",...}}'` |
| `ubus-cli '<Path>.?'` | Read everything under a path | `ubus-cli 'WiFi.Radio.?'` |
| `ubus-cli '<Path>.<Param>?'` | Read one parameter | `ubus-cli 'WiFi.SSID.1.SSID?'` |
| `ubus -v list <Object>` | List methods + their parameter schema | `ubus -v list WiFi.Radio.1` |

- **MUST** check `amxd-error-code` in the response. `0` = success. `4` = parameter not found / case mismatch / required field missing.
- **MUST NOT** trust ubus-cli filter expressions like `[Alias=="..."]` for verification — they often emit `Missing or invalid operator`. Use direct path queries instead.
- **SHOULD** prefer `ubus -v list <Object>` over guessing method names. The output reveals all `_*` and named methods with their schemas.

## Standard Configuration Recipes

### Static IP / Gateway / DNS

```bash
# 1. Static IP on WAN interface (interface index varies — check first with `ip a`)
ubus call IP.Interface.2.IPv4Address.1 _set '{"parameters":{
  "AddressingType":"Static",
  "IPAddress":"140.113.194.250",
  "SubnetMask":"255.255.255.224"}}'

# 2. Default gateway
ubus call Routing.Router.1.IPv4Forwarding.1 _set '{"parameters":{
  "GatewayIPAddress":"140.113.194.225",
  "Enable":true,
  "Origin":"Static"}}'

# 3. Device's own DNS resolver list — _add creates a new entry
ubus call DNS.Client.Server _add '{"parameters":{
  "Enable":true,"DNSServer":"8.8.8.8","Type":"Static",
  "Interface":"Device.IP.Interface.2"}}'

# 4. DNS served to clients (DNS.Relay.Forwarding) — Alias must be unique
ubus call DNS.Relay.Forwarding _add '{"parameters":{
  "Enable":true,"DNSServer":"8.8.8.8","Type":"Static",
  "Interface":"Device.Logical.Interface.1.","Alias":"upstream-1"}}'

# 5. Switch DNS Relay mode on the LAN bridge
ubus call DNS.Relay.X_PRPL-COM_Config.1 _set '{"parameters":{
  "DNSMode":"Static","ForwardingRef":"8.8.8.8"}}'
```

- **MUST** verify with `ip -4 addr show wan`, `ip -4 route`, `cat /tmp/resolv.conf` after applying. The TR-181 set returning `error-code: 0` does not guarantee the kernel state is updated.

### WiFi: Rename SSID + Set Passphrase

```bash
# SSID name lives on the SSID object
ubus call WiFi.SSID.1 _set '{"parameters":{"SSID":"WinLab"}}'

# Passphrase lives on AccessPoint.X.Security — note CASE on KeyPassPhrase!
ubus call WiFi.AccessPoint.1.Security _set '{"parameters":{
  "KeyPassPhrase":"winlabisgood"}}'

# For WPA3 (mandatory on 6 GHz), ALSO set SAEPassphrase:
ubus call WiFi.AccessPoint.5.Security _set '{"parameters":{
  "KeyPassPhrase":"winlabisgood","SAEPassphrase":"winlabisgood"}}'
```

- **MUST** spell the field as **`KeyPassPhrase`** with a capital P in `Phrase`. The variant `KeyPassphrase` (lowercase p) returns `error_code: 4 - required: true` — silent confusion.
- **MUST** also set `SAEPassphrase` on any AccessPoint whose `Security.ModeEnabled` is `WPA3-Personal` (or WPA2-WPA3 mixed). Setting only `KeyPassPhrase` will not unlock SAE clients.
- **SHOULD** check `Security.ModesAvailable` before forcing WPA3 — 6 GHz radios typically only list `WPA3-Personal`; 2.4/5 GHz list multiple modes.

### Channel Selection

```bash
# Atomically set channel + bandwidth + reason (preferred)
ubus call WiFi.Radio.1 setChanspec '{"channel":11,"bandwidth":"20MHz","reason":"manual"}'
ubus call WiFi.Radio.2 setChanspec '{"channel":149,"bandwidth":"80MHz","reason":"manual"}'

# Alternative for individual params (may not trigger driver reconfigure):
ubus call WiFi.Radio.1 _set '{"parameters":{"Channel":11,"OperatingChannelBandwidth":"20MHz"}}'
```

- **MUST** verify at both layers — TR-181 (`WiFi.Radio.X.Channel?`) AND driver (`iw dev | grep channel`). Datamodel may report success while driver is still on the old channel during DFS lock.
- **SHOULD** disable `AutoChannelEnable` (set to `0`) before manual channel selection, otherwise ACS may override.

### Spectrum Scan (find clean channels)

`iw dev wlanX scan` does **NOT** work while the radio is in AP mode (`command failed: Network is down`). Use the prplOS amx scan API instead:

```bash
# Trigger scan (async)
ubus call WiFi.Radio.1 startScan '{"forceFast":true}'
# Wait 12-18 seconds for completion
sleep 15
# Read results — minRssi filters weak signals
ubus call WiFi.Radio.1 getScanResults '{"minRssi":-95}'
```

- **MUST** wait at least 12 seconds after `startScan` before calling `getScanResults`. Querying earlier returns empty results.
- **SHOULD** also use `iw dev wlanX survey dump` for channel utilization (busy time / noise floor) — this works even in AP mode and gives complementary data: a channel with few visible APs but high `channel busy time` indicates non-WiFi interference (microwave, BT).
- **MAY** call `WiFi.Radio.X scanCombinedData` for richer per-AP info (beacon intervals, SignalNoiseRatio breakdowns).

## The Traps — Read Before Debugging

### Trap 1: `configure.sh` clobbers your SSID

`/etc/script/root-ap/configure.sh` contains hardcoded `ubus-cli WiFi.SSID.X.SSID="TEST-BH"` and `KeyPassPhrase="prplmesh"` lines. **It does NOT auto-trigger** (not in init.d / cron / hotplug), but **anyone** running `service prplmesh gateway_mode` will indirectly invoke it. After it runs, your manually-set values are gone.

- **MUST** patch `/etc/script/root-ap/configure.sh` itself if you want SSID/password to survive a `gateway_mode` cycle. Example:
  ```bash
  sed -i 's/TEST-BH/WinLab/g; s/TEST-FH/WinLab-Guest/g; \
          s/KeyPassPhrase="prplmesh"/KeyPassPhrase="winlabisgood"/g; \
          s/KeyPassPhrase="12345678"/KeyPassPhrase="winlabisgood"/g' \
          /etc/script/root-ap/configure.sh
  ```
- **MUST** also fix `MultiAPType` lines — see Trap 3.

### Trap 2: Stopping prplmesh kills `wld`, removes the WiFi datamodel

`/etc/init.d/prplmesh stop` triggers `prplmesh_whm stop`, which **kills `wld`** as a side effect. After that, `WiFi.*` ubus calls return `Command failed: Not found` because there is no datamodel publisher.

- **MUST NOT** run `prplmesh stop` before WiFi datamodel writes. Restart with `/etc/init.d/prplmesh_whm start && sleep 5 && /etc/init.d/prplmesh start` to bring `wld` back.
- **SHOULD** prefer surgical `pkill -f beerocks_controller` if the goal is just to silence the EasyMesh push-back, rather than nuking `wld`.

### Trap 3: `MultiAPType="BackhaulBSS"` blocks normal client association

`configure.sh` ships with main SSIDs (1, 4, 7) marked as **`BackhaulBSS`** and guest SSIDs (3, 6, 9) as **`FronthaulBSS`**. BackhaulBSS is meant for mesh agent-to-controller backhaul links and rejects normal client PSK associations. Symptom: client says "cannot join network" on main SSID but happily joins guest.

- **MUST** flip main APs to `FronthaulBSS` (or empty `""`) for end-user use:
  ```bash
  ubus call WiFi.AccessPoint.1 _set '{"parameters":{"MultiAPType":"FronthaulBSS"}}'
  ubus call WiFi.AccessPoint.3 _set '{"parameters":{"MultiAPType":"FronthaulBSS"}}'
  ubus call WiFi.AccessPoint.5 _set '{"parameters":{"MultiAPType":"FronthaulBSS"}}'
  ```
- **MUST** patch `configure.sh` accordingly so the change persists across `gateway_mode` cycles.

### Trap 4: hostapd `multi_ap` value semantics (counter-intuitive)

In `/tmp/wlanX_hapd.conf`, the `multi_ap=N` field maps as:

| Value | Meaning |
|-------|---------|
| `0` | Disabled (plain AP) |
| `1` | **Backhaul** BSS |
| `2` | **Fronthaul** BSS ← what you want for normal clients |
| `3` | Both |

`multi_ap=2` is the "normal client + EasyMesh fronthaul" mode. **Do not panic when you see `multi_ap=2` on your client-facing SSID — that is correct.**

### Trap 5: `service prplmesh gateway_mode` wipes default route + MASQUERADE rules

After the prplmesh `gateway_mode` provisioning runs, the device may end up with:
- No default route (`ip -4 route` does not show `default via ...`)
- No MASQUERADE rules in `iptables -t nat -L POSTROUTING` (clients get DHCP but cannot reach the internet)

This produces the diagnostic signature: AP itself returns `ping: connect: Network unreachable` from `ping 8.8.8.8`.

- **MUST** re-add the default route via `ubus call Routing.Router.1.IPv4Forwarding.1 _set '{"parameters":{"GatewayIPAddress":"...","Enable":true,"Origin":"Static"}}'` AND verify with `ip -4 route`. If still missing, add directly with `ip route add default via <gw> dev wan`.
- **MUST** re-add MASQUERADE rules manually:
  ```bash
  iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o wan -j MASQUERADE
  iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o wan -j MASQUERADE
  iptables -t nat -A POSTROUTING -s 192.168.3.0/24 -o wan -j MASQUERADE
  ```
- **SHOULD** prove the data path works by source-routed ping: `ping -c 3 -I 192.168.1.1 8.8.8.8`.
- **SHOULD** persist via the firewall datamodel (`Firewall.Service.NAT.*`) — runtime `iptables -A` does not survive reboot.

### Trap 6: `_scan` method does not exist — use `startScan` + `getScanResults`

Common naming guesses (`_scan`, `triggerScan`, `scan`) are unreliable. **The supported pair is**:
- `startScan` (async trigger, returns `{"retval":""}`)
- `getScanResults` (read results from an internal cache)

`iw dev wlanX scan` is blocked while the radio is hosting an AP (`command failed: Network is down`).

### Trap 7: phy ↔ netdev naming is non-obvious

The TR-181 datamodel may report `WiFi.Radio.1.Name="wlan0"` while `iw dev wlan0 info` shows it on a 5 GHz frequency. The `Name` field is unreliable for cross-referencing — always confirm with `iw dev` or `survey dump` to see the actual frequency.

## Diagnostic Workflow (when something is wrong)

When a client reports "no internet" or "cannot join", proceed in this order:

1. **AP itself online?** `ping -c 2 8.8.8.8` from the AP. If `Network unreachable`, fix routing/MASQUERADE first (Trap 5).
2. **Default route present?** `ip -4 route | grep default`. Re-add via TR-181 if missing.
3. **MASQUERADE present?** `iptables -t nat -L POSTROUTING -nv | grep MASQUERADE`. Re-add per-subnet rules if missing.
4. **Client associates?** Check `logread | grep -iE "deauth|disassoc|reject" | tail -20` for the client MAC and rejection reason.
5. **AccessPoint role correct?** `ubus-cli 'WiFi.AccessPoint.[1-6].MultiAPType?'`. Flip `BackhaulBSS` → `FronthaulBSS` (Trap 3).
6. **hostapd config matches datamodel?** `cat /tmp/wlanX_hapd.conf | grep -E '^ssid|^bss=|wpa_passphrase|multi_ap'`. If datamodel says `WinLab` but hostapd says `TEST-BH`, hostapd has not regenerated — re-run patched `configure.sh` or kick AP enable.

## Useful One-Liners

```bash
# What's actually broadcasting right now
cat /tmp/wlan*_hapd.conf | grep -E '^ssid|wpa_passphrase|^bss=|^interface=|multi_ap'

# All SSIDs at a glance
ubus-cli 'WiFi.SSID.?' | grep -E 'SSID\.[0-9]+\.(SSID|Name|Status)='

# Connected clients with IPs
cat /tmp/dhcp.leases

# Beerocks (prplmesh) live decisions
tail -f /tmp/beerocks/logs/beerocks_controller.log

# Real radio state (channel, freq, bw)
iw dev | grep -E 'Interface|channel'

# Channel utilization (works in AP mode!)
iw dev wlan0 survey dump

# Firewall counters (zero counter = packet not reaching this chain)
iptables -L FORWARD -nv --line-numbers
iptables -t nat -L POSTROUTING -nv

# Process map
ps -ef | grep -iE 'beerocks|prplmesh|hostapd|wld|amx' | grep -v grep
```

## Persistence Notes

- TR-181 `_set` writes are typically persisted by the amx framework into per-service config under `/etc/config/` (e.g., `/etc/config/wld`, `/etc/config/tr181-*`). Verify by checking the relevant config file after a write.
- Direct `iptables -A` and `ip route add` are **runtime only** — survive until reboot. For permanence write through TR-181 (`Firewall.*`, `Routing.*`) or the persistent uci config.
- `/etc/script/root-ap/configure.sh` modifications via `sed -i` survive reboot (it is in `/etc/`, not `/tmp/`), but a firmware upgrade will revert it.

## When to Reach for This Skill

Trigger conditions:
- Working with any prplOS / prplmesh device — typically WiFi 7 reference platforms with `OperatingStandards=be` on three radios (2.4 / 5 / 6 GHz).
- Configuring an AP via console (USB-Serial → BusyBox → ubus).
- Debugging "client can't connect" / "client connects but no internet" / "SSID name keeps reverting" on a prplOS device.
- Reading or writing TR-181 / amx datamodel via ubus.
- Doing spectrum survey or channel selection on a busy environment.

When in doubt: dump the AccessPoint Security object first (`ubus-cli 'WiFi.AccessPoint.X.Security.?'`) — that single command reveals available auth modes, current passphrase fields, and supported security profiles, which collapses most "why won't it connect" questions instantly.
