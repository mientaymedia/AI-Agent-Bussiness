# Image Generation Prompting Library

Ready-to-adapt prompt structures for AI product and marketing image generation. Fill in the bracketed fields.

## 1. Product Hero Shot

**Use for:** Sales page header, main product listing image

```
[Product name/description], centered composition, [surface/background type],
[lighting direction] light, [mood: soft/dramatic/bright], shallow depth of
field, no visible text, subtle natural shadow, [camera angle: eye-level /
slightly above / macro close-up], commercial product photography style,
high detail on [key material/texture feature].
```

**Example (skincare product):**
```
Matte glass serum bottle with minimalist white label, centered on a light
beige marble surface, soft natural light from upper-left, bright airy mood,
shallow depth of field, no visible text, subtle natural shadow, eye-level
angle, commercial product photography style, high detail on glass texture
and droplet condensation.
```

## 2. Lifestyle / In-Context Shot

**Use for:** Social ads, "how it fits into daily life" content

```
[Target persona description] using/holding [product] in [specific setting
relevant to Vietnamese buyer context], candid natural pose, [time of day]
lighting, phone-camera or DSLR feel (specify), authentic environment details
([background props that signal target lifestyle]), no posed/stock-photo
stiffness, slight motion blur optional for authenticity.
```

**Example (home fitness product):**
```
A Vietnamese woman in her 30s using a resistance band in a small apartment
living room, morning light through a window, casual home-wear, DSLR feel
with natural imperfection, background shows a yoga mat and water bottle,
candid mid-motion pose, not posed/stiff.
```

## 3. Before/After Comparison

**Use for:** Transformation-based offers (courses, services, physical results)

```
Split-frame comparison image: LEFT side shows [before state — specific,
concrete details], RIGHT side shows [after state — specific, concrete
result], consistent lighting and framing across both sides, clear visual
contrast, no exaggerated/unrealistic difference, subtle divider line,
realistic skin/material texture on both sides.
```

**Note:** For any before/after claiming a health, income, or performance result, keep the visual claim proportionate to what can be substantiated — an unrealistic visual gap undermines trust even if generated cleanly.

## 4. Ad Thumbnail / Scroll-Stopper

**Use for:** TikTok/Reels cover, paid ad creative first frame

```
[Product or persona] in a high-contrast, attention-grabbing composition,
bold color contrast between subject and background, [emotion to convey:
surprise/curiosity/satisfaction] visible in framing or expression, room
left at [top/bottom] for text overlay to be added separately, close crop,
mobile-vertical framing (9:16).
```

## 5. Multi-Variant Batch (A/B Testing Set)

**Use for:** Generating several ad variations to test simultaneously

```
Generate [N] variations of: [base product description]. Keep [product
framing/product itself] IDENTICAL across all variations. Vary only:
[background setting], [lighting mood], [persona/no persona]. Maintain
consistent brand color palette: [hex codes if known].
```

## Anti-"AI Smell" Modifiers (Append to Any Prompt)

Add these when output looks synthetic:

```
+ natural asymmetry, realistic skin/material texture, imperfect but
  natural lighting falloff, avoid overly smooth or plastic-looking
  surfaces, avoid perfect symmetry, subtle imperfections consistent
  with real photography
```

## Never Do This

- Never ask the model to render readable body text/paragraphs inside the image — accuracy fails, especially Vietnamese diacritics (ệ, ữ, ẫ). Generate the image clean, add text as a separate overlay layer (Canva/Figma/CSS).
- Never generate fake certification badges, review stars, or "verified" seals inside product images — this is a false-claims risk, not just a quality issue.
- Never reuse the exact same generated face across unrelated "different customer" testimonial images — pattern becomes visible on inspection and destroys trust when caught.

## Quality Checklist Before Publishing

- [ ] Zoomed in on hands/fingers if a person is present — no extra/missing digits
- [ ] Checked for warped/melted background elements (furniture, walls, text)
- [ ] Verified lighting direction is consistent across the whole frame
- [ ] Confirmed no unintended text/logos rendered in-image
- [ ] Compared against 2-3 real reference photos for realism check
- [ ] Generated at minimum 3 variations and selected the best, not the first
