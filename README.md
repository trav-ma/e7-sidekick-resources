# e7-sidekick-resources

## Overview

This repository contains community-entered, structured JSON data and a small subset of related images for Epic Seven.

## Data Files

### heroes.json

`heroUrl` is being deprecated in favor of `imageUrl`. 

**Key Properties:**
- `key` - Unique identifier (name, lowercased with underscores)
- `type` - Hero category/classification
  - currently only "Limited" and "Common" where Common refers to heroes always available in the game
- `releasedOn` - Release date (YYYY-MM-DD)
- `expiredOn` - Expiration date (YYYY-MM-DD)
  - Should always be empty in heroes.json

**Skills:**
- `skill1/2/3` - Skill descriptions
- `skill1/2/3IconUrl` - Skill icon image paths
- `skill1/2/3Soulburn` - Soulburn effect descriptions
- `skill1/2/3EE` - Equipment Enhancement arrays (example: it's possible skill 3 has 2 EEs associated to it)

**Base Stats:**
Please note that stats are increased by any passive skill effects, skill up effects and specialty change skill trees.

- `health`, `attack`, `defense`, `speed` - Base stat values as strings
- `dual` - Dual attack chance percentage
- `crit` - Critical hit chance percentage
- `cd` - Critical damage percentage
- `eff` - Effectiveness percentage
- `effres` - Effect resistance percentage

**Additional Stats:**
- `imprint` - Memory imprint bonuses (format: "stat,value,position")
  - Example: "Health,12.9%,rb" means +12.9% Health at right-bottom position. 
  - Order of positions is always l (left) t (top) r (right) b (bottom)
- `selfImprint` - Self-imprint bonus (format: "stat,value")
- `statEE` - Exclusive Equipment stat (format: "stat,value")

### history-heroes.json

**Structure:** Identical to heroes.json but contains archived versions of heroes before balance changes. Used to track how heroes have changed over time.

Can have multiple versions of the same hero. The key is `expiredOn` is the date of the balance patch. 

Exclusive equipments being added to a hero are considered as balance patch events.

### artifacts.json

**Key Properties:**
- `key` - Unique identifier  (name, lowercased with underscores)
- `type` - ("Common", "Limited")
  - Common refers to artifacts always available in the game
- `class` - "General" or specific hero class restriction
  - Artifacts locked to a specific hero will be shown as "Mage - Mercedes"
- `unit` - Hero associated with the artifact
- `releasedOn` - Release date (YYYY-MM-DD)
- `expiredOn` - Expiration date (YYYY-MM-DD)
  - Empty for active artifacts
  - Unlike heroes Artifacts have historical entries in the main artifacts.json (this will likel be split into "history-artifacts.json" very soon). This means that there will be multiple entries for the same artifact and Expiration date is the date of the balance patch where the artifact was changed

### buffs.json

**Key Properties:**
- `id` - Unique identifier (name, lowercased with underscores)
- `type` - "buff" or "debuff"

### hero-adjustments.json

Balance patch history tracking hero and artifact changes. Currently there's no "id" that links the entry to a hero or artifact but matching with the name is working fine for the time being. 

**Properties:**
- `patchDate` - Date of balance patch
- `entries` - Array of adjusted items
  - `type` - "Hero" or "Artifact"
  - `stars` - Rarity as string
  - `name` - Name of adjusted hero/artifact

Currently only storing balance patches back to "2024-09-12" and exclusive equipment additions back to "2024-11-22".

Older exclusive equipments worth adding:
- Wukong 2024-11-21
- Fairy 2024-11-21
- Spec Sez 2024-10-24
- Fire Hwa 2024-09-26
- Summer Iseria 2024-09-26

### Specialty Changes

Specialty Changes are a different beast. We want to account for all RUNE effects in the hero's skills. This is done by copying that data into the affected skill after a "Skill Tree: " point. For passive effects that are unrelated to any skills, those are copied into Skill2, even if that skill is not a passive skill. 

Example for Commander Lorina:
- "skill2": "Increases Effect Resistance by 50%. Increases Attack by 15.0% each time the caster attacks an enemy. Effect can only stack up to 5 times. Skill Tree: When Effect Resistance is 200% or more, damage suffered in one attack does not exceed 35% of max Health when attacked."

