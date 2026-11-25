# Weapon stats (Desert Fortress UE)

Generated 2026-11-25 from the UE project (origin/main bb11cac): `DT_Weapons`, `DT_Optics`, `DT_Muzzles`, `DT_Magazines`, `DA_CombatTuning`, `DA_MovementTuning` and the gunsmith maths of `UTRArmory::ComputeMods` / `ComputeStats`. Machine-readable: `stats.json` (names EN + TR, every raw value, bars, attachment deltas per gun).

Soldier HP 100. Walk 4.7 m/s x the gun's move multiplier, sprint x1.5, aimed x0.62.

## Formulas

- Shot interval i = 60 / RPM for every trigger type. Burst guns have no extra pause between bursts.
- Damage per trigger = damage x pellets (the twin Dual Berettas count 2 pellets: two traces, one round). Head = x headshot multiplier.
- **Burst DPS** = damage per trigger / i. **Sustained DPS** = mag x damage per trigger / ((mag - 1) x i + reload). Same as `assets/dps/DPS.md`.
- TTK = (shots to kill - 1) x i, vs 100 HP, every shot hits, inside the full-damage range.
- Falloff past the full-damage range R: mul = max(0.55, 1 - (d - R) / (2.5 R) x 0.45): x0.82 at 2R, floor x0.55 from 3.5R.
- ADS time (to 90 %) = ln(10) / 14 x (1 + max(0, 1 - MoveSpeedMul) x 2.5) x optic (1 + 0.05 (zoom - 1); big scopes 1.2 + 0.015 zoom).
- Mobility % = MoveSpeedMul x mag move mul x (0.5 + 0.5 x muzzle sprint mul) x 100.
- Recoil = sqrt(pitch^2 + yaw^2) degrees per shot (before the aimed x0.72 / crouch / prone multipliers).

### Bars (0-100, higher is better)

- **general**: bar = clamp(round(10 + 90 t), 0, 100); t = (v - min) / (max - min) over the 41 guns without attachments; lower-is-better stats use 1 - t. min/max are in bar_scale, so a site can recompute bars live for a weaponsmith build (values past the scale clamp).
- **damage**: t(damage_per_trigger) = damage x pellets (twin guns: 2 traces = 2 pellets)
- **fire_rate**: t(rpm)
- **range**: t(range_full_m): distance with full damage
- **dps**: t(dps_burst)
- **control**: 1 - t(recoil_deg), recoil_deg = sqrt(pitch^2 + yaw^2) kick per shot
- **mobility**: t(mobility_pct), mobility_pct = MoveSpeedMul x magMoveMul x (0.5 + 0.5 x muzzleSprintMul) x 100
- **accuracy**: 0.6 (1 - t(hip_spread_deg)) + 0.4 (1 - t(ads_spread_deg))
- **handling**: 0.5 (1 - t(ads_time_s)) + 0.25 (1 - t(reload_s)) + 0.25 t(mobility_pct)

Scale (min, max over the 41 guns without attachments): damage_per_trigger 19..120, rpm 42..1100, range_full_m 8..120, dps_burst 71.2..451, recoil_deg 0.335..4.54, mobility_pct 82..100, hip_spread_deg 2.6..6, ads_spread_deg 0.03..2.6, ads_time_s 0.164..0.238, reload_s 1.5..4.2, mag 5..71

## Guns

