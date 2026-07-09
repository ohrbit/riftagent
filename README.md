# riftagent
People thought we were joking. RIFTAGENT tears open a portal into the 4th dimension and materializes gacha pulls — legendary, mythic, and secret-tier items generated on the fly by an LLM, rendered as studio-quality product shots. Open-source agent skill, model-agnostic, ready to drop into any pipeline.

# RIFTAGENT — LLM-Powered Item Generation Workflow

> An open-source agent skill that transforms simple theme prompts into ultra-high-quality, gacha-style item drops using LLM-driven prompt orchestration and AI image generation.

Originated by **[eucdee / OHRBIT]** · Released under MIT License · Built for [agentskills.io](https://agentskills.io)

---

## What Is RIFTAGENT?

RIFTAGENT is a prompt-orchestration workflow that turns a short theme (e.g. "melee weapon," "epic mercenary," "futuristic shinobi," "island," "best companion") into a fully described, studio-quality item concept — complete with rarity tier, provenance, and an image-generation-ready prompt.

Instead of manually writing detailed prompts for every item, RIFTAGENT handles tier selection, item description, and prompt formatting automatically, then hands the result off to an image model (e.g. Nano Banana / Nano Banana Pro) for rendering as a clean, white-background product shot.

## Core Concept

Each "roll" generates one item drawn from a themed pool, with the following mechanics:

- **Tier system**: Legendary, Mythic, and Secret/Ultimate drops only — no filler commons or rares
- **Equal-weighted rarity**: The three included tiers are weighted equally (roughly 33% each), keeping every roll exciting
- **Studio-quality output**: Every generated item is described as a professional product shot on a plain white background, ready for direct use in a shop, game asset library, or showcase
- **Provenance tracking**: Each item's origin (which theme, which roll, when) is recorded so buyers/users can see where an item came from
- **Multi-item and bulk pulls**: Supports both single rolls and 10x-style bulk pulls, consistent with familiar gacha UX patterns

## How It Works

1. **Theme input** — user selects or types a theme (melee weapon, mercenary, companion, island, shinobi, etc.)
2. **Tier roll** — the agent randomly selects a rarity tier using equal-weighted odds across Legendary / Mythic / Secret
3. **Item generation** — the LLM writes a unique, high-detail item description matching the theme and tier
4. **Prompt formatting** — the description is converted into an image-generation prompt optimized for studio product photography style
5. **Image delivery** — the formatted prompt is sent via API to an image model (Nano Banana / Nano Banana Pro) for rendering
6. **Provenance log** — the roll's theme, tier, timestamp, and prompt are stored alongside the generated image

```
Theme Input → Tier Roll (33/33/33) → LLM Item Description → 
Formatted Image Prompt → Image Generation API → Item + Provenance Record
```

## Example

**Input theme:** `epic mercenary`

**Generated output (example structure):**
- Tier: Mythic
- Item: A detailed weapon/character concept description written by the LLM, following the theme
- Image prompt: Formatted for a plain-white-background studio product shot
- Provenance: `theme=epic_mercenary, tier=mythic, timestamp=...`

## Optional Extensions

These are patterns the community has explored on top of the core workflow and can be adapted freely:

- **Shared roll pools** — all users draw from the same rotating pool of themes/results for a live, communal feel
- **Free + premium roll tiers** — e.g. a daily free-roll allowance alongside paid or subscription-based premium rolls
- **Feedback loop** — simple thumbs-up / quality rating on generated items to improve future prompt tuning
- **3D extension** — pairing the 2D studio-shot output with a 3D generation service (e.g. hi3d) to produce full 3D models from the same prompt
- **Central visual theme** — wrapping the roll mechanic in a themed UI element (e.g. a chest, portal, or rift) as the "pull" interaction point

## Image Backend (Pluggable, Not Vendor-Locked)

RIFTAGENT's core logic — theme parsing, tier rolls, item description, prompt formatting, and provenance — is pure orchestration and runs entirely on Hermes (or any compatible agent runtime) without needing a GPU or external image API.

Only the final rendering step needs an image backend, and that step is fully swappable:

| Backend | Setup | Notes |
|---|---|---|
| `nano_banana` | API key | Default, zero infra required, fastest to start |
| `local_sd` | Self-hosted Stable Diffusion / ComfyUI | Full in-house control, needs GPU-capable hardware |
| `hermes_native` | Hermes-integrated renderer | Keeps the entire pipeline in one system once hardware supports it |

```python
generate_item(theme="futuristic shinobi", image_backend="hermes_native")
```

This means RIFTAGENT never locks a team into a single image provider — teams with GPU infrastructure can run everything in-house, while smaller teams can start with an API-based renderer and switch later without rewriting the skill.

## Installation

```bash
git clone https://github.com/ohrbit/riftagent/riftagent.git
cd riftagent
```

RIFTAGENT is a prompt-and-orchestration skill, not a standalone application — it's designed to be dropped into any agent framework or LLM pipeline that supports:

- Text generation (for item description + tier logic)
- Image generation API access (e.g. Nano Banana / Nano Banana Pro)
- Simple state storage (for provenance logging)

## Usage as an Agent Skill

RIFTAGENT.md is written to be loaded directly as a skill definition:

```python
load_skill("riftagent")
generate_item(theme="futuristic shinobi")
```

The skill file defines the tier logic, prompt templates, and output formatting so any compatible agent can execute the full roll-to-image pipeline without additional configuration.

## Roadmap Ideas

- [ ] Multi-language theme support
- [ ] Configurable tier weighting (beyond equal 33/33/33)
- [ ] Native 3D-model output mode
- [x] Local/self-hosted rendering (in progress — running Hermes + local image backend)
- [ ] Marketplace/export hooks (STL, GLB, PNG bundle)

## Repository Contents

- `README.md` — project overview (this file)
- `SPEC.md` — full technical specification (schemas, prompt templates, API contracts)
- `SKILL.md` — agent-loadable instructions for agentskills.io

## License

Released under the **MIT License** — free to use, modify, and build upon, with attribution appreciated.

```
MIT License

Copyright (c) 2026 [Your Name / OHRBIT]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

## Credits

Originally conceived and prototyped by **eucdee** as part of the OHRBIT project ecosystem. Built with community collaboration in mind — if you extend or ship a product using this workflow, a credit link back to this repo is appreciated but not required under MIT.

---

*Made public so the idea can grow beyond one team. If you build something with RIFTAGENT, open an issue or PR — we'd love to see it.*

---

## Support OHRBIT

If RIFTAGENT was useful to you, consider supporting OHRBIT:

- **Donate Bitcoin** — `[YOUR_BTC_ADDRESS_HERE]`
- **Buy a lighter sleeve** from our Cults3D collection — [cults3d.com/@ohrbit](https://cults3d.com/@ohrbit)
- **Subscribe on Patreon** — [patreon.com/ohrbit](https://patreon.com/ohrbit)

### Socials

- X (Twitter) — [x.com/_ohrbit](https://x.com/_ohrbit)
- TikTok — [tiktok.com/@_ohrbit](https://www.tiktok.com/@_ohrbit)
- YouTube — [youtube.com/@ohrbit.digital](https://www.youtube.com/@ohrbit.digital)
- GitHub — [github.com/ohrbit](https://github.com/ohrbit)
- Cults3D — [cults3d.com/@ohrbit](https://cults3d.com/@ohrbit)
