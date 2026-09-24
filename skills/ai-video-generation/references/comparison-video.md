# Comparison Video (HyperFrames)

Turns a pair of concepts/products/tools into a side-by-side comparison video with an animated avatar narrator. Adapted from the HyperFrames-based pattern in `Cuongyd196/auto-compare-video` (MIT, explicitly open for cloning/customizing, built on HyperFrames by HeyGen, Apache-2.0).

## Output

9:16 1080×1920 MP4, 30-40 seconds. Two concept cards side by side, animated captions highlighting key differences, a 2D gesturing avatar "MC", and synced narration.

## Tech Stack

- **HyperFrames** (`hyperframes init/preview/check/render/snapshot/publish` CLI) — plain HTML files with `data-hf-id`/`data-start`/`data-duration` attributes, animated via GSAP timelines
- **edge-tts-universal** (free default) or **Vbee** (Vietnamese-specialized, paid) — see `tts-and-vietnamese-narration-setup.md`
- `ffprobe` measures exact per-line audio duration to drive timing (never estimated)
- Self-embedded fonts (Be Vietnam Pro + JetBrains Mono) with `unicode-range` to preserve Vietnamese diacritics — do not swap in a Google Fonts `<link>`, embedding avoids a font-load flash and guarantees diacritic support

## The 3-Zone Design Contract

```
┌─────────────────────────────┐
│  [Concept A Card] [Concept B Card]   ← top zone: comparison cards
│                               │
│      "Single animated         │   ← middle zone: one caption line at a time
│       caption line"           │
│                               │
│     [Gesturing Avatar]       │   ← bottom zone: avatar MC
└─────────────────────────────┘
```

Keep safe-zone margins clear of platform UI overlays (like/comment/share buttons typically along the right edge and bottom of TikTok/Reels). Color-code keywords: **pink = difference/contrast**, **cyan = concept identity** — consistent color meaning helps viewers track which side of the comparison is being discussed even with sound off.

## The 12-Line Script Structure (Beat Template)

Write exactly 12 short lines (≤12-14 words each, for caption-line fit) following this beat sequence:

| Beat | Lines | Purpose |
|---|---|---|
| Hook | 2 | Grab attention with the comparison premise |
| Question | 1 | Pose the question the video will answer |
| Reveal A | 3 | Introduce and explain concept A |
| Reveal B | 3 | Introduce and explain concept B |
| Direct Comparison | 2 | Put A and B head-to-head explicitly |
| Payoff | 1 | The takeaway/conclusion |

## Timing Formula (Deterministic — Do Not Guess)

```
start[line 1] = 0.55s
+0.3 to 0.35s gap between lines within the same beat
+0.4 to 0.45s gap when crossing into a new beat
Total runtime must land in the 30-40s range
```

This is computed from **real measured VO durations** (via ffprobe on the generated MP3s), not fixed per-line estimates — generate audio first, then compute timing from it.

## Pipeline (You, the Agent, Perform Each Step)

```bash
# 1. Content planning (you write this directly, no code yet)
#    Propose: concept pair, slug, 12-line script per the beat template above

# 2. Scaffold the project
node .claude/skills/create-video/scripts/scaffold.mjs <slug>
cd videos/<slug> && npm install

# 3. Generate voiceover
#    Edit scripts/generate-vo.mjs's LINES array with your 12 lines
node scripts/generate-vo.mjs
#    -> generates 12 MP3s, probes real durations via ffprobe,
#       writes assets/vo/durations.json

# 4. Compose index.html
#    Copy structure from a reference video; swap ONLY text/icons/<audio> tags/
#    VO object/ROOT_DURATION/timeline calls — CSS, @font-face, avatar rig,
#    and GSAP helpers (showLine, pose, headTilt, talk) stay unchanged

# 5. Validate
npm run check                                   # HyperFrames lint: layout/motion/contrast

# 6. Preview
npx hyperframes@0.7.58 snapshot . --frames 7    # frame-by-frame visual check

# 7. Render
npm run render                                   # -> renders/<slug>.mp4

# 8. Optional: shareable link
npm run publish
```

Agent-invocation shorthand (if replicating the exact template repo): `/create-video Dev và DevOps`

## Known Layout Gotcha

Wrap caption text + any inline keyword `<span>` in one `.caption-line-text` container — splitting them across separate elements causes flexbox to break words mid-line unpredictably.

## Env Vars

```
TTS_PROVIDER=edge          # edge (free) | vbee (Vietnamese-specialized, paid)
EDGE_VOICE=
CHANNEL=
AUTO_CREATE_VIDEO=0         # 0 = confirm each step, 1 = run autonomously
# If TTS_PROVIDER=vbee:
VBEE_APP_ID=
VBEE_ACCESS_TOKEN=
VBEE_VOICE_CODE=
```

## Requirements

Node.js ≥18, FFmpeg+ffprobe in PATH.

## Self-Check Before Rendering

- [ ] Exactly 12 lines, each ≤12-14 words
- [ ] Beat structure followed (hook→question→revealA→revealB→comparison→payoff)
- [ ] Audio generated and durations measured before computing any timing
- [ ] Total runtime lands in 30-40s
- [ ] Keyword color-coding consistent (pink=difference, cyan=identity)
- [ ] `npm run check` passes before rendering
