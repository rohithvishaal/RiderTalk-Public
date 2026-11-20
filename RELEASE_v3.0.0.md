# RiderTalk v3.0.0 — Production Release Notes 🖤🏍️

> **Release Tag:** `v3.0.0` (also tagged `v3.0-stable`)  
> **Release Name:** `RiderTalk v3.0.0: Tailnet Wide-Area Mesh, 24/7 Background Radar, Jitter-Free Music Sharing & Android 11+ Support`  
> **Version Code:** `3000` (`3.0.0+3000`)  
> **Date:** November 20, 2026  
> **Package Name:** `com.ridertalk.app`  
> **Target SDK:** `35` (Android 15)  
> **Minimum SDK:** `30` (Android 11)  

---

## 🚀 What's New in v3.0.0 Stable

### 📱 1. Android 11 (API Level 30) Backwards Compatibility Expansion
* **Lowered Minimum SDK:** Changed `minSdk` from 31 down to 30, bringing complete RiderTalk intercom capabilities to devices running Android 11 (API 30) and newer.
* **Dual-Tier Audio Routing Engine:** 
  * On Android 12+ (API 31–35), leverages modern `AudioManager.setCommunicationDevice` and `AudioDeviceInfo` communication routing.
  * On Android 11 (API 30), automatically falls back to output device querying via `AudioManager.getDevices(AudioManager.GET_DEVICES_OUTPUTS)` paired with classic `AudioManager.startBluetoothSco()` and `setSpeakerphoneOn()` control.
* **Resilient Audio Playback Capture on Android 11:**
  * Fixed transient `AudioRecord.read()` read errors (`ERROR_INVALID_OPERATION`) during initial audio stream warmup or between paused tracks, preventing premature capture teardown and UI dropouts.
  * Added fallback foreground promotion in `RiderIntercomService` to ensure `FOREGROUND_SERVICE_TYPE_MEDIA_PROJECTION` binds reliably across OEM ROMs.
  * Added a persistent 1.5s network heartbeat timer on the local sharer in `CallViewModel` and `GroupCallViewModel`, preventing silence eviction when media playback is paused or loading.
* **Full Feature Parity:** 24/7 sticky background radar service (`FOREGROUND_SERVICE_TYPE_CONNECTED_DEVICE`), Tailnet Wide-Area Mesh, high-priority heads-up calling notifications, and `AudioPlaybackCaptureConfiguration` music sharing function seamlessly on Android 11.

### 🌐 2. Tailnet Wide-Area Mesh Intercom & MagicDNS
* **Multi-Bike Convoys over Tailscale:** Intercom now spans beyond local Wi-Fi to support remote multi-bike groups riding across highways, cities, and backcountry roads connected via 4G/5G mobile data or Starlink satellite links over encrypted WireGuard mesh VPN.
* **MagicDNS Resolution & Quad-100 Fallback:** Resolves Tailscale hostnames (`rider.tailnet-name.ts.net`) and IPv4 `100.x.y.z` addresses. Bypasses Android Private DNS (DNS-over-TLS) restrictions using direct UDP lookups against Tailscale's Quad-100 DNS server (`100.100.100.100:53`).
* **Unicast Keep-Alive Pinning:** Tailscale interfaces suppress standard subnet broadcast traffic (`255.255.255.255`). RiderTalk automatically pins saved Tailnet peers, sending 1.5-second unicast UDP beacons so remote riders remain permanently discoverable.
* **Dual-Interface Discovery:** Devices active on both local Wi-Fi and Tailscale appear as separate, independently callable entries (`${id}_tailscale` and `${id}_wifi`), letting riders choose between ultra-low-latency local LAN or mesh fallback.
* **Reciprocal Device Saving & Live Online Probing:** Connecting once over Tailnet saves both peers to each other's persistent roster. Background 20-second TCP HTTP probes update real-time `ONLINE` status badges and trigger heads-up alerts when buddies arrive on the mesh.

### 🛡️ 3. Persistent 24/7 Background Radar & "Stop Radar" Exit Control
* **Permanent Background Radar Service:** Runs a sticky Android foreground service (`FOREGROUND_SERVICE_TYPE_CONNECTED_DEVICE`) backed by a cached `FlutterEngine` (`CACHED_ENGINE_ID = "ridertalk_engine"`) and `PowerManager.PARTIAL_WAKE_LOCK`. Sockets and background listeners stay active when the phone is locked or riding in a pocket.
* **Lockscreen Heads-Up Call Alert:** High-priority system **Heads-Up Notification** (`NotificationCompat.CallStyle.forIncomingCall`) wakes the display with caller identity and prominent **"Answer"** (green) and **"Decline"** (red) action buttons for 1-tap answering with riding gloves without unlocking the screen.
* **"Stop Radar" Notification Action:** Added a dedicated **"Stop Radar"** action directly on the persistent notification card. Tapping it safely stops the foreground service, removes the notification, destroys the cached FlutterEngine, and terminates the app process completely (`exitProcess(0)`).
* **Branded App Notification Icons:** Replaced the generic telephone handset icon across all system notifications with a crisp 24dp vector silhouette of the RiderTalk helmet (`ic_notification.xml`) tinted with Signal Green (`#00E676`), complemented by full-resolution launcher icon bitmaps in the notification drawer via `setLargeIcon`.