| Gun | TR | Class | Modes | Dmg body/head | Pellets | RPM | Mag | Reload s | Range m | ADS s | Mob % | Recoil deg | Hip deg | TTK body/head s | DPS burst | DPS sust. | Damage | Rate | Range | Control | Accuracy | Handling | Mobility |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| AK-47 (`ak47`) | AK-47 | Assault | Auto/Semi | 32 / 64 | 1 | 600 | 30 | 2.5 | 40 | 0.19 | 94 | 0.88 | 3.4 | 0.300 / 0.100 | 320 | 177.8 | 22 | 57 | 36 | 88 | 84 | 69 | 70 |
| AUG A3 (`aug_a1`) | AUG A3 | Assault | Auto/Semi | 26 / 52 | 1 | 700 | 30 | 2.6 | 40 | 0.18 | 95 | 0.47 | 3 | 0.257 / 0.086 | 303.3 | 153.4 | 16 | 66 | 36 | 97 | 90 | 72 | 75 |
| DP-27 (`dp27`) | DP-27 | Assault | Auto/Semi | 34 / 68 | 1 | 550 | 47 | 4.2 | 40 | 0.22 | 86 | 0.69 | 3.6 | 0.218 / 0.109 | 311.7 | 173.4 | 23 | 53 | 36 | 92 | 80 | 25 | 30 |
| FAMAS (`famas`) | FAMAS | Assault | Auto/Semi | 24 / 48 | 1 | 950 | 25 | 2.4 | 40 | 0.18 | 95 | 0.58 | 3 | 0.253 / 0.126 | 380 | 153.2 | 14 | 87 | 36 | 95 | 90 | 73 | 75 |
| G36C (`g36`) | G36C | Assault | Auto/Semi | 25 / 50 | 1 | 750 | 30 | 2.3 | 40 | 0.18 | 95 | 0.45 | 3 | 0.240 / 0.080 | 312.5 | 162.3 | 15 | 70 | 36 | 98 | 90 | 74 | 75 |
| Galil (`galil`) | Galil | Assault | Auto/Semi | 28 / 56 | 1 | 650 | 35 | 2.6 | 40 | 0.18 | 95 | 0.69 | 3 | 0.277 / 0.092 | 303.3 | 170.8 | 18 | 62 | 36 | 92 | 90 | 72 | 75 |
| HK416 (`hk416`) | HK416 | Assault | Auto/Semi | 27 / 54 | 1 | 780 | 30 | 2.1 | 40 | 0.18 | 95 | 0.50 | 3 | 0.231 / 0.077 | 351 | 187 | 17 | 73 | 36 | 96 | 90 | 76 | 75 |
| M14 (`m14`) | M14 | Assault | Semi | 40 / 80 | 1 | 480 | 20 | 2.6 | 40 | 0.20 | 92 | 1.01 | 2.6 | 0.250 / 0.125 | 320 | 160.8 | 29 | 47 | 36 | 86 | 96 | 61 | 60 |
| M16A4 (`m16a4`) | M16A4 | Assault | Burst/Semix3 | 33 / 66 | 1 | 820 | 30 | 2.2 | 40 | 0.18 | 95 | 0.58 | 3 | 0.220 / 0.073 | 451 | 229.1 | 22 | 76 | 36 | 95 | 90 | 75 | 75 |
| M4A1 (`m4a1`) | M4A1 | Assault | Auto/Semi | 25 / 50 | 1 | 800 | 30 | 2.1 | 40 | 0.18 | 95 | 0.56 | 3 | 0.225 / 0.075 | 333.3 | 175.4 | 15 | 74 | 36 | 95 | 90 | 76 | 75 |
| SCAR-L (`scarl`) | SCAR-L | Assault | Auto/Semi | 29 / 58 | 1 | 625 | 30 | 2.3 | 40 | 0.18 | 95 | 0.66 | 3 | 0.288 / 0.096 | 302.1 | 171.1 | 19 | 60 | 36 | 93 | 90 | 74 | 75 |
| TAR-21 (`tar21`) | TAR-21 | Assault | Auto/Semi | 26 / 52 | 1 | 850 | 30 | 2.4 | 40 | 0.18 | 97 | 0.55 | 3 | 0.212 / 0.071 | 368.3 | 175.4 | 16 | 79 | 36 | 95 | 90 | 81 | 85 |
| Bordyukov SMG (`bordyukov`) | Bordyukov Hafif Makineli | Medic | Auto/Semi | 24 / 48 | 1 | 760 | 35 | 2.2 | 18 | 0.16 | 100 | 0.39 | 3.4 | 0.316 / 0.158 | 304 | 172 | 14 | 71 | 18 | 99 | 81 | 94 | 100 |
| M1014 (`m1014`) | M1014 | Medic | Semi | 12 / 18 | 8 | 230 | 7 | 3 | 8 | 0.16 | 100 | 2.26 | 4.5 | 0.261 / 0.000 | 368 | 147.2 | 79 | 26 | 10 | 59 | 34 | 88 | 100 |
| MAC-10 (`mac10`) | MAC-10 | Medic | Auto/Semi | 19 / 38 | 1 | 1100 | 30 | 1.8 | 18 | 0.16 | 100 | 0.60 | 3.8 | 0.273 / 0.109 | 348.3 | 168.5 | 10 | 100 | 18 | 94 | 75 | 98 | 100 |
| MP40 (`mp40`) | MP40 | Medic | Auto/Semi | 26 / 52 | 1 | 520 | 32 | 2.3 | 18 | 0.16 | 100 | 0.34 | 3.4 | 0.346 / 0.115 | 225.3 | 141.6 | 16 | 51 | 18 | 100 | 81 | 93 | 100 |
| MP5 (`mp5`) | MP5 | Medic | Auto/Semi | 23 / 46 | 1 | 800 | 30 | 2 | 18 | 0.16 | 100 | 0.41 | 3.4 | 0.300 / 0.150 | 306.7 | 165.3 | 14 | 74 | 18 | 98 | 81 | 96 | 100 |
| MP7A1 (`mp7`) | MP7A1 | Medic | Auto/Semi | 21 / 42 | 1 | 950 | 40 | 2.1 | 18 | 0.16 | 100 | 0.41 | 3.4 | 0.253 / 0.126 | 332.5 | 184.1 | 12 | 87 | 18 | 98 | 81 | 95 | 100 |
| P90 (`p90`) | P90 | Medic | Auto/Semi | 19 / 38 | 1 | 900 | 50 | 2.6 | 18 | 0.16 | 100 | 0.36 | 3.4 | 0.333 / 0.133 | 285 | 161.9 | 10 | 83 | 18 | 99 | 81 | 91 | 100 |
| PPS-43 (`pps43`) | PPS-43 | Medic | Auto/Semi | 26 / 52 | 1 | 650 | 35 | 2.3 | 18 | 0.16 | 100 | 0.44 | 3.4 | 0.277 / 0.092 | 281.7 | 167.3 | 16 | 62 | 18 | 98 | 81 | 93 | 100 |
| PPSh-41 (`ppsh41`) | PPSh-41 | Medic | Auto/Semi | 21 / 42 | 1 | 1000 | 71 | 3.4 | 18 | 0.18 | 97 | 0.50 | 3.7 | 0.240 / 0.120 | 350 | 196.2 | 12 | 91 | 18 | 96 | 77 | 73 | 85 |
| Spectre M4 (`spectre`) | Spectre M4 | Medic | Auto/Semi | 21 / 42 | 1 | 850 | 50 | 2.4 | 18 | 0.16 | 100 | 0.46 | 3.4 | 0.282 / 0.141 | 297.5 | 179.2 | 12 | 79 | 18 | 97 | 81 | 92 | 100 |
| UMP45 (`ump45`) | UMP45 | Medic | Auto/Semi | 29 / 58 | 1 | 600 | 25 | 2.1 | 18 | 0.16 | 100 | 0.49 | 3.4 | 0.300 / 0.100 | 290 | 161.1 | 19 | 57 | 18 | 97 | 81 | 95 | 100 |
| UZI (`uzi`) | UZI | Medic | Auto/Semi | 20 / 40 | 1 | 950 | 32 | 1.9 | 18 | 0.16 | 100 | 0.53 | 3.4 | 0.253 / 0.126 | 316.7 | 165.9 | 11 | 87 | 18 | 96 | 81 | 97 | 100 |
| Vector (`vector`) | Vector | Medic | Auto/Semi | 19 / 38 | 1 | 1100 | 25 | 1.9 | 18 | 0.16 | 100 | 0.38 | 3.4 | 0.273 / 0.109 | 348.3 | 148 | 10 | 100 | 18 | 99 | 81 | 97 | 100 |
| AUG HBAR (`aug_hbar`) | AUG HBAR | Sniper | Auto/Semi | 30 / 75 | 1 | 650 | 42 | 2.9 | 120 | 0.21 | 90 | 0.50 | 3.3 | 0.277 / 0.092 | 325 | 188.5 | 20 | 62 | 100 | 96 | 87 | 50 | 50 |
| AWM (`awm`) | AWM | Sniper | Bolt | 120 / 300 | 1 | 42 | 5 | 3.6 | 120 | 0.21 | 90 | 3.62 | 6 | 0.000 / 0.000 | 84 | 64.4 | 100 | 10 | 100 | 30 | 46 | 44 | 50 |
| Lever-Action Rifle (`lever`) | Manivelalı Tüfek | Sniper | Bolt | 72 / 180 | 1 | 80 | 6 | 3.6 | 120 | 0.21 | 90 | 2.43 | 6 | 0.750 / 0.000 | 96 | 58.8 | 57 | 13 | 100 | 55 | 46 | 44 | 50 |
| M24 (`m24`) | M24 | Sniper | Bolt | 98 / 245 | 1 | 48 | 5 | 3.2 | 120 | 0.21 | 90 | 3.03 | 6 | 1.250 / 0.000 | 78.4 | 59.8 | 80 | 11 | 100 | 42 | 46 | 48 | 50 |
| Barrett M82 (`m82`) | Barrett M82 | Sniper | Semi | 110 / 275 | 1 | 110 | 10 | 4 | 120 | 0.24 | 82 | 4.54 | 6 | 0.000 / 0.000 | 201.7 | 123.5 | 91 | 16 | 100 | 10 | 46 | 12 | 10 |
| Mosin (`mosin`) | Mosin | Sniper | Bolt | 95 / 237.5 | 1 | 45 | 5 | 3.4 | 120 | 0.21 | 90 | 3.44 | 6 | 1.333 / 0.000 | 71.2 | 54.4 | 78 | 10 | 100 | 34 | 46 | 46 | 50 |
| PSG-1 (`psg1`) | PSG-1 | Sniper | Semi | 62 / 155 | 1 | 190 | 5 | 3 | 120 | 0.21 | 90 | 2.02 | 6 | 0.316 / 0.000 | 196.3 | 72.7 | 48 | 23 | 100 | 64 | 46 | 49 | 50 |
| SSG 69 (`ssg69`) | SSG 69 | Sniper | Bolt | 90 / 225 | 1 | 50 | 5 | 3 | 120 | 0.21 | 90 | 3.23 | 6 | 1.200 / 0.000 | 75 | 57.7 | 73 | 11 | 100 | 38 | 46 | 49 | 50 |
| Dragunov SVD (`svd`) | Dragunov SVD | Sniper | Semi | 58 / 145 | 1 | 220 | 10 | 3 | 120 | 0.21 | 90 | 2.13 | 6 | 0.273 / 0.000 | 212.7 | 106.3 | 45 | 25 | 100 | 62 | 46 | 49 | 50 |
| Desert Eagle (`deagle`) | Desert Eagle | Sidearm | Semi | 55 / 110 | 1 | 180 | 7 | 2 | 20 | 0.16 | 100 | 2.45 | 2.6 | 0.333 / 0.000 | 165 | 96.2 | 42 | 22 | 20 | 55 | 95 | 96 | 100 |
| Dual Berettas (`dual_beretta`) | Çift Beretta | Sidearm | Semi | 24 / 48 | 2 (twin) | 380 | 30 | 2.6 | 20 | 0.16 | 100 | 1.46 | 3.2 | 0.316 / 0.158 | 304 | 200.6 | 36 | 39 | 20 | 76 | 85 | 91 | 100 |
| Glock 17 (`glock17`) | Glock 17 | Sidearm | Semi | 24 / 48 | 1 | 520 | 17 | 1.5 | 20 | 0.16 | 100 | 0.93 | 2.6 | 0.462 / 0.231 | 208 | 121.9 | 14 | 51 | 20 | 87 | 95 | 100 | 100 |
| M1911 (`m1911`) | M1911 | Sidearm | Semi | 34 / 68 | 1 | 400 | 7 | 1.7 | 20 | 0.16 | 100 | 1.43 | 2.6 | 0.300 / 0.150 | 226.7 | 91.5 | 23 | 40 | 20 | 77 | 95 | 98 | 100 |
| Beretta M9 (`m9`) | Beretta M9 | Sidearm | Semi | 27 / 54 | 1 | 460 | 15 | 1.6 | 20 | 0.16 | 100 | 1.03 | 2.6 | 0.391 / 0.130 | 207 | 118.2 | 17 | 46 | 20 | 85 | 95 | 99 | 100 |
| Revolver (`revolver`) | Altıpatlar | Sidearm | Semi | 60 / 120 | 1 | 150 | 6 | 2.6 | 20 | 0.16 | 100 | 2.24 | 2.6 | 0.400 / 0.000 | 150 | 78.3 | 47 | 19 | 20 | 59 | 95 | 91 | 100 |
| TEC-9 (`tec9`) | TEC-9 | Sidearm | Semi | 22 / 44 | 1 | 450 | 20 | 1.9 | 20 | 0.16 | 100 | 0.87 | 2.6 | 0.533 / 0.267 | 165 | 99.2 | 13 | 45 | 20 | 88 | 95 | 97 | 100 |

