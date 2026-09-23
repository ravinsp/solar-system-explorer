# Solar System Explorer

**This is an AI-generated project**

Live page: https://ravinsp.github.io/solar-system-explorer/

A 3D solar system simulator for primary school students. Fly from Earth out past the planets and moons to the Kuiper belt, the heliosphere, the Oort cloud and the whole Milky Way. You can speed time up, slow it down or run it backwards to watch everything move.

It's a single file, `index.html`, with no build step and no install.

## Running it

You can open `index.html` directly in a browser, or serve the folder:

```sh
python -m http.server 8000
# then open http://localhost:8000
```

An internet connection is needed. The page loads [three.js r128](https://threejs.org/) and its OrbitControls add-on from a CDN (cdnjs and jsdelivr), and the fonts from Google Fonts. All textures are drawn in code, so there are no image files.

It needs a browser with WebGL. Recent Chrome, Edge, Firefox and Safari all work, on desktop and tablet.

## What's in it

- **The Sun, 8 planets and 14 moons.** These are the biggest moons: Moon; Phobos, Deimos; Io, Europa, Ganymede, Callisto; Enceladus, Rhea, Titan; Miranda, Titania, Oberon; Triton.
  - Saturn and Uranus have rings, Jupiter has the Great Red Spot, and Earth has clouds.
- **Beyond the planets:**
  - the asteroid belt and the Kuiper belt;
  - the heliosphere (the Sun's bubble), with Voyager 1 and 2 just outside it;
  - the Oort cloud;
  - the Milky Way.
- **The Milky Way:** about 200,000 glowing particles make up a central bulge and bar, four spiral arms, the Orion Spur where the Sun lives, blue haze, pink star-forming regions and dark dust lanes.
- **Info cards** for every place. Each card has a fun fact, simple facts (distance, day length, year length, temperature, moons) and a size comparison with Earth.
- **Grand tour:** visits the Sun and each planet in turn, then zooms out to the galaxy.

## Controls

| Action | Mouse / keyboard | Touch |
| --- | --- | --- |
| Look around | Drag | Drag |
| Zoom | Scroll | Pinch |
| Pan | Right-drag | Two-finger drag |
| Visit a place | Click it, its name, or a button in the bottom bar | Tap |
| Fly | `W` `A` `S` `D` / arrow keys, `Q` / `E` for down / up, hold `Shift` to go faster | Fly pad (bottom left) |
| Play / pause | `Space` | ⏯ button |
| Close info card | `Esc` | ✕ button |

The time panel (bottom right) has these controls:
- **Backwards** and **Forwards** set the direction of time.
- **Today** jumps back to today's date.
- **− / +** step the speed from *1 second = 1 hour* up to *1 second = 10 million years*.

The top-right buttons are:
- **Orbits** shows or hides the orbit lines.
- **Names** shows or hides the labels.
- **Planets** returns to the planets view.
- **Grand tour** starts or stops the tour.

### Links to a place

Add a place's id to the end of the address to open the page there, for example `index.html#saturn`, `#oort` or `#milkyway`.

The ids are `sun`, the planet names (`mercury` … `neptune`), the moon names (`moon`, `io`, `titan`, …), `asteroids`, `kuiper`, `heliosphere`, `voyager1`, `voyager2`, `oort` and `milkyway`.

## How real is it?

**Real:**
- Orbit periods, spin periods and axis tilts. Venus and Uranus spin backwards or on their side, and Triton orbits backwards.
- Where the planets are today. They start at their positions for today's date, calculated from their mean longitudes at the year-2000 reference date. The orbits are drawn as circles.
- The tilt between the planets' orbits and the galaxy's disc, about 60°. It uses measured directions for the galactic centre and the galaxy's north pole.
- Directions in space for:
  - the heliosphere's nose, which points into the interstellar wind;
  - Voyager 1 and Voyager 2;
  - the Sun's path around the galaxy, which heads towards Cygnus.
- The Sun's trip around the galaxy, about 230 million years. It also bobs up and down through the disc, about every 64 million years.

**Simplified:**
- **Sizes and distances are squeezed** so everything fits on screen. The Sun's info card explains the real scale.
- Orbits are circles, not ellipses.
- At very high time speeds, planets, moons and spins are slowed down on screen so they don't flicker. They return to their exact positions when you slow down or press **Today**.
- The galaxy's spiral pattern turns as one rigid shape, a little slower than the Sun.
- Galaxy stars are decoration, not a real star map.

Moon counts in the cards say "over 90" (Jupiter), "over 270" (Saturn) and "over 25" (Uranus), because new moons keep being discovered.

## How the code is organised

Everything is in `index.html`, in this order:

1. **Styles:** colour tokens, the top bar, the planet bar, the time panel, the fly pad, the info card and the labels.
2. **Page layout:** loading screen, top bar, info card, fly pad, planet bar and time panel.
3. **Script:**
   - **Data:** `SUN` and `PLANETS`, with their moons, real periods, display sizes and card text.
   - **Textures:** noise-based canvas painters for rocky, banded, Earth and Sun surfaces, plus rings and clouds.
   - **Scene setup:** the renderer (using a logarithmic depth buffer, so a moon one unit away and the galaxy ten million units away draw correctly together), camera, controls and the background sky.
   - **Big-scale shaders:** soft glowing point clouds and see-through glowing bubbles.
   - **Beyond the planets:**
     - the belts, heliosphere, Oort cloud and galaxy;
     - the galaxy tilt (`galaxyTilt`);
     - the `REGIONS` list, which holds the card data for places beyond the planets;
     - `updateScales`, which fades each layer in and out with zoom distance.
   - **Time:** `advance()` moves everything forward by a number of days.
   - **Camera:** visiting, following and a flight that zooms smoothly across scales.
   - **Grand tour, UI** (cards, chips, labels, clock, controls) and **main loop.**

The Sun always stays at the centre of the scene. For the galaxy, the page moves the galaxy around the Sun rather than moving the Sun, which keeps the planets precise up close.

### Changing things

- **Add a moon:** add an entry to a planet's `moons` array in `PLANETS`.
- **Add a place beyond the planets:** add an entry to `REGIONS`.
  - `anchor` and `range` set where its label sits and at what zoom distance it shows.
  - `view` sets where the camera goes.
  - `chip: true` adds a button to the bottom bar.
- **Change the time speeds:** edit the `SPEEDS` list.
- **Change the galaxy's look:** edit `buildGalaxy()`. It sets particle counts, colours and sizes for the bulge, bar, disc, arms, haze and dust.
