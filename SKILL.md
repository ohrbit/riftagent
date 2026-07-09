---
name: riftagent
description: Generates ultra-high-quality, gacha-style item drops from a theme prompt (e.g. "melee weapon", "epic mercenary", "futuristic shinobi"), using tiered rarity rolls (Legendary/Mythic/Secret) and studio-quality image generation via Nano Banana. Use this skill when the user wants to generate randomized high-detail items, character concepts, or product-style visuals with rarity tiers and provenance tracking.
license: MIT
version: 1.0.0
author: OHRBIT
---

# RIFTAGENT

RIFTAGENT turns a short theme into a fully described, studio-quality item concept complete with rarity tier, detailed description, provenance record, and a ready-to-use image generation prompt.

## When to Use This Skill

Use RIFTAGENT when the user wants to:

- Generate a random high-detail item, weapon, character, companion, or location concept from a theme
- Produce gacha-style "rolls" with rarity tiers (Legendary, Mythic, Secret/Ultimate)
- Get a studio-quality, white-background product shot prompt for an image generator
- Track where a generated item came from (theme, tier, timestamp)
- Run single rolls or bulk (e.g. 10x) rolls from one or more themes

Do NOT use this skill for straightforward image generation requests unrelated to themed item drops, or for non-item creative writing tasks.

## Instructions

Follow these steps in order for every roll:

### 1. Resolve the Theme

Accept a theme from the user (e.g. `melee weapon`, `epic mercenary`, `best companion`, `island`, `futuristic shinobi`). If no theme is given, ask the user to pick one or offer 3-4 example themes.

### 2. Roll the Tier

Randomly select one tier using equal weighting across the three included tiers:

- Legendary (33%)
- Mythic (33%)
- Secret/Ultimate (33%)

Do not include common or rare tiers — this skill only produces high-value drops.

### 3. Generate the Item Description

Write a unique, high-detail description of an item matching the theme and rolled tier. Each generation must be different from prior rolls, even for the same theme. Include:

- Item name
- Tier-appropriate visual details (materials, effects, distinguishing features)
- A short flavor description (1-2 sentences)

### 4. Format the Image Prompt

Convert the item description into an image generation prompt for Nano Banana (or Nano Banana Pro), following this structure:

```
Studio product photography of [item name], [key visual details from step 3], 
presented on a plain white background, professional lighting, high detail, 
[tier]-quality finish, centered composition
```

### 5. Record Provenance

Attach a provenance record to the output:

```
theme: <theme>
tier: <rolled tier>
timestamp: <ISO 8601 timestamp>
roll_id: <unique identifier>
```

### 6. Return the Full Package

Present to the user:

- The item name and description
- The rolled tier
- The formatted Nano Banana prompt (ready to copy/paste or send via API)
- The provenance record

## Bulk Rolls

If the user requests multiple rolls (e.g. "10x pull"), repeat steps 2-5 independently for each roll and return all results as a list, each with its own provenance record. Do not reuse the same tier roll or item description across multiple pulls.

## Image Backend

Default rendering backend is Nano Banana / Nano Banana Pro via API. The skill produces a fully formatted prompt regardless of backend, so it can be swapped later for a local Stable Diffusion/ComfyUI instance or another image model without changing steps 1-5.

## Example

**User input:** "Roll a mythic-tier item for theme: epic mercenary"

**Output:**

```
Item: Duskfang Reaver
Tier: Mythic
Description: A curved obsidian blade etched with crimson runes that pulse 
faintly with residual battlefield energy. The mercenary who wields it is 
said to never lose a contract.

Image Prompt: Studio product photography of Duskfang Reaver, a curved 
obsidian blade with glowing crimson rune etchings and a worn leather-wrapped 
hilt, presented on a plain white background, professional lighting, high 
detail, mythic-quality finish, centered composition

Provenance:
theme: epic_mercenary
tier: mythic
timestamp: 2026-07-10T00:00:00Z
roll_id: rift-8f3a2c
```

## Notes

- Never repeat an exact item description across rolls, even for the same theme and tier
- Keep tier distribution truly random and equal-weighted per roll — do not let recent tier history bias future rolls
- If the user wants shared/community rolls, a free-daily-roll allowance, or premium tiers, these are platform-level features layered on top of this skill, not part of the core generation logic
