# Pokémon Extinction calculator

A fork of [Dynamic Calc](https://github.com/hzla/Dynamic-Calc-Decomps), set up for Pokémon Extinction.

## Regenerating the data

All data comes from the hack's decomp. After changing trainers, box pools, species or moves, run
this from the hack repo in a VS Code terminal. It exports, commits and pushes in one step:

```sh
tools/calc_export/update_calc.sh
```

To only export, without committing:

```sh
python3 tools/calc_export/export_calc.py --calc ../pokemon-extinction-calc
```

It writes `backups/extinction.js`. It needs devkitARM, because species and move data are read through the
same C preprocessor the ROM build uses.

## Running locally

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Then open <http://localhost:8000/>. Without a `?data=` parameter the page opens Pokémon Extinction.

## What is Pokémon Extinction-specific

| File | What it does |
|---|---|
| `backups/extinction.js` | Generated data: trainers (incl. the scripted Mossdeep bosses), box pools, species and their Mega/Primal forms, moves, per-trainer field info |
| `js/pokemon_extinction.js` | Box picker, set-list filters, per-trainer field effects, Trick Room and Swamp speed order, Mega/Primal toggle, box labels |
| `js/shared_controls.js` | Also refreshes the opposing sprite when the form changes |
| `css/pokemon_extinction.css` | Styles for the above |
| `js/initialize.js` | `Pokémon Extinction` game settings; hooks the set-list filters |
| `js/index_randoms_controls.js` | Hooks the Trick Room / Swamp speed colouring |
| `index.html` | Default URL, script and style tags |

## The hack's custom abilities

Implemented in `calc/mechanics/gen789.js`, next to the official ability each one copies:

| Ability | Effect |
|---|---|
| Dragonize | Normal moves become Dragon, x1.2 (the -ate family) |
| Kings Pride | In sun, special moves x1.3333 (Orichalcum Pulse for special) |
| Two Headed | Parental Bond: a second hit at 0.25 |
| Mega Sol | Weather Ball is always Fire (it also skips Solar Beam's charge, which the calc never modelled) |
| Piercing Drill | Physical moves ignore Protect |
| Instant Preparation | +1 Def, +1 Sp. Def and +1 Speed on entry, once per battle. Set those stages yourself; the calc never applies boosts on its own. |

## Shortcuts

Right click a box icon to add that Pokemon to the party view. That is the only one left: the alt+key
shortcuts and the other right-click actions were removed, because a hidden shortcut nobody needs is
just a surprise waiting to happen.

## Reading the calculator

- The marked damage roll is the one the AI plans with: your attacks are marked at the **maximum**
  roll, because the AI assumes your best roll against it, and its own attacks at the **median**.

## Not modelled yet

The data is right, but the damage engine has no behaviour for these:

- Draconic Terrain (Fairy moves deal half damage to grounded Pokemon)
- Torment Weather, Sea of Fire, Rainbow and Permanent Hazards are listed in the field notes only;
  none of them changes a damage roll
