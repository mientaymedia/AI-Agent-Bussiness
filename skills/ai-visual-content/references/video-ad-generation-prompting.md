# Video Ad Generation Prompting Guide

Prompt structures for generating short-form video ads and UGC-style clips from text or image prompts.

## Video Generation Approaches

| Approach | When to Use | Input Needed |
|---|---|---|
| Text-to-video | No existing product image, fully synthetic concept | Detailed text prompt only |
| Image-to-video | You already have a clean product hero image and want motion | Static image + motion prompt |
| Talking avatar / UGC | Need a "person speaking" testimonial or demo without filming | Script + persona description (+ reference face if tool supports it) |

**Default recommendation:** generate the hero image first with `image-generation-prompting.md`, then animate it (image-to-video). This gives more control over product appearance than pure text-to-video, and avoids re-rolling the whole clip if only the product looks wrong.

## 1. Product Demo Video (15-30s)

```
Animate this product scene: [describe starting frame/product]. Motion:
[camera slowly pushes in / product rotates 180° / hand picks up product
and turns it]. Duration: [15-30]s. Pacing: [slow and premium / fast and
energetic]. End frame: [close-up on product label / logo reveal].
No text overlay (added separately). Lighting stays consistent throughout.
```

## 2. UGC-Style Unboxing/Testimonial Clip

```
[Persona: age range, gender, general style] in [setting] [unboxing /
holding / demonstrating] [product]. Camera style: handheld phone-camera
feel, slight natural shake, casual framing (not studio-perfect). Duration:
[15-30]s. Tone: [excited / calm and trustworthy / curious]. Natural
pauses and imperfect framing — avoid overly polished commercial feel.
```

**Why imperfection matters here:** UGC-style ads outperform polished ads specifically because they read as authentic. A prompt that produces studio-perfect output defeats the format's purpose — deliberately request "phone-camera feel" and "slight shake."

## 3. Image-to-Video Animation (from an existing static hero shot)

```
Animate the attached product image with: [subtle motion type — gentle
zoom / product rotation / background element movement (steam, light
flare, fabric flutter)]. Keep product identical to source image, no
distortion. Duration: [5-10]s loopable clip.
```

## 4. Multi-Scene Ad Sequence (for 15-30s full ad structure)

Break into 3 scenes and generate/prompt each separately, then assemble in `video-batch-editor`:

```
Scene 1 (0-5s, HOOK): [Problem visual — e.g. person frustrated with old
method]. Fast cut, close-up on frustration/pain point.

Scene 2 (5-20s, SOLUTION): [Product in use, clear demonstration of the
core benefit]. Medium pacing, clean product visibility.

Scene 3 (20-30s, RESULT/CTA): [Satisfied outcome — person happy, product
hero shot, or result visual]. Slower pacing, room at bottom for CTA text
overlay.
```

## Camera & Motion Vocabulary (Use Precisely — Vague Terms Produce Inconsistent Output)

- **Push in / push out**: camera moves toward/away from subject
- **Pan left/right**: camera rotates horizontally on fixed point
- **Orbit**: camera circles around the subject (good for 360° product views)
- **Static with subject motion**: camera fixed, only the subject/product moves
- **Handheld**: slight natural shake, for authenticity (UGC style)

## Common Failure Modes and Fixes

| Problem | Fix |
|---|---|
| Product morphs/distorts mid-clip | Use image-to-video from a locked reference image instead of pure text-to-video |
| Motion looks unnaturally smooth/robotic | Add "natural, slightly imperfect motion, subtle handheld feel" |
| Face/hands glitch during movement | Shorten clip duration, reduce motion complexity, or regenerate that scene only |
| Video looks generic/stock-footage-like | Add specific Vietnamese-market context details (setting, props, wardrobe) instead of generic descriptors |
| Audio doesn't match generated motion | Generate video silent, add music/voiceover/SFX as a separate layer in `video-batch-editor` |

## Handoff Checklist

Before sending output to `video-batch-editor`:
- [ ] Each scene clip is under the target duration (trim in editor, don't rely on generation length being exact)
- [ ] No baked-in text/captions — added separately for easier localization/edits
- [ ] Consistent product appearance verified across all scenes if multi-scene
- [ ] Aspect ratio noted (batch editor will handle multi-platform export, but generate in the highest-resolution/widest ratio available as source)
