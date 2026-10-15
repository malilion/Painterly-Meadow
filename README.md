# 風之草原 · Painterly Meadow

**English** | [繁體中文](README.zh-TW.md)

A nature-wandering sandbox in a hand-painted animation style, rendered with NPR (non-photorealistic) storybook shading.
You play a little straw-hatted spirit who runs and jumps across wind-swept meadows and glides down hillsides holding a leaf. Along the way you can shake trees for branches, tap rocks for stone, pick flowers, build huts and stone lanterns, sculpt your own hills and ponds, and hunt for star fragments scattered across the world.

The whole world is generated from continuous Simplex noise: soft hills, irregular lakeshores, rounded tree canopies, clouds and buildings — **no blocks or voxels**. Everything lives in a single `index.html`; there is no build step.

**▶ Play online:** https://malilion.github.io/Painterly-Meadow/

![Meadow and lake in daytime](docs/screenshots/meadow-day.jpg)

---

## Quick start

### Open directly

Just open `index.html` in a browser. Three.js is loaded from the jsDelivr CDN through an import map, so **an internet connection is required**.

### Use a static server (recommended)

Some browsers restrict `file://` pages, so any static server is more reliable:

```bash
# Python
python3 -m http.server 8000

# or Node.js
npx serve .
```

Then open <http://localhost:8000/>.

### Choosing a terrain seed

Add `?seed=` to the URL to reproduce the same world:

```
http://localhost:8000/?seed=325863
```

- Numbers are used as the seed directly; any other text (e.g. `?seed=meadow`) is hashed into a number.
- Without a seed, a random one is generated.
- After each world rebuild the current seed is written back to the address bar, so copying the URL shares the same terrain.
- Backpack, buildings, terrain edits and star-fragment progress are saved **per seed** in the browser's `localStorage`; a shared URL does not carry that progress.

### Requirements

A browser with WebGL and ES modules (import maps), such as recent Chrome, Edge, Firefox or Safari.

---

## Feature demos

| Feature | Demo |
| --- | --- |
| Explore, run, jump, rotate camera | ![explore](docs/demo/explore-run-jump.gif) |
| Leaf glide | ![glide](docs/demo/leaf-glide.gif) |
| Gathering (branches / stone / flowers) | ![gather](docs/demo/gather.gif) |
| Build, rotate, demolish | ![build](docs/demo/build.gif) |
| Terrain sculpting (raise / dig / flatten) | ![sculpt](docs/demo/terrain-sculpt.gif) |
| Wind strength | ![wind](docs/demo/wind.gif) |
| Time of day (dawn to night) | ![time](docs/demo/time-of-day.gif) |
| Sunset color temperature | ![warmth](docs/demo/sunset-warmth.gif) |
| Inverted-hull outline toggle | ![outline](docs/demo/outline-toggle.gif) |
| Re-roll terrain seed | ![seed](docs/demo/seed-reseed.gif) |

---

## Screenshots

| Gathering: shake a tree for branches | Build mode: a green preview means you can place it |
| --- | --- |
| ![Gathering](docs/screenshots/gather.jpg) | ![Build mode](docs/screenshots/build.jpg) |

| Sculpting: raise hills, dig ponds | A star fragment at dusk |
| --- | --- |
| ![Sculpting](docs/screenshots/sculpt.jpg) | ![Star fragment](docs/screenshots/star.jpg) |

After dark, stone lanterns, the hut's round windows and star lamps light up:

![Village at night](docs/screenshots/village-dusk.jpg)

Gliding on a leaf from the lakeside:

![Leaf glide](docs/screenshots/leaf-glide.jpg)

> Screenshots use seed `325863`.

---

## Controls

### Movement and gathering

| Key | Action |
| --- | --- |
| `W` `A` `S` `D` / arrow keys | Move (relative to the camera) |
| `Shift` | Run |
| `Space` | Jump |
| Hold `Space` in the air | Open a leaf and glide |
| `E` | Gather the tree, bush, rock or flower in front of you |
| Mouse drag | Rotate camera |
| Mouse wheel | Zoom in / out |

### Build mode

| Key | Action |
| --- | --- |
| `B` | Toggle build mode |
| `1`–`9` | Select a building or terrain tool (also enters build mode from normal play) |
| Mouse move | Aim at the ground to place or sculpt |
| Left click / `F` | Place building |
| Hold left mouse / hold `F` | Sculpt terrain (with tools 7–9) |
| `R` | Rotate building 45° |
| `X` | Remove the building nearest the cursor (all materials refunded) |
| Right-drag | Rotate camera while sculpting |
| `Esc` | Leave build mode |

### Control panel (top left)

