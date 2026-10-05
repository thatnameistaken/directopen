# Direct Open

**Open apps, settings and device features directly from your browser.**

Direct Open is a single-page collection of app shortcuts, deep links, Android intents and browser utilities for Android, Samsung, Xiaomi, Windows and iOS. It is designed to support kiosk breakout testing, FRP bypass workflows, device recovery and troubleshooting by bringing useful launch links and diagnostics together in one place.

Link behaviour depends on the device, installed apps, browser and operating system. Direct Open does not itself remove FRP, unlock a device or override system permissions.

## Features

- **Platform shortcuts:** launch apps, system settings and device features, with alternative launch methods where available.
- **Automatic tab selection:** detects Android, Samsung, Xiaomi, Windows and iOS from browser information. Tabs can also be selected manually.
- **Windows settings browser:** grouped settings links, app launchers and local folder links.
- **Browser information:** browser and device readings, live date and time, time zone, optional public IP lookup and extension detection based on visible page markers.
- **Custom links:** save web addresses, search text, app links and JavaScript snippets in the current browser.
- **API tests:** try supported browser capabilities, including camera, microphone, screen capture, clipboard, sensors, fullscreen, Bluetooth, USB, serial, HID and NFC.
- **Local utilities:** build Android intents, inspect URLs, convert text, format JSON, calculate file checksums and test touch input or display colours.

## Getting started

1. Open the in a browser on the device you want to use.
2. Check the selected platform tab, or choose another manually.
3. Select a launch variant if one is available, then press **Open**.
4. Expand **Info** or **Help / compatibility** if a link does not open.

The page contains its own HTML, CSS and JavaScript. It needs no build step, package installation or backend.

For broader browser API support, serve the page over HTTPS.

## Tabs

| Tab | Purpose |
| --- | --- |
| Android | General Android apps, settings and intents |
| Samsung | Samsung apps and device shortcuts |
| Xiaomi | Xiaomi / MIUI apps and device shortcuts |
| Windows | Settings categories, app links and local folders |
| iOS | App URL schemes for iPhone and iPad |
| Browser info | Browser readings, clock, IP lookup and extension scan |
| Links | Custom links stored in this browser |
| API tests | Interactive browser capability tests |
| Utilities | Local text, link, file and device tools |

### Device detection

The initial tab is selected using the user agent, platform and touch information. On Android, supported User-Agent Client Hints can also provide a model name when the user agent hides it.

Samsung and Xiaomi detection requires an identifiable brand, browser or model marker. Unidentified Android devices use the Android tab. Other unidentified platforms also start on Android, and users can choose a tab manually. A later model lookup does not change the tab after the user has interacted with the page.

## Utilities

| Utility | What it does |
| --- | --- |
| Android intent builder | Builds an intent from a package, action, data URI and optional HTTPS fallback; lets you copy or open the result |
| URL inspector | Displays the scheme, host, path, fragment and query parameters without opening the URL |
| Text encoder / decoder | URL encoding and decoding, UTF-8 Base64 conversion, character counts, byte counts and line counts |
| JSON formatter | Validates, formats or compacts JSON |
| File checksum | Calculates SHA-256 locally for files up to 100 MiB |
| Touch / pointer tester | Shows contact positions, pointer types, pressure and the maximum simultaneous contact count observed |
| Display colour tester | Displays white, black, red, green and blue, with optional fullscreen |
| Scratchpad | Temporary notes with copy, download and clear controls |

Scratchpad notes are cleared when you leave the Utilities tab. Download notes you want to keep.

## Privacy and storage

- Utility inputs and checksum files are processed locally; they are not uploaded by these tools.
- Custom links are stored in browser local storage. Clearing site data removes them, and they are not synced across devices.
- Camera and microphone tests provide local previews or readings. The microphone level test does not record audio.
- Permission-based tests run when their buttons are pressed, subject to browser permission rules.
- Public IP lookup contacts `ipwho.is`, falling back to `api.ipify.org`, only when requested. These services see your public IP address.
- Browser speech recognition may use an external service, depending on the browser.
- Opening web links, app links or running a saved JavaScript snippet can have its own effects. Only run snippets you understand and trust.

## Compatibility

- Apps must be installed, and their URL schemes or activities must be available on the device.
- Browsers may block Android intents, system settings links and local `file://` links.
- iOS does not provide a supported general Settings link for websites; unofficial schemes may do nothing.
- Windows settings destinations vary between releases. Local folder links depend on browser policy.
- Browser API availability varies by platform, browser, secure context, hardware and permissions. An available API does not guarantee a successful request.
- The extension scan uses page changes and exposed markers. It cannot enumerate every installed extension.
- Direct Open can request that an app opens, but cannot confirm that the launch succeeded.

## Customising shortcuts

Edit `Direct-Open.html` in a text editor. Platform shortcuts are defined in `data` and extended with helper functions. A shortcut has a title and one or more launch variants:

```js
{
  title: 'Wi-Fi settings',
  icon: 'wifi',
  category: 'System',
  desc: 'Open wireless network preferences.',
  variants: [
    {
      name: 'Standard Android',
      url: 'intent:#Intent;action=android.settings.WIFI_SETTINGS;end',
      note: 'Device-dependent; the browser may block this intent.'
    }
  ]
}
```

Windows settings entries are grouped in `categories`. Browser diagnostics, API tests and utilities are rendered by `renderBrowser()`, `renderApis()` and `renderUtilities()` respectively. Initial platform selection is handled by `detectPlatform()`.

When changing a link, check it on the target device and include a compatibility note. Test tab switching and the mobile layout after UI changes.
