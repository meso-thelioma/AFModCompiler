# AFModCompiler
The Acolyte Fight Mod Compiler. Used to compile Acolyte Fight mod projects into a single playable json file.

## Table of Contents

* [Usage](#usage)
* [Project Structure](#project-structure)
  * [Mod Info](#mod-info)
  * [Constants](#constants)
  * [Icons](#icons)
  * [Maps](#maps)
    * [Example](#map-example)
  * [Obstacles](#obstacles)
  * [Projectiles](#projectiles)
    * [Example](#projectile-example)
  * [Sounds](#sounds)
    * [Example](#sound-example)
  * [Spells](#spells)
    * [Example](#spell-example)
* [Parenting](#parenting)

## Usage
```
usage: AFModCompiler.py [-h] [--output, -o [OUTPUT_FILE]]
                        [--max-projectile-depth [MAX_PROJECTILE_DEPTH]]
                        [--max-parent-depth [MAX_PARENT_DEPTH]]
                        [Directory]

Process a mod project

positional arguments:
  Directory             Directory that will be processed

options:
  -h, --help            show this help message and exit
  --output, -o [OUTPUT_FILE]
                        Set the destination file that the mod will be compiled
                        to
  --max-projectile-depth [MAX_PROJECTILE_DEPTH]
                        Sets the maximum depth projectiles will generate in
                        the case of recursion or sufficiently long
                        projectile chains
  --max-parent-depth [MAX_PARENT_DEPTH]
                        Sets the maximum depth parents will be searched to
                        generate in the case of nested parent chains and
                        infinite loops
```

## Project Structure

A mod project is expected to follow a specific directory and file sturcture, although no singular file or directory is required.

The project directory name does not matter, but the files `mod.json` and all constants must have their appropriate names to be included. Objects in the other folders do not need to follow any particular naming scheme, but object names should be addressed consistently when it comes to parenting and templating. An example of a project directory and file structure is as follows:

```
project_directory/
├─ constants/
│  ├─ ai.js
│  ├─ audio.json
│  ├─ choices.json
│  ├─ hero.json
│  ├─ matchmaking.json
│  ├─ obstacle.json
│  ├─ tips.json
│  ├─ visuals.json
│  └─ world.json
├─ icons/
│  └─ thunderball.json
├─ maps/
│  └─ circle.json
├─ obstacles/
│  └─ mirror.json
├─ projectiles/
│  └─ fireball.json
├─ sounds/
│  ├─ fireball.json
│  └─ standard-hit.json
├─ spells/
│  └─ fireball.json
└─ mod.json
```

### Mod Info

Mod information is located in the `mod.json` file in the root directory. The file is expected to contain a single key-less dictionary value.

### Constants

Constants are located in the `constants` directory. There are 8 constant files:

* `ai.js` contains javascript code for the NPC's AI. Formatted as a standard Javascript file.
* `audio.json` contains global settings for the game's audio. Formatted as a key-less dictionary value.
* `choices.json` contains global settings for players' spell selection. Formatted as a key-less dictionary value.
* `hero.json` contains global settings for player characters. Formatted as a key-less dictionary value.
* `matchmaking.json` contains global settings for the game's matchmaking. Formatted as a key-less dictionary value.
* `obstacle.json` contains global settings for the game's obstacles. Formatted as a key-less dictionary value.
* `tips.json` contains randomized tips for the game. Formatted as a list.
* `visuals.json` contains global settings for the game's visuals. Formatted as a key-less dictionary value.
* `world.json` contains global settings for the game maps. Formatted as a key-less dictionary value.

Tips are the one file that expects a list `[]` rather than a dictionary `{}`. As it's how the game implements tips, tips in the `tips.json` file will replace all of the existing game's tips.

### Icons

Icons are located in the `icons` directory. Sub-directories are acceptable, but not important.

An icon is generated from a json file where the icon entry uses the file name without the file extension. Each icon file is expected to contain a single key-less dictionary value.

### Maps

Maps are located in the `maps` directory. Sub-directories are acceptable, but not important.

A map is generated from a json file where the map entry uses the file name without the file extension. Each map file is expected to contain a single key-less dictionary value.

#### Map Example

The following examples is the contents of the Mirrors map (as of the time of writing) that might be located in the file `project_directory/maps/mirrors.json`

```json
{
  "id": "mirrors",
  "color": "#41334d",
  "background": "#25192e",
  "obstacles": [
    {
      "type": "mirror",
      "numObstacles": 7,
      "layoutRadius": 0.22,
      "layoutAngleOffsetInRevs": 0,
      "numPoints": 4,
      "extent": 0.005,
      "orientationAngleOffsetInRevs": 0,
      "angularWidthInRevs": 0.05
    }
  ],
  "numPoints": 7
}
```

### Obstacles

Obstacles are located in the `obstacles` directory. Sub-directories are acceptable, but not important.

An obstacle is generated from a json file where the obstacle entry uses the file name without the file extension. Each obstacle file is expected to contain a single key-less dictionary value.

### Projectiles

AFModCompiler introduces the concept of Projectile Templating.

A projectile template string can be used in place of a `projectile`'s value. For clarity, I use `"ProjectileTemplate:fireball"` where projectileTemplate is the name of a projectile template file (without the file extension), but you may exclude `ProjectileTemplate:` and it should also function (untested).

```json
"projectile": "ProjectileTemplate:fireball"
```

These can be placed in place of a Spell's `projectile` field, or in place of a `spawn` behaviour's `projectile` field.

Projectile Templates are located in the `projectiles` directory. Sub-directories are acceptable, but not important.

A projectile is generated from a json file where the projectile template name uses the file name without the file extension. Each projectile file is expected to contain a single key-less dictionary value.

#### Projectile Example

The following example is the contents of the Fireball's projectile dictionary (as of the time this was written) that might be located in the file `project_directory/projectiles/fireball.json`

```json
{
  "density": 25,
  "radius": 0.003,
  "speed": 0.6,
  "maxTicks": 90,
  "damage": 16,
  "lifeSteal": 0.3,
  "categories": 2,
  "sound": "fireball",
  "soundHit": "standard",
  "color": "#f80",
  "renderers": [
    {
      "type": "bloom",
      "radius": 0.045
    },
    {
      "type": "projectile",
      "ticks": 30,
      "smoke": 0.05
    },
    {
      "type": "ray",
      "ticks": 30
    },
    {
      "type": "strike",
      "ticks": 30,
      "flash": true,
      "numParticles": 5
    }
  ]
}
```

### Sounds

Sounds are located in the `sounds` directory. Sub-directories are acceptable, but not important.

A sound is generated from a json file where the sound entry uses the file name without the file extension. Each sound file is expected to contain a single key-less dictionary value.

#### Sound Example

The following example is the contents of the vanilla Fireball's sound effect (as of the time this was written) that might be located in the file `project_directory/sounds/fireball.json`

```json
{
  "id": "fireball",
  "sustain": [
    {
      "stopTime": 1.5,
      "attack": 0.25,
      "decay": 0.25,
      "highPass": 432,
      "lowPass": 438,
      "wave": "brown-noise"
    }
  ]
}
```

### Spells

Spells are located in the `spells` directory. Sub-directories are acceptable, but not important.

A spell is generated from a json file where the spell entry uses the file name without the file extension. Each spell file is expected to contain a single key-less dictionary value.

#### Spell Example

The following example is the contents of the vanilla Fireball (as of this writing) that might be located in the file `project_directory/spells/fireball.json`

```json
{
  "id": "fireball",
  "description": "Quick cooldown and packs a punch. Good old trusty fireball.",
  "action": "projectile",
  "color": "#f80",
  "icon": "thunderball",
  "maxAngleDiffInRevs": 0.01,
  "cooldown": 90,
  "throttle": true,
  "projectile": {
    "density": 25,
    "radius": 0.003,
    "speed": 0.6,
    "maxTicks": 90,
    "damage": 16,
    "lifeSteal": 0.3,
    "categories": 2,
    "sound": "fireball",
    "soundHit": "standard",
    "color": "#f80",
    "renderers": [
      {
        "type": "bloom",
        "radius": 0.045
      },
      {
        "type": "projectile",
        "ticks": 30,
        "smoke": 0.05
      },
      {
        "type": "ray",
        "ticks": 30
      },
      {
        "type": "strike",
        "ticks": 30,
        "flash": true,
        "numParticles": 5
      }
    ]
  }
}
```

Note that the dictionary value of `"projectile"` could be replaced with a [Projectile Template](#projectiles) string (e.g. `"projectile": "ProjectileTemplate:fireball.json"`) to condense things here.

## Parenting

AFModCompiler checks for the key `"basedOn"` and uses its value (formatted as a file name without the extension) to generate a base object that the settings then overwrite.

One thing to keep in mind is that you can only replace values in the root, so attempting to add something to a nested dictionary or list will not work.
