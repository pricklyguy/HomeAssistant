# TP-Link / Kasa / Tapo: TPAP support for Home Assistant (unofficial workaround)

Newer TP-Link/Tapo firmware (1.4.0 and up on the devices I tested) changed how a device logs in.
Home Assistant's built-in TP-Link integration doesn't understand the new login (called TPAP) yet.

Symptoms:

- A TP-Link entry shows "Failed setup" in Settings > Devices & Services
- `TRANSPORT_UNKNOWN_CREDENTIALS_ERROR(1003)` or `responded with 403 to handshake1`
- `Unsupported device ... encrypt_type='TPAP'`
- The device still works in the Tapo app

This folder holds a **custom copy of the TP-Link integration** that uses the pending, unmerged
[python-kasa PR #1592](https://github.com/python-kasa/python-kasa/pull/1592) (commit `f4dff39`), which adds TPAP support.
It sits in `custom_components/tplink` and takes over from the built-in integration.
Entity IDs, unique IDs and automations are preserved.

## Read this first

- **This is unofficial.** The upstream pull request is not merged and has had changes requested. Use at your own risk.
- **It is pinned to Home Assistant Core 2026.11.3.** That is the only version I tested. Do not update Core
  without re-checking this on a test instance first, because the custom copy replaces part of the built-in integration.
- **It is reversible.** Delete `custom_components/tplink` and restart Home Assistant.
- **Take a full backup first**, and copy it off the machine.

## Tested

| Device | Type | Firmware | Third-Party Compatibility | Result |
|---|---|---|---|---|
| HS200 | SMART.KASASWITCH | 1.4.0 | off (second account) | works |
| S500 | SMART.TAPOSWITCH | 1.4.1 Build 260825 Rel.005446 | on | works |

Tested on Home Assistant OS with Core 2026.11.3. Adding, toggling, rediscovery and re-authentication all worked,
with entity IDs unchanged. Other models and versions are untested.

## Install

1. Take a full backup in Home Assistant and copy it off the machine.
2. In the Tapo app, turn **Auto-update off** for your TP-Link devices (do this while they still work).
3. Make sure you are on **Core 2026.11.3** (Settings > About).
4. Download `tplink-2026.11.3-f4dff39.zip` and `MANIFEST.sha256` from this folder.
5. Extract the zip so you end up with `/config/custom_components/tplink/__init__.py`
   (use the Samba or Advanced SSH add-on to get files onto Home Assistant OS).
6. Optional but recommended: check the files. The zip's SHA-256 is
   `a0d8008d3f14f5b0625e69bd323359cd56528d2002cee4fcf11b7771325d0d4a`. To check every extracted file, copy
   `MANIFEST.sha256` into `/config/custom_components/tplink/` and run
   `sha256sum -c MANIFEST.sha256` there (then delete the manifest copy, or ignore it).
7. Restart Home Assistant.
8. Go to Settings > Devices & Services > TP-Link, and click **Re-authenticate** on **one** failing device.
   Enter your TP-Link account details. Use the account the device is on.
9. If it loads and the switch toggles, repeat for the rest.

**Try each device once.** If a re-authentication fails, stop and look into why before retrying. Repeated
failed attempts can lock a device out.

## If a device still fails with error 1003

That device may be on an account with **Third-Party Compatibility** switched on, and for some devices that
makes them use the older login that fails. What worked for me with the HS200s:

1. Create a second TP-Link account and switch Third-Party Compatibility **off** in its account settings.
2. For each affected device: remove it from the old account, factory reset it, and add it to the second account
   with the same name and Wi-Fi. Turn Auto-update off straight away.
3. Re-authenticate it in Home Assistant with the second account.

The S500s I tested worked without this step, so try the plain re-authentication first.

## Optional: keep the devices off the internet

I block the devices from the internet at my router so that firmware can't change under me. Home Assistant
controls them locally, so the Tapo app's cloud features stop working.

## Rollback

Delete `/config/custom_components/tplink` and restart Home Assistant. The built-in integration takes over again.

## Updating Home Assistant later

This custom copy shadows the built-in integration, so a Core update does not upgrade TP-Link support until
you remove the folder. Check this page and the upstream pull request first. Once the fix ships in a Home Assistant
release, delete the folder and use the built-in integration.

## Licenses

See [LICENSE-NOTICE.md](./LICENSE-NOTICE.md). The package includes GPL-3.0 code.