## Attachments

Every muzzle device and magazine fits every gun. Optics: the gun's `AllowedMainOptics` (sniper rifles take the big scopes, the others the 1-4x sights). Weapon XP unlock levels: **not in the game yet** (the weapon XP system is not on origin/main); "planned" = the weaponxp branch, may change.

| Attachment | TR | Slot | Effects | Guns | XP level (planned) |
|---|---|---|---|---|---|
| Red Dot (`reddot`) | Kırmızı Nokta | optic | zoom 1, zoom_range [1, 2], big_scope False, illuminated True, ads_time_mul 1.0 | m4a1, ak47, famas, scarl, aug_a1, g36, m16a4, galil, tar21, m14, hk416, uzi, spectre, mp40, p90, mp5, mp7, vector, ump45, mac10, pps43, ppsh41, bordyukov, m1014, ssg69, psg1, m82, awm, m24, svd, aug_hbar, lever, mosin, m1911, tec9, glock17, m9, dual_beretta, deagle, revolver | Any L2 |
| Reflex Sight (`reflex`) | Refleks Nişangâh | optic | zoom 1, zoom_range [1, 1.75], big_scope False, illuminated True, ads_time_mul 1.0 | m4a1, ak47, famas, scarl, aug_a1, g36, m16a4, galil, tar21, m14, hk416, uzi, spectre, mp40, p90, mp5, mp7, vector, ump45, mac10, pps43, ppsh41, bordyukov, m1014, ssg69, psg1, m82, awm, m24, svd, aug_hbar, lever, mosin, m1911, tec9, glock17, m9, dual_beretta, deagle, revolver | Any L10 |
| Holo Sight (`holo`) | Holografik Nişangâh | optic | zoom 2, zoom_range [1.5, 3], big_scope False, illuminated True, ads_time_mul 1.05 | m4a1, ak47, famas, scarl, aug_a1, g36, m16a4, galil, tar21, m14, hk416, uzi, spectre, mp40, p90, mp5, mp7, vector, ump45, mac10, pps43, ppsh41, bordyukov, m1014, ssg69, psg1, m82, awm, m24, svd, aug_hbar, lever, mosin | Any L6 |
| Prism Optic (`prism`) | Prizma Optik | optic | zoom 3, zoom_range [2, 4], big_scope False, illuminated True, ads_time_mul 1.1 | m4a1, ak47, famas, scarl, aug_a1, g36, m16a4, galil, tar21, m14, hk416, uzi, spectre, mp40, p90, mp5, mp7, vector, ump45, mac10, pps43, ppsh41, bordyukov, m1014, ssg69, psg1, m82, awm, m24, svd, aug_hbar, lever, mosin | Any L10, Sniper L10 |
| Marksman 5x (`s5`) | Nişancı 5x | optic | zoom 5, big_scope True, illuminated True, ads_time_mul 1.275 | ssg69, psg1, m82, awm, m24, svd, lever, mosin | Sniper L2 |
| Longview 8x (`s8`) | Uzak Görüş 8x | optic | zoom 8, big_scope True, illuminated True, ads_time_mul 1.32 | ssg69, psg1, m82, awm, m24, svd, lever, mosin | Sniper L6 |
| Summit 12x (`s12`) | Zirve 12x | optic | zoom 12, big_scope True, illuminated True, ads_time_mul 1.38 | ssg69, psg1, m82, awm, m24, svd, lever, mosin | Sniper L10 |
| Suppressor (`supp1`) | Susturucu | muzzle | shot_volume_mul 0.4, ai_hearing_mul 0.4, hip_spread_mul 1, sprint_mul 1, flash_mul 0.15, muzzle_light_mul 0.15, hides_scope_glint False | all 41 | Any L8 |
| Advanced Suppressor (`supp2`) | Gelişmiş Susturucu | muzzle | shot_volume_mul 0.15, ai_hearing_mul 0.15, hip_spread_mul 1.15, sprint_mul 0.9, flash_mul 0.15, muzzle_light_mul 0.15, hides_scope_glint False | all 41 | Any L12 |
| Flash Hider (`hider`) | Alev Gizleyici | muzzle | shot_volume_mul 1, ai_hearing_mul 1, hip_spread_mul 1, sprint_mul 1, flash_mul 0.4, muzzle_light_mul 0.4, hides_scope_glint True | all 41 | Any L5 |
| Extended Mag (`ext`) | Uzatılmış Şarjör | mag | mag_size x1.5, reload_mul 1, move_mul 1 | all 41 | Any L9 |
| Quick-Swap Mag (`quick`) | Hızlı Değişim Şarjörü | mag | mag_size x1, reload_mul 0.65, move_mul 1 | all 41 | Any L4 |
| Light Mag (`light`) | Hafif Şarjör | mag | mag_size max(ceil(mag / 2), mag - 5), reload_mul 1, move_mul 1.06 | all 41 | Any L13 |

