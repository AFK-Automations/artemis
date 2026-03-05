# Moonlight Voice

**Voice dictation fix for [Moonlight Android](https://github.com/moonlight-stream/moonlight-android)** — the original open-source GameStream client.

## What This Fixes

Stock Moonlight drops all text from Android IME `commitText()` calls. This means **voice dictation, swipe typing, and autocomplete** through Gboard (or any soft keyboard) silently fail — the text never reaches the Windows host.

This fork adds a custom `InputConnection` that intercepts those IME events and routes them to the remote host via the existing `NvConnection.sendUtf8Text()` protocol path.

| Input Method | Stock Moonlight | Moonlight Voice |
|---|---|---|
| Hardware keyboard | Works | Works |
| Gboard tap typing | Works | Works |
| Gboard voice dictation | **Broken** | **Fixed** |
| Gboard swipe typing | **Broken** | **Fixed** |
| Autocomplete suggestions | **Broken** | **Fixed** |

## Downloads

* [APK (Debug)](https://github.com/AFK-Automations/moonlight-voice/raw/master/builds/artemis.apk) — installs alongside stock Moonlight

## Changes from Upstream

Only **3 files** changed (117 lines added):

1. **`StreamInputConnection.java`** (new) — Custom `BaseInputConnection` that routes `commitText()` → `NvConnection.sendUtf8Text()`
2. **`StreamView.java`** — Override `onCreateInputConnection()` to return `StreamInputConnection`
3. **`Game.java`** — Pass `NvConnection` reference to `StreamView`

Application ID changed to `com.mncompute.artemis` so it installs alongside stock Moonlight.

## Related

* **[artemis-voice](https://github.com/AFK-Automations/artemis-voice)** — Same voice fix applied to [Artemis Android](https://github.com/ClassicOldSong/moonlight-android) (feature-rich Moonlight fork with custom buttons, portrait mode, etc.)
* **Upstream issue:** [moonlight-stream/moonlight-android#1496](https://github.com/moonlight-stream/moonlight-android/issues/1496)
* **Upstream PR:** [moonlight-stream/moonlight-android#1556](https://github.com/moonlight-stream/moonlight-android/pull/1556)

## Building

```bash
git clone --recurse-submodules https://github.com/AFK-Automations/moonlight-voice.git
cd moonlight-voice
# Create local.properties with sdk.dir and ndk.dir paths
./gradlew assembleNonRootDebug
```

## Credits

* [Moonlight](https://github.com/moonlight-stream/moonlight-android) by Cameron Gutman et al.
* Voice dictation fix by [Jon Gutierrez / MN Compute](https://github.com/AFK-Automations)

## License

GPL-3.0-or-later (same as upstream)
