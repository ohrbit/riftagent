# RIFTAGENT — Technical Specification (v1.0)

This document defines the full technical spec for the RIFTAGENT workflow: data structures, tier logic, prompt templates, and API contracts. Use this alongside `SKILL.md` (agent instructions) and `README.md` (project overview).

---

## 1. Overview

RIFTAGENT is a stateless orchestration pipeline with four logical stages:

```
Theme Input -> Tier Roll -> Item Generation -> Prompt Formatting -> Image Render -> Provenance Log
```

Each stage is independently implementable and testable. No stage depends on a specific LLM or image model vendor.

## 2. Tier System

### 2.1 Included Tiers

Only three tiers are used. No common/rare/uncommon tiers exist in this system.

| Tier | Weight | Description |
|---|---|---|
| Legendary | 33.3% | High-value baseline drop |
| Mythic | 33.3% | Mid-high value drop |
| Secret / Ultimate | 33.3% | Rarest narrative framing, same statistical weight |

### 2.2 Roll Algorithm

```python
import random

TIERS = ["legendary", "mythic", "secret"]
WEIGHTS = [1/3, 1/3, 1/3]

def roll_tier():
    return random.choices(TIERS, weights=WEIGHTS, k=1)[0]
```

Weights are configurable per deployment but ship as equal by default. Tier history must NOT influence future rolls (no pity system in v1.0).

## 3. Item Generation

### 3.1 Input Schema

```json
{
  "theme": "string, required",
  "tier": "enum[legendary, mythic, secret], required",
  "previous_items": "array[string], optional, for de-duplication"
}
```

### 3.2 LLM Generation Prompt Template

```
You are generating a unique item for a themed collection.

Theme: {theme}
Rarity tier: {tier}

Write a short, high-detail item description including:
- A distinct item name
- Visual details appropriate to a {tier}-tier item (materials, effects, craftsmanship)
- A 1-2 sentence flavor description

Do not repeat any of these previously generated items: {previous_items}

Output as JSON:
{{
  "name": "...",
  "visual_details": "...",
  "flavor_text": "..."
}}
```

### 3.3 Output Schema

```json
{
  "name": "string",
  "visual_details": "string",
  "flavor_text": "string"
}
```

## 4. Image Prompt Formatting

### 4.1 Template

```
Studio product photography of {name}, {visual_details}, presented on a 
plain white background, professional lighting, high detail, 
{tier}-quality finish, centered composition
```

### 4.2 Backend Contract

Any image backend must accept a single formatted string prompt and return an image asset (URL, base64, or binary). Default backend is Nano Banana / Nano Banana Pro.

```json
{
  "backend": "nano_banana",
  "prompt": "string",
  "output": "image_url | base64 | binary"
}
```

Swapping backends (e.g. to a local Stable Diffusion/ComfyUI instance) requires no changes to sections 1-3.

## 5. Provenance Record

Every roll produces a provenance record, regardless of backend or deployment context.

```json
{
  "roll_id": "uuid",
  "theme": "string",
  "tier": "legendary | mythic | secret",
  "timestamp": "ISO 8601 string",
  "item_name": "string",
  "backend": "string"
}
```

`roll_id` must be globally unique (UUID v4 recommended). Provenance records should be stored append-only for auditability, especially in paid-roll contexts.

## 6. Bulk Rolls

For multi-item pulls (e.g. 10x), each roll executes sections 2-5 independently. Results are returned as an array of full roll objects, each with its own provenance record. Themes may vary per roll if the caller supplies multiple themes.

```json
{
  "pulls": [
    { "theme": "...", "roll": { ... } },
    { "theme": "...", "roll": { ... } }
  ]
}
```

## 7. De-duplication

Implementations SHOULD track a rolling window of previously generated item names/descriptions per theme (recommended: last 50) and pass them into the generation prompt (section 3.2) to avoid repeats. This is a soft constraint enforced via prompt, not a hard uniqueness guarantee.

## 8. Configuration Reference

| Config key | Default | Description |
|---|---|---|
| `tier_weights` | equal (1/3 each) | Override rarity distribution |
| `image_backend` | `nano_banana` | Rendering backend identifier |
| `dedup_window` | 50 | Number of prior items considered for de-duplication |
| `bulk_pull_size` | 1 | Default number of rolls per request |

## 9. Non-Goals (v1.0)

- No pity/guarantee system across rolls
- No persistent user inventory/wallet logic (platform-level concern, out of scope)
- No payment processing (platform-level concern, out of scope)
- No built-in 3D model generation (see README "Optional Extensions" for hi3d integration pattern)

## 10. Versioning

This spec follows semantic versioning independent of the SKILL.md agent-instruction version. Breaking changes to schemas in sections 2-5 require a major version bump.

---

License: MIT. See `README.md` for project overview and `SKILL.md` for agent-facing usage instructions.