## Attachment deltas per gun

`stats.json` -> `guns.<id>.attachment_deltas.<attachment>` holds every changed stat as `[without, with]` and the changed bars. Examples:

- **M4A1**: reddot: ; reflex: ; holo: ads_time_s 0.185->0.194, zoom 1.3->2; prism: ads_time_s 0.185->0.204, zoom 1.3->3; supp1: noise_m 75.0->30.0, shot_volume_mul 1.0->0.4, flash_mul 1.0->0.15; supp2: mobility_pct 95.0->90.2, sprint_mps 6.7->6.03, hip_spread_deg 3.0->3.45, noise_m 75.0->11.2, shot_volume_mul 1.0->0.15, flash_mul 1.0->0.15; hider: flash_mul 1.0->0.4, hides_scope_glint False->True; ext: mag 30->45, dps_sustained 175.4->208.3; quick: reload_s 2.1->1.37, dps_sustained 175.4->211.9; light: mag 30->25, dps_sustained 175.4->160.3, mobility_pct 95.0->100.7, walk_mps 4.46->4.73, sprint_mps 6.7->7.1, ads_walk_mps 2.77->2.93
- **AWM**: reddot: ; reflex: ; holo: ads_time_s 0.206->0.216, zoom 1.5->2; prism: ads_time_s 0.206->0.226, zoom 1.5->3; s5: ads_time_s 0.206->0.262, zoom 1.5->5; s8: ads_time_s 0.206->0.271, zoom 1.5->8; s12: ads_time_s 0.206->0.284, zoom 1.5->12; supp1: noise_m 120.0->48.0, shot_volume_mul 1.0->0.4, flash_mul 1.0->0.15; supp2: mobility_pct 90.0->85.5, sprint_mps 6.35->5.71, hip_spread_deg 6.0->6.9, noise_m 120.0->18.0, shot_volume_mul 1.0->0.15, flash_mul 1.0->0.15; hider: flash_mul 1.0->0.4, hides_scope_glint False->True; ext: mag 5->8, dps_sustained 64.4->70.6; quick: reload_s 3.6->2.34, dps_sustained 64.4->74.5; light: mag 5->3, dps_sustained 64.4->55.8, mobility_pct 90.0->95.4, walk_mps 4.23->4.48, sprint_mps 6.35->6.73, ads_walk_mps 2.62->2.78
- **MP5**: reddot: ; reflex: ; holo: ads_time_s 0.164->0.173, zoom 1.2->2; prism: ads_time_s 0.164->0.181, zoom 1.2->3; supp1: noise_m 55.0->22.0, shot_volume_mul 1.0->0.4, flash_mul 1.0->0.15; supp2: mobility_pct 100.0->95.0, sprint_mps 7.05->6.35, hip_spread_deg 3.4->3.91, noise_m 55.0->8.2, shot_volume_mul 1.0->0.15, flash_mul 1.0->0.15; hider: flash_mul 1.0->0.4, hides_scope_glint False->True; ext: mag 30->45, dps_sustained 165.3->195.3; quick: reload_s 2.0->1.3, dps_sustained 165.3->198.6; light: mag 30->25, dps_sustained 165.3->151.3, mobility_pct 100.0->106.0, walk_mps 4.7->4.98, sprint_mps 7.05->7.47, ads_walk_mps 2.91->3.09
