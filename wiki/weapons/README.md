# Weapons wiki assets

For the site pages /wiki/weapons, /wiki/weaponsmith and the home page class showcase. Made 2026-11-25 from the Desert Fortress UE 5.8 build (the real in-game gun meshes, incl. the new M16A4). Index: `manifest.json`. Licences: `LICENSES.md` (all 41 guns may be shown in 3D; attachments are pictures only).

| Path | What |
|---|---|
| `cards/<id>.png` | 1600x900 card, studio background (neutral slate gradient, soft shadow), master |
| `cards/<id>_800.webp` | 800x450 WebP of the card |
| `cards/alpha/<id>.png`, `<id>_800.png` | the same gun on a transparent background (1600x900 / 800x450), to composite on the site |
| `models/<id>.glb` | web GLB of the in-game mesh: <= 30k triangles, JPEG textures <= 2048 px (CC0 library guns 1024 px), -X muzzle (barrel tip at min X), +Y up, +Z = the gun's left side, metres, origin at the bounding box centre; attachment points in the manifest (`sockets`) |
| `attachments/<id>.png` | 800x450 transparent picture of each optic / muzzle / magazine; `<id>_card.webp` with the studio background |
| `stats.json`, `STATS.md` | per gun stats (EN + TR names), 0-100 bars with formulas and scale, DPS / TTK, attachment effects and per-gun deltas |
| `licences.json`, `LICENSES.md` | author, licence, URL, 3D allowed per gun and attachment, credit lines |
| `contact_sheet.jpg` | all 41 cards |

Cards: right side of the gun, muzzle to the right, a slight 3/4 view (20 deg toward the muzzle, 9 deg above), soft key + fill + two rim lights, rendered with Movie Render Queue (Lumen, hardware ray tracing, 8 spatial samples at 3200x1800), tone-mapped and framed from a coverage matte so every card has the same margins. Pistols and rifles fill the card the same way (cards are not to scale; `size_m` in the manifest has the real size).

Notes: AUG A3 and AUG HBAR are one model in the game (same card and GLB). Dual Berettas = two M9s. SSG 69 is a stand-in model (a Kar98k). The holo sight reticle of the AUG and TAR-21 is drawn by the HUD in the game, so it is not on the model. reddot / reflex / holo share one in-game model, s5 / s8 / s12 another, supp1 / supp2 another; hider, ext, quick and light have no model in the game, their pictures are CC-BY illustrations. Weapon XP unlock levels are null: the weapon XP system is not in the game build yet (planned levels in `stats.json`).

## Attachment points (`sockets` in manifest.json)

Per gun `sockets.optic`, `sockets.muzzle`, `sockets.mag` = [x, y, z] in metres in that gun's GLB space (null = no such point), plus `optic_rail_offset` and `sight_line.rear/front`. Taken from the UE weapon mesh sockets of the game build and moved exactly the way the GLB was built: `glb = [-(ue.y - c.y), ue.z - c.z, ue.x - c.x] / 100`, c = the UE mesh bounds centre (= the GLB bounding box centre).

- `muzzle`: UE socket `Muzzle` (muzzle flash point), the barrel tip at the bounding box min X. All 41 guns.
- `mag`: UE socket `Mag` (magazine bone pivot, top of the magazine in the magwell; DP-27: the pan on top). null on p90, m1014, m24, lever, mosin, revolver, ssg69 (no detachable magazine in the model).
- `optic`: where a scope's mount base sits: on the receiver / rail top under the in-game sight line (UE `Sight_Rear` -> `Sight_Front`), 25 % of the way from the rear sight to the front sight (mosin, ssg69, svd: over the receiver), height from the GLB surface (sight posts ignored). On carry handles / built-in sights (famas, g36, p90, m16a4, awm) it is the top of those. The game has no optic socket (attachments change stats only), so this point is derived. null on dp27 (no optic slot).
- `optic_rail_offset`: how far the in-game iron sight line runs above `optic` [m]; raise a scope base by it to keep the game's aim line and clear the iron sights, or ignore it for a low mount.
- `dual_beretta`: points of the upper pistol; the second pistol = the same points + `second_pistol_offset`.
