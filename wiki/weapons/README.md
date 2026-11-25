# Weapons wiki assets

For the site pages /wiki/weapons, /wiki/weaponsmith and the home page class showcase. Made 2026-11-25 from the Desert Fortress UE 5.8 build (the real in-game gun meshes, incl. the new M16A4). Index: `manifest.json`. Licences: `LICENSES.md` (all 41 guns may be shown in 3D; attachments are pictures only).

| Path | What |
|---|---|
| `cards/<id>.png` | 1600x900 card, studio background (neutral slate gradient, soft shadow), master |
| `cards/<id>_800.webp` | 800x450 WebP of the card |
| `cards/alpha/<id>.png`, `<id>_800.png` | the same gun on a transparent background (1600x900 / 800x450), to composite on the site |
| `models/<id>.glb` | web GLB of the in-game mesh: <= 30k triangles, JPEG textures <= 2048 px (CC0 library guns 1024 px), +X muzzle, +Y up, metres, origin at the centre |
| `attachments/<id>.png` | 800x450 transparent picture of each optic / muzzle / magazine; `<id>_card.webp` with the studio background |
| `stats.json`, `STATS.md` | per gun stats (EN + TR names), 0-100 bars with formulas and scale, DPS / TTK, attachment effects and per-gun deltas |
| `licences.json`, `LICENSES.md` | author, licence, URL, 3D allowed per gun and attachment, credit lines |
| `contact_sheet.jpg` | all 41 cards |

Cards: right side of the gun, muzzle to the right, a slight 3/4 view (20 deg toward the muzzle, 9 deg above), soft key + fill + two rim lights, rendered with Movie Render Queue (Lumen, hardware ray tracing, 8 spatial samples at 3200x1800), tone-mapped and framed from a coverage matte so every card has the same margins. Pistols and rifles fill the card the same way (cards are not to scale; `size_m` in the manifest has the real size).

Notes: AUG A3 and AUG HBAR are one model in the game (same card and GLB). Dual Berettas = two M9s. SSG 69 is a stand-in model (a Kar98k). The holo sight reticle of the AUG and TAR-21 is drawn by the HUD in the game, so it is not on the model. reddot / reflex / holo share one in-game model, s5 / s8 / s12 another, supp1 / supp2 another; hider, ext, quick and light have no model in the game, their pictures are CC-BY illustrations. Weapon XP unlock levels are null: the weapon XP system is not in the game build yet (planned levels in `stats.json`).

