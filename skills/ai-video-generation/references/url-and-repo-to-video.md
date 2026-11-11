# URL & Repo to Video (HyperFrames)

Turns a news article, blog post, or GitHub repo URL into a narrated 9:16 recap video. Adapted from the HyperFrames-based pattern in `Cuongyd196/auto-video-gen` (MIT, forked/extended from an open "Auto-Create-Video" project — explicitly built for reuse by AI coding agents).

## Output

9:16 1080×1920 @30fps MP4, ~48-72 seconds, plus companion files: `voice.mp3`, `script.txt` (for platforms with their own auto-caption like CapCut), `voice/*.srt`, and `caption.txt` (a ready-to-post caption + 4 hashtags).

## Tech Stack

- **HyperFrames** (HTML/CSS + GSAP timelines, rendered via Puppeteer/headless Chrome, encoded with FFmpeg) — not Remotion/React
- **edge-tts-universal** (free default) or paid alternatives (LucyLab voice cloning, ElevenLabs, Vbee) — see `tts-and-vietnamese-narration-setup.md`
- `og:image` scraped from the source URL for the visual (or a gradient fallback if none found) — no AI image generation involved
- **Zod v4** discriminated unions validate the script's JSON schema before rendering

## Split Architecture: Creative Half vs Deterministic Half

```
"AI creative half" (you, the agent, do this):
  Fetch the URL → extract title/content/og:image/domain → write script.json

"Deterministic production half" (a fixed code pipeline runs this):
  script.json → TTS → HTML compose → Puppeteer render → FFmpeg encode → video.mp4
  Same script.json always produces identical output frames.
```

Get the `script.json` right — everything after it is mechanical and repeatable.

## Step 1: Content Extraction (You, the Agent)

Fetch the URL and extract:
```json
{
  "title": "...",
  "content": "...",
  "ogImage": "https://...",
  "domain": "example.com"
}
```

For a GitHub repo URL specifically: extract the repo description, README highlights, and key stats (stars, primary language, recent notable commits/releases) instead of article body text.

## Step 2: Write the Script (5-8 Scenes, ~150-200 words total, ~55-65s)

Structure: **1 hook + 3-6 body scenes + 1 outro**. Each scene maps to one of 6 visual templates (defined in the schema — pick per scene based on content type, not arbitrarily):

| Template | Use For |
|---|---|
| `hook` | Opening scene — must contain a specific claim, not a vague teaser |
| `comparison` | Contrasting two things mentioned in the source |
| `stat-hero` | A single striking number/statistic as the visual focus |
| `feature-list` | 3-5 bullet points (e.g. repo features, article key points) |
| `callout` | A quote or key takeaway, visually emphasized |
| `outro` | Recap + source attribution + CTA |

**Hook rule:** the first scene must contain a specific, concrete claim from the source content — a vague teaser ("today we'll look at...") loses viewers in the first 2 seconds.

## Step 3: Vietnamese TTS Phonetic Normalization (Critical)

Before sending script text to TTS, normalize anything that TTS commonly mispronounces:
- Spell out decimals and percentages: "50%" → "năm mươi phần trăm"
- Spell out currency: "$100" → "một trăm đô la"
- Spell out ambiguous units/abbreviations rather than relying on TTS to infer pronunciation
- Watch for English loanwords the TTS engine mispronounces (e.g. "Dev" read incorrectly) — consider a manual phonetic respelling if the mispronunciation is jarring

This step is the single most common cause of unusable narration — do not skip it even when the script looks fine as written text.

## Step 4: Run the Deterministic Pipeline

```bash
# One-time setup
git clone <this-pattern-as-your-own-project>
npm install
cp .env.example .env.local
# Default TTS (edge-tts-universal) needs zero API keys

# Generate the video from your script.json
npm run pipeline -- output/<slug>/script.json

# Re-render visuals only (reuse existing voice audio) — fast iteration
npm run rerender -- output/<slug>
```

**Pipeline internals (for reference, runs automatically):**
1. Validate `script.json` against the Zod schema
2. Write `script.txt` (concatenated narration, for CapCut-style auto-caption)
3. Fetch `og:image` + generate per-scene TTS mp3/srt in parallel (idempotent — skips regenerating if the mp3 already exists)
4. Concatenate scene audio with silence gaps, auto-select and mix SFX per scene (keyword-matched against narration text, falls back to template default)
5. Compose the full HTML (scenes + GSAP timeline)
6. Render via `npx hyperframes render <dir> --output video.mp4 --fps 30 --quality standard`

## Env Vars

```
TTS_PROVIDER=edge                # edge (free, default) | lucylab | elevenlabs | vbee
VIDEO_THEME=dark-neon            # or light-pro
TIKTOK_DISPLAY_NAME=
TIKTOK_HANDLE=
TIKTOK_FOLLOWERS=
TIKTOK_AVATAR_URL=
TTS_CONCURRENCY=
# Paid TTS providers only, if selected:
ELEVENLABS_API_KEY=
ELEVENLABS_VOICE_ID=
VBEE_APP_ID=
VBEE_ACCESS_TOKEN=
```

## Requirements

Node.js ≥22, FFmpeg+ffprobe in PATH, Chrome/Chromium (Puppeteer auto-downloads it on install).

## Self-Check Before Rendering

- [ ] Hook scene contains a specific claim, not a vague teaser
- [ ] Total script is 150-200 words (~55-65s at natural speaking pace)
- [ ] Numbers/percentages/currency normalized for TTS pronunciation
- [ ] Each scene assigned the correct template for its content type
- [ ] `script.json` validates against the Zod schema before running the pipeline
- [ ] Caption + hashtags generated for the target platform
