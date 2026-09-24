# Topic Explainer Video (Remotion)

Turns a single topic/concept into a 9:16 vertical animated explainer video with synced AI narration. Adapted from the Remotion-based pattern in `Cuongyd196/remotion-cuongit-template` (MIT-licensed template; note the package.json itself says `UNLICENSED`/`private:true` as leftover boilerplate — treat as open template per the README, but don't assume a clean legal MIT grant without checking the actual repo before commercial reuse). Remotion itself carries a separate commercial license above certain usage thresholds — check remotion.dev's license terms before scaling this to paid client work.

## Output

9:16 vertical MP4, 1080×1920 @30fps, ~50-60 seconds. Animated React/Tailwind visuals (gradients, glassmorphism cards, spring animations — no raster images), synced narration, single-line chunked subtitles, fixed brand header.

## Tech Stack

- **Remotion** (React-based "video-as-code") for rendering
- **Tailwind CSS** for styling components
- **Zod** for composition prop schemas
- **edge-tts-universal** for free narration (no API key) — see `tts-and-vietnamese-narration-setup.md`

## The 6-Scene Structure (Time & Frame Budget)

Write the script to this exact structure — it's calibrated for a 50-60s total runtime at 30fps:

| Scene | Purpose | Time | Frames (approx) |
|---|---|---|---|
| 1. Hook | Grab attention, state the topic | 0-5s | ~150f |
| 2. Problem/Context | Why this topic matters | 5-15s | ~300f |
| 3. Concept 1 | First core idea | 15-28s | ~390f |
| 4. Concept 2 | Second core idea | 28-42s | ~420f |
| 5. Impact | Why it matters / result | 42-52s | ~300f |
| 6. Outro | Recap + CTA | 52-60s | ~240f |

Each scene's narration text: 15-30 words, short declarative sentences. Vietnamese or English depending on target audience.

## Pipeline (You, the Agent, Perform Each Step)

```
1. Draft the 6-scene script (short sentences per the table above)
2. Generate TTS per scene:
   - Write a small script following the pattern: call generateTopicVoices()
     from a tts helper, passing [{id: 'scene1', text: '...'}, ...]
   - This writes public/audio/<Topic>/<sceneId>.mp3 per scene
   - Parse each MP3's real duration (frame-header parsing or ffprobe),
     convert to durationInFrames at 30fps + ~10-frame safety pad
   - Write public/audio/<Topic>/manifest.json and src/<Topic>/audioData.ts
     (an `audioManifest` const mapping sceneId -> durationInFrames)
3. Scaffold the composition folder, following an existing example composition:
   src/<Topic>/{<Topic>.tsx, types.ts, audioData.ts, components/, scenes/Scene1..6.tsx}
4. Build each scene using Remotion's <Series>/<Series.Sequence>, pulling
   durationInFrames from the audio manifest (+3 frames buffer between scenes)
5. Register the new composition in src/Root.tsx:
   <Composition id="<Topic>" component={...} durationInFrames={...}
     fps={30} width={1080} height={1920} schema={zodSchema} defaultProps={...} />
6. Validate: npm run lint (eslint + tsc)
7. Preview: npm run dev (opens Remotion Studio at localhost:3000)
8. Render: npx remotion render <Topic> out/<Topic>.mp4
```

## Coding Rules (Non-Negotiable for Visual Quality)

- **Always use `spring()` for animation entrances, never linear easing** — spring physics reads as more natural/premium
- **Always clamp `interpolate()` calls** (`extrapolateLeft: 'clamp', extrapolateRight: 'clamp'`) — unclamped interpolation causes visual glitches outside the intended frame range
- **3-4 frame buffer between scene transitions** for a snappy (not jarring) cut
- **Subtitles: single-line, 5-7 words per chunk** — matches how captions are consumed on mobile, avoids 2-line wrapping that breaks the layout
- **Fixed brand header position** — keep it out of the zone platforms overlay with their own UI (like/comment buttons, captions)
- **Dark-mode glassmorphism design tokens**: use consistent blur/opacity/border values across all scenes in a project for visual coherence

## CLI Reference

```bash
npm run dev                                    # remotion studio (live preview)
npm run build                                   # remotion bundle
npm run lint                                    # eslint src && tsc
npm run tts -- --text "..." public/audio/test.mp3   # single-clip TTS test
npx remotion still <Topic> out/preview.png --frame 200   # thumbnail at a specific frame
npx remotion render <Topic> out/<Topic>.mp4     # final render
```

## Env Vars (All Optional — No Paid Keys Required by Default)

```
EDGE_TTS_VOICE=vi-VN-HoaiMyNeural        # or vi-VN-NamMinhNeural, en-US-ChristopherNeural, etc.
EDGE_TTS_RATE=+10%
EDGE_TTS_PITCH=
EDGE_TTS_VOLUME=
EDGE_TTS_OUTPUT_DIR=public/audio
CHANNEL_NAME=                             # brand header text
```

## Self-Check Before Rendering

- [ ] Each scene's narration is 15-30 words, matches the time budget
- [ ] Audio manifest durations were measured from real generated MP3s, not estimated
- [ ] All `interpolate()` calls are clamped
- [ ] Entrance animations use `spring()`, not linear easing
- [ ] Subtitle chunks are single-line, 5-7 words
- [ ] `npm run lint` passes before rendering