| Control | Effect |
| --- | --- |
| 🌬 Wind strength | Affects grass sway, petal and dandelion drift, cloud speed and how hard the wind pushes you while gliding |
| ☀ Time | 05:30–20:00; changes the sun path, sky gradient, fog and light color. The darker it gets, the brighter lanterns and stars glow |
| 🎨 Sunset warmth | Shifts the overall lighting toward cool (blue) or warm (orange) |
| 🌱 Terrain seed | Type a seed and press `Enter`, or press "🎲 Reroll" for a random new world |
| 🧹 Clear this world's save | Resets the backpack, buildings, terrain edits and star fragments for the current seed |
| ✏ Outline | Toggles the inverted-hull outline on the character, trees, rocks and buildings |

The "–" button collapses the panel; the backpack at the top right shows your current materials.

> Only keyboard and mouse are supported for now. On touch devices you can drag to rotate the camera, but you can't walk or build.

---

## Gameplay

### Wandering and gliding

- **Wandering**: the character smoothly hugs the rolling terrain, parts the grass as it walks, and is blocked by trunks, rocks and buildings. Slopes that are too steep can't be climbed, and a ring of mountains encloses the world.
- **Jumping**: jumps have input buffering and "coyote time", so you can still jump shortly after walking off a ledge. Landings play a squash-and-stretch animation.
- **Leaf glide**: hold Space while falling and the spirit raises a big leaf, drifting slowly down with the wind. Gliding has limited stamina (about 2.8 s) that refills on landing; a bar at the bottom of the screen shows what's left.
- **Wading**: in deep water the character floats, moves more slowly and can still jump.

### Gathering

When you get close to something gatherable, a prompt appears at the bottom of the screen; press `E` to gather.

| Target | Yield | Rule |
| --- | --- | --- |
| 🌳 Broadleaf / cedar tree | 🪵 Branch +2 | The tree shakes and drops leaves; the same tree can be shaken again after 20 s |
| 🌿 Bush | 🪵 Branch +1 | Can be searched again after 12 s |
| 🪨 Rock | 🪨 Stone +1 | Each rock can be tapped 3 times, shrinks when depleted, and recovers after 60 s |
| 🌸 Daisy, buttercup, cosmos, lavender | 🌸 Flower +1 | Picked flowers regrow after 45 s |

### Building

Press `B` to enter build mode and a toolbar appears at the bottom. The ground under the mouse shows a preview of the building: **green means it can be placed, red means it can't**, and the prompt explains why (too far, in water, too steep, blocked by a tree, not enough materials, etc.).

| # | Building | Cost | Notes |
| --- | --- | --- | --- |
| 1 | Wooden fence | 🪵3 | Chain segments together to enclose a little yard |
| 2 | Bench | 🪵4 | A log bench |
| 3 | Flower bed | 🪵2 🌸4 | Planted with little flowers that sway in the wind |
| 4 | Stone lantern | 🪨4 | Glows warmly after dark |
| 5 | Hut | 🪵10 🪨6 | A round thatched hut whose round windows light up at night |
| 6 | Star lamp | ✨1 🪨2 | A spinning star on top; needs one star fragment |

The hut needs fairly flat ground; smooth it first with the Flatten tool. Removing a building with `X` refunds all of its materials.

### Terrain sculpting

Tools 7–9 on the build toolbar are terrain brushes and cost nothing. Hold the left mouse button (or `F`) to paint; the circle on the ground shows the brush area.

| # | Tool | Effect |
| --- | --- | --- |
| 7 | ⛰️ Raise | Piles the ground up into a small hill |
| 8 | 🕳️ Dig | Digs down; below the water line it becomes a pond |
| 9 | 〰️ Flatten | Smooths the ground to the height at the center where you started |

After the terrain changes, grass, flowers, trees, buildings and star fragments re-settle onto the ground, the surface is recolored for the new slope (steep slopes expose rock), the water-depth map updates live, and grass and trees that end up underwater are hidden.

### Star fragments

Each world hides about 12 glowing star fragments; walk into one to add it to your backpack:

- **Hilltops**: on the highest point nearby — you'll have to climb.
- **Mid-air**: floating about 3 m above the ground — jump or glide over from somewhere high.
- **Over lakes**: hovering above a lake — swim out and jump, or glide from the shore.

Their glow ignores fog so you can spot them from afar, and they shine brighter at dusk and night. Collected fragments are used to build star lamps; the backpack shows how many you've found in this world.

### Saving

Your backpack, buildings, terrain edits and found star fragments are saved automatically to the browser's `localStorage` after every change, separately for each seed. Switching seeds saves the current world first, and returning to a seed loads its progress. "🧹 Clear this world's save" in the panel restores the world to its original state.

---

## Rendering techniques

### Terrain and lakes

- A custom seedable 2D Simplex noise with layered fbm forms rolling hills, with a ring of mountains around the edge.
- Each seed carves 2–3 lakes whose shore radius is perturbed with noise into irregular shapes; small stray hollows are filled so water only stays in the lakes.
- Character height sampling uses exactly the same interpolation as the mesh triangulation, so the character never floats or sinks.
- Ground color is a vertex-color blend by height, slope and noise (grass, flower fields, sandy shore, rock, lakebed), overlaid with a canvas-drawn watercolor brush texture.
- Normals are computed by finite differences; sculpting only recomputes heights, normals, colors and water depth inside the brush area, and other systems update only the affected region.

