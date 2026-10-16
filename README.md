# Keiser M3 Bridge web app

A single-file phone page (`index.html`) for the Keiser M3 Bridge (Adafruit CLUE nRF52840). It connects over Web Bluetooth (Nordic UART Service) and lets you:

- **Ride**: see a large gear number, watts, cadence and resistance, plus status chips (pod, Zwift, calibrated) and warnings (saturated, off-path hold).
- **Calibrate**: run the 25-point gear calibration from the handlebars.
- **Console**: read the bridge's text output and send single-character commands.

Everything is inline: there is no build step and no network access beyond Bluetooth. It needs the bridge running the v2 firmware, which sends the status lines this page reads; with older firmware only the Console tab works.

## Which browser

| Device | Browser |
|---|---|
| iPhone / iPad | **Bluefy** (free in the App Store). Safari and Chrome on iOS do not support Web Bluetooth. |
| Android | Chrome or Edge |
| Windows / macOS / Linux | Chrome or Edge |

Web Bluetooth only works on secure pages: `https://`, or `http://localhost` on a desktop. Host `index.html` on any static https host (for example GitHub Pages), then type or bookmark that URL in Bluefy.

## How to use

1. Power the bridge. Open the page and tap **Connect**. Pick "Keiser M3 Bridge" in the list.
   The page turns on the status stream automatically and keeps the screen awake where the browser allows it.
2. **Ride** tab: mount the phone and ride. If it says "Not calibrated", go to Calibrate first.
   An amber gear means the bridge is holding the last good gear because the reading is off-path.
3. **Calibrate** tab: tap **Start calibration** and follow the prompt for each of the 25 points:
   - Point 1: push the gear lever all the way down.
   - Points 2 to 24: move the lever up until the console first shows the next gear, then stop pedalling.
   - Point 25: push the gear lever all the way up.

   With **Auto-capture** on, each point is taken once the ring fills (about 2 seconds held steady). Turn it off to use **CAPTURE** yourself. **UNDO** goes back one point. **CANCEL** needs a second tap to confirm.
   At the end you get a result card with the path span and the smallest step between gears. A smallest step under 1.5 µT is flagged as marginal.
4. **Console** tab: shows the bridge's text lines. Quick buttons send `h` (help), `p` (print calibration) and `d` (debug stream, chatty). The text box sends any single character. "Show machine lines" also shows the raw S/C/K/A/U lines.

If the connection drops, tap **Reconnect**.

## Demo mode (no hardware)

Add `?demo=1` to the URL, for example `index.html?demo=1`. A built-in simulator stands in for the bridge, feeding protocol lines through the same parser as Bluetooth. It cycles gear, watts and cadence, and runs a full calibration (ending with span 146.0 µT, smallest step 3.8 µT at gear 6).

Use `?demo=1&uncal=1` to start in the uncalibrated state.