### 🎵 4. Jitter-Free Single-App Music Sharing & Dynamic Ducking
* **Jitter Buffer Expansion & Starvation Recovery:** Upgraded the listener audio pipeline with a 40-frame jitter buffer (~800ms) with a 20-frame soft cap (~400ms), 4-frame prebuffering (80ms), and 3-timeout starvation tolerance (240ms), eliminating audio stutter and choppiness over both local Wi-Fi and Tailnet connections.
* **Single-App Audio Capture on Android 14+:** Restored the dedicated single-app audio capture selector via `MediaProjection` and native `AudioPlaybackCaptureConfiguration`, allowing riders to stream Spotify, Apple Music, or YouTube Music directly into the intercom without system noise.
* **Mutual Music Sharing Token Mutex:** Single-sharer token mutex (`mus_tok_*`) with 4.5s heartbeat eviction prevents glare and simultaneous music collisions while allowing seamless handover between riders.
* **Intelligent Voice Ducking:** Automatically drops shared music volume to 20% within 150ms when either rider speaks, holds for 800ms, and smoothly recovers to 100% once conversation pauses.

### 🎨 5. Interactive UI Prototype Studio (`feature/ui-ux-overhaul`)
* Fully interactive Flutter Web and Desktop prototype studio (`lib/prototype_main.dart` and `lib/ui_prototype/`) comparing **Tarmac Stealth**, **Amber Dash**, and **Nordic Ice** design directions with live interactive screens for Home Radar, IP & Subnets Hotspot Hub, Call Ringing, Active Intercom & Music Cockpit, and Group Intercom.

---

## 📦 Release Assets & Checksums

| Asset Name | Target Architecture | File Size | SHA-256 Checksum |
| :--- | :--- | :--- | :--- |
| **`app-release.apk`** *(Universal)* | All Android Devices (`arm64-v8a`, `armeabi-v7a`, `x86_64`) | 96.8 MB | `7E46AA8C205146BCDCDBD426CC2357CD417AB095296A8F641B4DC0644E2BBF16` |
| **`app-arm64-v8a-release.apk`** | Modern 64-bit ARM Phones (Snapdragon, Tensor, Dimensity) | 36.7 MB | `A04C68AEBB7AB1213F76CCF68A78C438F37BCDF789AA460D0CED6183A0C92C24` |
| **`app-armeabi-v7a-release.apk`** | 32-bit ARM Legacy Devices | 28.5 MB | `FBCCB522888C5B9B45CB290C6F03D36E952610789A9A7569B671945223BF7B25` |
| **`app-x86_64-release.apk`** | 64-bit x86 Devices & Android Emulators | 40.3 MB | `16BB6F6A5545E39ED72EB3CB49B800CA4049C26951C06790D173B7246FAD16BD` |

---

## 🧪 Hardware & Test Verification

- **Automated Tests:** **865/865** unit, widget, and integration tests passing (`flutter test` 100% green).
- **Static Analysis:** `flutter analyze` passed with 0 issues.
- **Physical Test Devices:**
  - **Samsung Galaxy S24 Ultra (`SM-S928B`, Android 14/15, One UI 6.1):** Verified Tailnet mesh discovery, background lockscreen heads-up calling, branded helmet notification icons, "Stop Radar" instant termination, and jitter-free music sharing.
  - **Motorola Moto G32 (`devon`, Android 13):** Verified mutual 1:1 intercom, simultaneous Wi-Fi & Tailnet peer entries, low-latency Opus audio routing to Bluetooth helmet headset, and 4:1 stereo IMA-ADPCM music reception.
  - **Android 11 Emulated / Physical Target (API 30):** Verified package installation (`sdkVersion: 30`), classic Bluetooth SCO fallback routing, and persistent background radar service execution.

---

## 📄 License & Terms

RiderTalk is licensed under the **RiderTalk Non-Commercial & Personal Use License**.  
See [LICENSE](LICENSE) for complete terms.