### Storybook toon shading

- The whole scene uses `MeshToonMaterial` with a custom canvas-drawn gradient map (three tones with slightly softened boundaries).
- The `HemisphereLight` sky and ground colors are tuned purple-blue, giving shadows a warm purple-blue tint instead of flat black.
- Normals of tree canopies and clouds are "inflated" toward their centers so they catch light like single soft masses of color.
- Lanterns, windows and stars share an emissive material and an additive glow sprite whose brightness follows the time of day.

### Inverted-hull outlines

- The vertex shader pushes vertices out along their normals and renders only back faces, producing a soft dark-brown outline.
- Tree and rock outline meshes share the body's `instanceMatrix`, so sway animation and scaling stay in sync with the outline.

### Wind-swept grass

- Tens of thousands of grass clumps and four kinds of flowers (daisy, buttercup, cosmos, lavender) are rendered with `InstancedMesh`, split into 8×8 chunks for frustum culling.
- The vertex shader is patched via `onBeforeCompile` to compute, in world space, waves of wind sweeping the meadow, fine jitter, and the character pushing grass aside.
- Grass normals point up so grass and ground are lit consistently; back-face normals aren't flipped in double-sided rendering, avoiding dark backsides.
- Picking a flower or flooding it just collapses that instance's matrix to a point — no need to rebuild the whole `InstancedMesh`.

### Animated water

- Vertices undulate with several sine waves.
- The fragment shader offsets noise coordinates along the wind to draw cartoon ripple lines, a hard-edged sun highlight and a Fresnel sky reflection.
- A depth map baked from the terrain height decides shallow vs. deep water color and drives breathing shore foam; digging a new pond rebakes it locally.

### Sky, clouds and particles

- The sky is a shader sphere with a gradient, sun glow and horizontal watercolor brush texture.
- About 14 volumetric clouds, each merged from several spheres with flattened bottoms, drift with the wind and cast moving shadows.
- The shadow camera follows the character so nearby shadows keep enough resolution.
- Petals use `InstancedMesh` and dandelion fluff uses `Points` with a canvas-drawn sprite; both drift with the wind.
- Gathering, building and collecting stars burst small particles from a shared `InstancedMesh`, with DOM floating text showing what you got.

### Time and color temperature

The timeline has seven palette keyframes (dawn, morning, noon, afternoon, dusk, sunset, night). Sky, sun, hemisphere light, fog and water colors are interpolated by time, then shifted cool or warm by the color-temperature slider.

---

## Project structure

```
.
├── index.html            # The whole game: HTML, CSS and JavaScript in one file
├── README.md             # English
├── README.zh-TW.md       # Traditional Chinese
├── LICENSE
├── .github/workflows/    # GitHub Pages deployment
└── docs/
    ├── demo/             # Feature demo GIFs
    └── screenshots/      # Screenshots used in the README
```

The JavaScript in `index.html` is organized into these sections:

| Section | Contents |
| --- | --- |
| 0. Globals and utilities | World size, state and backpack, RNG, Simplex noise |
| 1. Storybook textures | Toon gradient maps, brush textures, dandelion sprite, glow and emissive materials |
| 2. Shared shaders | Wind-field grass material, inverted-hull outline material, geometry helpers |
| 3. Renderer / scene / lights / sky | WebGLRenderer, hemisphere light, sun and shadows, sky sphere |
| 4. Terrain | Heightfield generation, lakes, local updates, sculpt brushes, save deltas |
| 5. Water | Depth-map baking (locally updatable), water shader |
| 6. Vegetation | Grass and flower generation, picking and regrowth, terrain fitting |
| 7. Trees / bushes / rocks | Models, gather nodes, colliders, sway and recovery animation |
| 8. Clouds | Volumetric cloud generation and drift |
| 9. Particles | Petals and dandelion fluff |
| 10. Character | Spirit model, movement, gliding, collision and animation |
| 11. Feedback | Burst particles, floating text, prompts |
| 12. Star fragments | Placement rules, floating animation, pickup detection |
| 13. Building and sculpting | Building models, toolbar, preview and placement, terrain sync, collision, gathering, saving |
| 14. Environment | Time-of-day palette, color temperature and night glow |
| 15. Camera / input | Third-person camera, keyboard and mouse |
| 16. World generation | Rebuild the whole world from a seed and load the save |
| 17. UI bindings | Control panel events |
| 18. Main loop | Per-frame update and render |

### Dependencies

- [Three.js](https://threejs.org/) r160 (including `BufferGeometryUtils`), loaded from a CDN
- Google Fonts: Space Grotesk, Noto Sans TC (falls back to system fonts if unavailable)

## License

Released under the [MIT License](LICENSE).
