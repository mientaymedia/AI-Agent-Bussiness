---
name: ai-visual-content
description: "AI-generated product images and video ads from text prompts — tool selection, prompting frameworks, and Vietnamese-market visual style guidance to replace manual photoshoots and stock footage."
---

# AI Visual Content Skill

## When to Use

Activate this skill when:
- Generating product images (hero shots, lifestyle context, variant mockups) without a physical photoshoot
- Creating short video ads or UGC-style product videos from a text/image prompt
- Producing ad creative variations fast for testing (multiple angles, backgrounds, models)
- Building a visual identity for a product launch without a design budget
- Turning a single product photo into multiple styled variations for different platforms

This skill does not replace `content-creation` (which handles scripts, captions, and calendars) — it generates the underlying visual/video asset that `content-creation` and `sales-page-blueprint` then use.

## Core Principle: Prompt First, Tool Second

The same prompt structure works across tools; only the execution differs. Always build the prompt fully before picking a tool — this keeps output consistent if you switch tools mid-project (e.g. platform outage, cost limit, quality mismatch).

## Tool Landscape

| Tool | Best For | Cost Model | Vietnamese-Market Note |
|---|---|---|---|
| Ideogram / Midjourney | Product hero images, stylized graphics, text-in-image (Vietnamese diacritics render inconsistently — verify) | Subscription | Strong for lifestyle/aspirational shots |
| Nano banana / Gemini image | Fast product mockups, background replacement, batch variations | Pay-per-image or subscription | Good for quick iteration at low cost |
| Kling / Runway / Sora | Short video ads, product demo motion, UGC-style talking clips | Credit-based, per-second | Kling has stronger Asian-face realism; test before committing budget |
| Magnific UGC toolkit (if available in your workspace) | End-to-end: image → video, camera angle changes, color grade, background removal, music/SFX layering | Credit-based per operation | Use when you need a full pipeline, not just one shot |

**Decision rule:** static product/ad image → image tool only. Anything with motion, voice, or a "creator talking to camera" feel → video tool, generate a clean still first if you need a consistent starting frame.

## Prompting Framework: S-C-O-P-E

Every visual generation prompt should specify:

- **S**ubject — exact product/person, materials, distinguishing details (avoid vague nouns; "matte black ceramic mug with white logo" not "a mug")
- **C**ontext — setting/background that signals the target buyer's world (a Vietnamese home kitchen, not a generic studio, if selling to VN home cooks)
- **O**ptics — camera angle, lens feel, lighting direction (e.g. "eye-level, soft window light from left, shallow depth of field")
- **P**urpose — what the image needs to do (stop-scroll thumbnail vs. trust-building hero shot vs. comparison chart background) — this changes composition choices
- **E**xclusions — explicitly state what to avoid (no visible brand logos of competitors, no text overlay if adding captions separately, no exaggerated claims in-image)

## Reference Files

- **[Image Generation Prompting](references/image-generation-prompting.md)**: Full prompt library for product hero shots, lifestyle/UGC-style images, before/after comparisons, and ad thumbnail variations — with the "AI smell" pitfalls to avoid
- **[Video Ad Generation Prompting](references/video-ad-generation-prompting.md)**: Prompt structures for 15-30s product demo videos, UGC-style talking-to-camera clips, and image-to-video animation of static product shots

## Avoiding the "AI Smell" (Critical for Vietnamese Market Trust)

Audiences increasingly recognize and distrust obviously-AI visuals. This directly undermines conversion — the same failure mode the competitor's own "kho template landing page" claims to fix.

**Tells that break trust:**
- Perfectly symmetrical faces/products with no natural imperfection
- Text rendered inside the image (garbled, especially Vietnamese diacritics)
- Overly saturated, uniform lighting with no real shadow logic
- Hands with incorrect finger count or unnatural poses
- Backgrounds that are technically coherent but "too clean" for the claimed context

**Fixes:**
- Add explicit imperfection cues to prompts: "slight asymmetry, natural skin texture, realistic shadow falloff"
- Never render Vietnamese text inside the generated image — add captions/text as a separate overlay step
- Reference a real photo (image-to-image / style reference) instead of pure text-to-image when brand consistency matters
- Always generate 3-5 variations and manually select — never ship the first output

## Usage Examples

**Product Hero Image**:
```
Generate a product hero image for [product]: matte packaging, centered on
a warm wood surface, soft natural light from upper-left, shallow depth of
field, no text overlay, slight natural shadow, Vietnamese home context.
```

**Ad Creative Batch**:
```
Generate 5 variations of a lifestyle ad image for [product] targeting
Vietnamese online shoppers 25-40: different backgrounds (home kitchen,
office desk, outdoor cafe), same product framing, consistent lighting mood.
```

**Video Ad from Prompt**:
```
Generate a 15-second UGC-style video: a Vietnamese woman in her late 20s
unboxing [product] in a bright apartment, natural hand movements, casual
phone-camera framing, ending on a close-up of the product label.
```

## Handoff to Other Skills

- Generated images/video feed into `content-creation` for platform-specific posts and `sales-page-blueprint` for hero sections
- Batch video output should be passed to `video-batch-editor` for cutting, captioning, and multi-aspect-ratio export before publishing
