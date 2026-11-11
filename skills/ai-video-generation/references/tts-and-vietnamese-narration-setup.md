# TTS & Vietnamese Narration Setup

Shared narration layer for all three video types in this skill. Covers provider selection, setup, and the Vietnamese-specific pronunciation issues that are the most common cause of unusable output.

## Provider Comparison

| Provider | Cost | API Key Needed | Best For |
|---|---|---|---|
| **Edge TTS** (`edge-tts-universal`) | Free | None — wraps Microsoft Edge's neural TTS | Default choice for all 3 video types. No account, no billing, good Vietnamese voice quality (`vi-VN-HoaiMyNeural`, `vi-VN-NamMinhNeural`) |
| **Vbee** | Paid | `VBEE_APP_ID` + `VBEE_ACCESS_TOKEN` + `VBEE_VOICE_CODE` | Vietnamese-specialized voice quality beyond Edge TTS — worth it if narration quality is a differentiator for the content |
| **ElevenLabs** | Paid | `ELEVENLABS_API_KEY` + `ELEVENLABS_VOICE_ID` | Multilingual, highest naturalness for mixed Vietnamese/English content; `eleven_multilingual_v2` model |
| **LucyLab.io** | Paid | `VIETNAMESE_API_KEY` + `VIETNAMESE_VOICEID` | Voice cloning — only needed if replicating a specific brand voice/persona |

**Default recommendation:** start with Edge TTS for every project — it's free, requires zero setup, and the quality is production-viable. Only switch to a paid provider if a specific project needs voice cloning or a measurably better Vietnamese accent/naturalness.

## Setup (Edge TTS — Default)

```bash
npm install edge-tts-universal
```

No account, no API key. Set the voice via env var:
```
EDGE_TTS_VOICE=vi-VN-HoaiMyNeural     # female voice
# or
EDGE_TTS_VOICE=vi-VN-NamMinhNeural    # male voice
EDGE_TTS_RATE=+10%                     # optional speed adjustment
EDGE_TTS_PITCH=                        # optional pitch adjustment
```

Requires outbound internet access to Microsoft's Edge TTS endpoint — no account needed, but not usable fully offline.

## Critical: Vietnamese Phonetic Normalization

TTS engines (all providers, not just Edge) frequently mispronounce the following in Vietnamese narration. **Normalize the script text before sending it to TTS, not after** — this is the single most common cause of unusable narration across all three video pipelines.

| Input Pattern | Problem | Fix |
|---|---|---|
| `50%` | Often read as raw symbol or English | Write out: "năm mươi phần trăm" |
| `$100` / `100k` / currency | Inconsistent reading of symbols | Write out: "một trăm đô la" / "một trăm nghìn" |
| Decimals (`3.5`) | May read the period as "chấm" incorrectly or skip it | Write out: "ba phẩy năm" |
| English loanwords (`Dev`, `SaaS`, `AI`) | Mispronounced with English phonetics forced into Vietnamese TTS, or vice versa | Test the specific word; if mispronounced, respell phonetically (e.g. "Dev" → "Đép") — verify by listening to the generated clip, don't guess |
| Abbreviations (`VND`, `Q1`, `KPI`) | Read as letters instead of the intended expansion | Spell out the intended reading: "VND" → "đồng Việt Nam" if that's the intended pronunciation |

**Workflow:** write the script naturally first, then run a normalization pass specifically looking for numbers, currency, percentages, and loanwords before generating TTS. If producing many videos, keep a running phonetic-substitution list of words that come up repeatedly in your niche.

## Measuring Real Audio Duration (Do Not Estimate)

All three video pipelines depend on knowing the *exact* duration of generated narration to time visuals correctly. Never assume "N words ≈ N/2.5 seconds" — always measure the actual generated audio file:

```bash
# Via ffprobe (HyperFrames-based pipelines)
ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 scene1.mp3

# Via MP3 frame-header parsing (Remotion-based pipeline pattern)
# — parses the MP3's own frame headers to compute exact seconds,
#   more precise than relying on file metadata alone
```

Convert the measured duration to frames (`seconds * fps`) and add a small safety pad (~10 frames at 30fps ≈ 0.33s) before setting scene/composition duration — this prevents audio getting cut off at a scene boundary.

## Testing a Voice Before Committing to a Full Video

Always generate a short test clip with the specific phrases likely to cause pronunciation issues (numbers, brand names, technical terms in your niche) before writing a full script — cheaper to catch a mispronunciation in a 5-second test than after rendering a full 60-second video.

```bash
npm run tts -- --text "Kiểm tra: 50%, một trăm nghìn đồng, SaaS" public/audio/test.mp3
```

Listen to the output. If anything mispronounces, add it to your normalization list before writing the real script.
