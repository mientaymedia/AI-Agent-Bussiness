---
name: video-batch-editor
description: "Automated batch video editing — cut, sync music, burn captions, and apply effects across many videos in one pass, with multi-aspect-ratio export for TikTok/Reels/YouTube."
---

# Video Batch Editor Skill

## When to Use

Activate this skill when:
- Editing multiple raw video clips (product demos, UGC clips, talking-head content) into publish-ready posts in one pass
- Auto-captioning a batch of videos for accessibility and sound-off viewing (85%+ of social video is watched muted)
- Syncing multiple clips to trending audio without manual timeline editing per clip
- Re-exporting one video into multiple aspect ratios (9:16 TikTok/Reels, 1:1 Facebook feed, 16:9 YouTube) without re-editing each
- Applying a consistent brand look (color grade, intro/outro, logo watermark) across a content batch

This skill takes raw or AI-generated clips (from `ai-visual-content` or a real camera) and turns them into publish-ready output. It does not generate video — see `ai-visual-content` for that.

## Batch Editing Philosophy

**Edit the template once, apply to many.** Instead of manually cutting each video, define an edit template (cut rules, caption style, music, effects) once, then run it across a folder of raw clips. This is what makes 10-20 videos/week achievable for one person.

```
Raw clips folder → Define edit template → Batch apply → Review/spot-fix → Multi-ratio export → Publish
```

## Tool Options

| Tool | Best For | Setup Complexity |
|---|---|---|
| CapCut (desktop/auto-cut features) | Fast manual + semi-auto editing, built-in caption auto-generation, trending audio library | Low — GUI-based |
| ffmpeg scripting | Fully automated batch pipelines (cut silence, resize, watermark, concatenate) with no manual GUI work per video | Medium — requires scripting, but scales infinitely |
| Whisper (OpenAI) + burn-in | Automatic transcription → SRT captions → styled burn-in, works in any language including Vietnamese | Medium — pairs with ffmpeg for the burn-in step |
| Magnific UGC toolkit (if available) | Silence-cut ("magic cut"), camera angle, color grade/transfer, background removal, music/SFX layering, audio isolation/mix — via prompt-driven calls, no manual timeline | Low — tool-call driven, good for batch runs without scripting |

**Decision rule:** one-off video → CapCut or a single Magnific UGC call is faster. 5+ videos with the same edit pattern → build the ffmpeg/automation pipeline once, it pays for itself by the 3rd video.

## Reference Files

- **[Batch Editing Workflow](references/batch-editing-workflow.md)**: Full technical pipeline — silence-cut, music sync, brand overlay, multi-aspect-ratio export, with ffmpeg command examples
- **[Caption & Subtitle Automation](references/caption-subtitle-automation.md)**: Auto-transcription workflow, Vietnamese diacritics handling, caption styling for stop-scroll readability

## Standard Batch Pipeline

```
1. INGEST
   └─> Collect raw clips into dated batch folder (assets/video-batch/{date}-{campaign}/raw/)

2. AUTO-CUT
   └─> Remove dead air/silence (auto-detect below volume threshold)
   └─> Trim to platform-appropriate length (TikTok: 15-34s sweet spot, Reels: 15-30s, YouTube Shorts: up to 60s)

3. CAPTION
   └─> Transcribe (Whisper or platform auto-caption)
   └─> Style captions (font, position, highlight color per brand)
   └─> Burn in or export as separate SRT (burn-in for social, SRT for YouTube)

4. AUDIO
   └─> Sync to trending/brand audio track
   └─> Balance voice vs. music levels (voice should stay 3-6dB above music)
   └─> Add SFX at key moments if applicable (product reveal, CTA)

5. VISUAL POLISH
   └─> Apply consistent color grade/LUT across batch
   └─> Add intro bumper (1-2s) and outro CTA card
   └─> Add logo watermark (corner, low opacity, consistent position)

6. MULTI-RATIO EXPORT
   └─> 9:16 (TikTok, Reels, Shorts)
   └─> 1:1 (Facebook/Instagram feed)
   └─> 16:9 (YouTube, website embed)
   └─> Reframe/crop per ratio — do not just letterbox; recompose so the subject stays centered

7. QUALITY CHECK
   └─> Spot-check every 5th video in batch for cut errors, caption sync drift, audio clipping
   └─> Verify no copyrighted music flagged (if using trending audio commercially)
```

## Best Practices

1. **Batch by content type, not by date** — mixing talking-head and product-demo clips in one batch template produces worse edits than running two smaller batches with matched templates.

2. **Caption for sound-off first** — design the edit assuming zero audio. If the message survives with captions alone, audio becomes a bonus, not a dependency.

3. **Keep the first 1-2 seconds cut-free of branding** — logo/intro bumpers before the hook kill retention. Hook first, brand identity woven in after (lower-third watermark, not a full-screen intro).

4. **Version-control your edit template** — when a batch performs well, save the exact cut/caption/music settings as a named template (`templates/edit-template-{name}.md`) to reuse, not just "remember roughly what worked."

5. **Export once, verify per platform** — a 9:16 crop that looks fine standalone can crop out a product/face when the platform adds its own UI overlay (captions, like button). Preview inside the actual platform app before scheduling.

## Usage Examples

**Batch Processing Request**:
```
I have 8 raw UGC clips in assets/video-batch/260315-product-launch/raw/.
Apply the standard batch pipeline: auto-cut silence, burn Vietnamese
captions styled bold-white-yellow-highlight, sync to [track name], export
all 3 aspect ratios.
```

**Template Definition**:
```
Define a reusable edit template for talking-head testimonial videos:
15-25s length, captions bottom-third, brand color grade [name], logo
watermark bottom-right, background music at -18dB under voice.
```

**Multi-Ratio Re-export**:
```
I have one 16:9 product demo video already edited. Re-export it as 9:16
and 1:1 with subject-centered reframing, no re-edit needed.
```

## What This Skill Does Not Do

- Does not generate new video content from scratch — that's `ai-visual-content`
- Does not write video scripts or hooks — that's `content-creation` (`references/video-script-framework.md`)
- Does not handle long-form (10+ minute) editing workflows — this is built for short-form batch output

## Handoff

Output from this skill feeds directly into `content-creation`'s content calendar (as the ready-to-publish asset) and into paid ad campaigns managed under the Conversion/Attraction Agent workflow.
