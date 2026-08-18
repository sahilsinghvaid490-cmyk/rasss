# GunDrop — Fabric 1.21.11

Break dirt or grass, get a small chance at a realistic firearm. 17 guns across
7 categories (pistol, revolver, SMG, rifle, shotgun, sniper, rocket launcher),
each with its own magazine size, fire rate, damage, spread, and synthesized
gunshot sound. Bullets are real gravity-affected projectiles, not hitscan.

## Controls
- **Right-click (hold for automatics):** fire
- **R:** reload

## How to build (you have no local dev environment — use GitHub Actions)
1. Create a new **empty** GitHub repo (e.g. `gundrop-mod`).
2. Upload every file in this project to the repo root, preserving folders
   (drag-and-drop the whole extracted zip into GitHub's web uploader, or
   `git push` from any machine/Termux).
3. Go to the repo's **Actions** tab. The included workflow
   (`.github/workflows/build.yml`) runs automatically on push and produces a
   `gundrop-jar` artifact — download it, unzip it, and you'll have
   `gundrop-1.0.0.jar`.
4. Drop that jar into your server's `mods/` folder alongside **Fabric API
   0.141.4+1.21.11** and **Fabric Loader 0.18.1**, both fetched from Modrinth
   or fabricmc.net.

## Adding more guns (no Java needed)
Every gun is just a JSON file in `src/main/resources/guns/`. Copy an existing
one (e.g. `ak47.json`), rename it, tweak the numbers, add its filename to
`manifest.json`, and drop a matching icon PNG in
`src/main/resources/assets/gundrop/textures/item/<id>.png`. Push, and the
Actions build picks it up automatically.

## Known first-pass limitations (be honest about these)
- **Sounds are procedurally synthesized**, not real recordings — I don't have
  network access to fetch licensed gunshot audio, so I generated
  layered noise-burst "cracks/booms" per gun category in Python. They're
  distinct and punchy but won't sound like a movie. Swap the `.ogg` files in
  `assets/gundrop/sounds/` with real recordings any time (keep filenames).
- **Icons are simple generated pixel-art silhouettes**, not full 3D models.
  For real 3D gun models with proper first-person viewmodels you'd want to
  build them in Blockbench and drop the resulting item model JSON in
  `models/item/`.
- This is a **first compile pass against a brand-new Minecraft version**
  (1.21.11 shipped only recently and I can't compile locally in this sandbox
  — no route to Fabric's Maven from here). If the GitHub Actions build fails,
  copy the error log back to me and I'll patch the exact line; this is
  normal for cutting-edge MC versions and usually a one or two file fix.

## Balancing knobs
Loot chance is controlled in `GunDrop.java` (`NOTHING_WEIGHT`, currently
9800) against each gun's `dropWeight` in its JSON — lower `NOTHING_WEIGHT` or
raise `dropWeight` values to make drops more common.
