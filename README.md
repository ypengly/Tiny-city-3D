# Tiny City — A Day in a Tiny City

A single-file, self-contained 3D interactive miniature city built with Three.js.
Watch cars, pedestrians, traffic lights, weather, and a day/night cycle play out
across five districts (Downtown, Residential, Shopping, Restaurant Street, and
Central Park), in English, Khmer, Chinese, or Japanese.

## Files

- `index.html` — the entire app (HTML, CSS, and JS in one file). Just open it
  in a browser, no build step or server required.

## What was fixed

The file you sent had one real functional bug and a few small pieces of dead
code. Here's exactly what changed:

1. **"Restaurant Street" card did nothing when clicked (the main bug).**
   The district card had `data-preset="restaurant"`, but the camera-preset
   dropdown only listed `city / downtown / residential / shopping / park /
   night` — there was no `restaurant` option and no matching entry in the
   `cameraPresets` object. So clicking the card set the dropdown to a value
   that didn't exist and `flyTo("restaurant")` returned immediately without
   moving the camera. Fixed by adding a real `restaurant` camera preset
   (positioned over that district's actual location in the grid), a matching
   `<option>` in the dropdown, and translated labels for it in all four
   languages.

2. **Dead / unreachable code cleaned up:**
   - `scene.fog.density;` was a no-op statement (reading a property and
     discarding it) — removed.
   - The clock formatter had an `if (hh >= 24)` branch that could never run,
     since `hh` is always taken `% 24`. Simplified to a single AM/PM check.
   - An unused `isBuildingOpen()` helper and an unused `targetEmissive`
     variable in the night-lighting loop were left over from an earlier
     version of the logic and never used — removed.
   - A redundant, always-true/false `swapAxis` parameter on the street-lamp
     builder did the exact same thing in both branches — removed for clarity.

3. **Small robustness fix:** the ambient sound toggle now resumes the
   `AudioContext` if the browser created it in a suspended state (some mobile
   browsers do this even on a click), so sound reliably starts on first tap.

Everything else — the day/night cycle, weather toggle, language switcher,
car/pedestrian traffic, building info panel, camera flythroughs, and stats
counters — was already working and is untouched.

## How to use it

1. Unzip and open `index.html` in any modern desktop or mobile browser
   (Chrome, Safari, Firefox, Edge). An internet connection is needed the
   first time, since Three.js, GSAP, Tailwind, and the Google Fonts are
   loaded from public CDNs.
2. Drag to orbit, scroll/pinch to zoom.
3. Use the bottom time dock to scrub through the day, play/pause, and change
   speed.
4. Click a building to see its name, type, floor count, population, and
   hours.
5. Use the district cards or the dropdown to fly the camera to a specific
   part of the city.
6. Switch language from the top-right menu (🇬🇧 🇰🇭 🇨🇳 🇯🇵) — your choice is
   remembered on next visit via `localStorage`.

## Tech used

Three.js (r128) + OrbitControls, GSAP for camera easing, Tailwind CSS (via
CDN) for layout utilities, and Google Fonts (Unbounded, Inter, Noto Sans
Khmer/SC/JP) for typography.

## Known limitations

- Requires WebGL; very old devices/browsers without WebGL support won't
  render the scene (Tailwind/CSS UI will still load).
- All data (traffic, building stats, weather) is generated client-side for
  atmosphere — it isn't tied to a real place or real data.
