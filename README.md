# Hacker Squad — Warehouse Championship

https://mochiyaki.github.io/app7/
Demo link ☝️


A voxel crowd-brawler that runs in the browser. Pick one of four fighters, walk into a warehouse turned tournament
arena, knock out **1000 hackers** and the **four top hackers** who come out to stop you, and take the title.

One stage, no story mode, four difficulty levels. Plain ES modules on three.js, a deterministic fixed 60 Hz simulation,
no build step.

## Run

ES modules don't load from `file://`, so serve the folder over http:

```sh
node serve.mjs            # http://localhost:8000
# or
python3 -m http.server 8000
```

Needs a WebGL2 browser. On a phone, hold it in landscape: touch controls appear on their own.

URL switches: `?go=story&char=ana` (or `go=free`) skips the menus straight into a battle; `?enemies=200` sets the crowd
size; `?hq` forces the full quality tier on a touch device.

## The fighters

| | Weapons | Plays like | Overclock |
| --- | --- | --- | --- |
| **Adam** — a boy in a black T-shirt and jeans | Microphone (on its stand) and a drone | Long reach, wide sweeps; screams and drone strafes hit at range | **Sonic Boom** — three screams, a drone strafe, the mic drop |
| **Ana** — a girl in a white sailor-style dress | Cellphone (on a selfie stick) and a laptop | The fastest hands: short-gap strings, dash cancels, spinning moves | **Viral Storm** — six dash cuts, a spinning storm, the burst |
| **Brian** — a man in a cap and a blue T-shirt | A cart and a camera | Slow and heavy; the cart flattens a rank, the camera's flash staggers a lane | **Rush Hour** — three flashes, a cart ride through the crowd, the unload |
| **Connector** — an oval jelly monster: three hairs, big eyes, a large mouth, orange balls for hands | Itself | Hops, spins, belly bumps; **copies** Adam's scream and Brian's flash, **clones** itself into three | **Giga Connect** — blows itself up to 100× its size: giant hops, a giant spin, the belly flop |

## The stage

| Round | Hall | Goal | Boss |
| --- | --- | --- | --- |
| 1 | Loading Dock | 200 knock-outs | **PHISH** — lure casts, then a spam flood |
| 2 | Server Aisles | 450 knock-outs | **TROJAN** — shield charges, shock waves, calls his squads |
| Semi-final | Mainframe Core | 700 knock-outs | **RANSOM** — payload drops, a lockdown, carpet drops |
| Final | Mainframe Core | 1000 knock-outs | **ROOT**, the champion — four phases: blade rushes, three forks of himself, a blackout, the mask comes off |

Every boss area attack is telegraphed by a red disc filling up on the floor: walk out of it, roll through it, or jump
the travelling shock rings. Clearing a round heals you. Your squad (the white hoodies) fights alongside you.

**Practice** is the endless arena in the Mainframe Core: waves keep coming and you can't be knocked out.

## Difficulty

| Level | What changes |
| --- | --- |
| Easy | Half damage, long wind-ups, one attacker at a time, weaker bosses, bigger heals |
| Normal | The baseline |
| Hard | Tougher hackers and bosses, ×1.5 damage, shorter wind-ups, smaller heals |
| Extremely Hard | ×1.9 damage, three attackers at once, bosses shrug off light hits |

## Controls

| Action | Keys | Gamepad | Touch |
| --- | --- | --- | --- |
| Move (camera-relative) | WASD / arrow keys | left stick | floating stick (left half) |
| Attack | J / left click | X □ | ATK |
| Charge (mid-combo: a finisher) | K / right click | Y △ | CHG |
| Jump | Space | A × | JMP |
| Dodge | L / Shift | R1 R2 | DDG |
| Overclock (a gauge segment full) | I | B ○ | OC (lit when ready) |
| Camera | mouse (click the field to lock it) / Q E | right stick | drag on the right half |
| Recenter / face the nearest boss | R | L1 L2 | — |
| Pause | Esc | Start | II |

Tap attack for the string (up to six hits); press charge after the 1st–5th hit for a different finisher each time, or
on its own for the fighter's signature move. Run for a moment and attack for a dash attack; attack or charge in the air
for air moves.

## Project layout

```
index.html            page, styles, screens
serve.mjs             local static server
src/main.js           boot, flow (title → select → loading → battle → result), the fixed-step loop
src/core/             input, events, RNG, voxel mesher helpers, difficulty
src/hero/             the hero: rig + IK, combo system, locomotion
src/chars/<id>/       a fighter: char.js (texts, portrait) · kit.js · moves.js · anims.js · model.js · musou.js (Overclock) · view.js (effects)
src/chars/shared/     clothes, body assembly, hair chains, locomotion carry, pooled effects
src/chars/officers/   enemy skins, lieutenant / boss models, boss behaviours
src/crowd/ src/combat/   the crowd (hundreds of fighters, squads, reinforcement waves) and hit resolution
src/story/            the stage script (championship.js), the director, the result screen
src/world/            the map engine (walk field, gates) and the warehouse (map.js + world.js)
src/ui/               title, select, loading, HUD, touch pad, menu helpers
src/camera/ src/post/ src/vfx/ src/audio/   camera, post-processing, combat effects, synthesised sound
bench/                headless bot: plays the whole stage in Node (no browser)
```

Adding a fighter = a folder in `src/chars/` plus one line in `src/chars/index.js`. Adding a stage = a data module in
`src/story/` plus one entry in `src/story/chapters.js` (and a map in `src/world/maps/` if it needs a new one).

## Test

```sh
node --import ./bench/register.mjs bench/run.mjs --char adam --diff normal
```

plays the championship start to finish with an autoplay bot in the Node sim and prints the timeline and the result
(`--char adam|ana|brian|connector`, `--diff easy|normal|hard|extreme`, `--style steady|careless`, `--quiet`). Node ≥ 22.

## Credits & license

Built on the engine of [voxel-musou](https://github.com/mike007jd/voxel-musou) by BubuAi (MIT, see [LICENSE](LICENSE)),
by way of the sheep-village and freedom-voxel reskins in this workspace; the Hacker Squad content is added on top
under the same license. [three.js](https://threejs.org/) r186 — MIT. All characters are original and fictional.
