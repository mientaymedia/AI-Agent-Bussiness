# Batch Editing Workflow

Technical pipeline for turning a folder of raw clips into publish-ready, multi-aspect-ratio video output with minimal manual per-clip work.

## Folder Structure

```
assets/video-batch/{date}-{campaign}/
├── raw/              # Original unedited clips
├── working/          # Intermediate cuts during processing
├── captioned/         # After caption burn-in
└── export/
    ├── 9x16/          # TikTok, Reels, Shorts
    ├── 1x1/           # Facebook/Instagram feed
    └── 16x9/          # YouTube, website embed
```

## Step 1: Silence-Cut (Auto Remove Dead Air)

**ffmpeg approach** (detects and removes segments below a volume threshold):

```bash
# Detect silence segments
ffmpeg -i raw/clip1.mp4 -af silencedetect=noise=-30dB:d=0.5 -f null - 2>silence_log.txt

# Use a tool like auto-editor (built on ffmpeg) for one-command silence removal:
auto-editor raw/clip1.mp4 --output working/clip1_cut.mp4 --silent-threshold 0.04
```

**Magnific UGC toolkit approach** (if available in your workspace): use the "magic cut" video tool — pass the raw clip, it detects and removes dead air/silence automatically without manual threshold tuning.

**Manual fallback (CapCut):** Use "Auto captions" first (creates a transcript with timestamps), then delete segments where the transcript shows long gaps.

## Step 2: Trim to Platform Length

| Platform | Sweet Spot | Hard Max |
|---|---|---|
| TikTok | 15-34s | 10 min |
| Instagram Reels | 15-30s | 90s |
| YouTube Shorts | 15-60s | 60s |
| Facebook feed video | 15-60s | No hard limit, but retention drops sharply after 60s |

Trim from the working cut, keeping the strongest hook in the first 1-2 seconds — never bury the hook under an intro bumper.

## Step 3: Caption Burn-In

See `caption-subtitle-automation.md` for the full transcription-to-burn-in pipeline. Summary:

```bash
# 1. Transcribe with Whisper (handles Vietnamese)
whisper working/clip1_cut.mp4 --language Vietnamese --output_format srt

# 2. Style and burn in with ffmpeg
ffmpeg -i working/clip1_cut.mp4 -vf "subtitles=clip1_cut.srt:force_style='FontName=Montserrat,FontSize=24,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,BorderStyle=3,Outline=2'" captioned/clip1_captioned.mp4
```

## Step 4: Audio Sync

**Layering rule:** voice (if present) stays 3-6dB above background music; music ducks further during any spoken word.

```bash
# Mix voice track with background music, ducking music under voice
ffmpeg -i captioned/clip1_captioned.mp4 -i music/track.mp3 \
  -filter_complex "[1:a]volume=0.3[music];[0:a][music]amix=inputs=2:duration=first" \
  -c:v copy working/clip1_audio.mp4
```

If using the Magnific UGC toolkit: the audio mix and audio isolation tools handle voice/music balancing and can layer SFX at marked timestamps without manual ffmpeg filter chains.

## Step 5: Visual Polish (Color Grade + Brand Overlay)

```bash
# Apply a LUT for consistent color grade across the batch
ffmpeg -i working/clip1_audio.mp4 -vf "lut3d=brand_lut.cube" working/clip1_graded.mp4

# Add logo watermark (bottom-right, low opacity, small size)
ffmpeg -i working/clip1_graded.mp4 -i assets/logo.png -filter_complex \
  "[1:v]scale=100:-1,format=rgba,colorchannelmixer=aa=0.6[wm];[0:v][wm]overlay=W-w-20:H-h-20" \
  working/clip1_branded.mp4
```

If using Magnific UGC toolkit: the color-grade and color-transfer tools apply a consistent look across a batch by referencing one graded example clip — faster than manually tuning a LUT per project.

## Step 6: Multi-Aspect-Ratio Export

**Do not just letterbox — recompose so the subject stays centered and readable in each ratio.**

```bash
# 9:16 (TikTok/Reels/Shorts) — crop centered on subject
ffmpeg -i working/clip1_branded.mp4 -vf "crop=ih*9/16:ih,scale=1080:1920" export/9x16/clip1.mp4

# 1:1 (Facebook/Instagram feed)
ffmpeg -i working/clip1_branded.mp4 -vf "crop=ih:ih,scale=1080:1080" export/1x1/clip1.mp4

# 16:9 (YouTube/website) — usually the native source ratio, pad if needed
ffmpeg -i working/clip1_branded.mp4 -vf "scale=1920:1080" export/16x9/clip1.mp4
```

**Subject tracking note:** if the subject moves across the frame, a fixed center-crop can cut them out. For dynamic shots, either (a) reframe manually per ratio, or (b) use a tool with auto-reframe/subject-tracking (many AI editing tools, including CapCut's "auto reframe," handle this automatically).

## Step 7: Batch Script (Process Many Clips in One Run)

```bash
#!/bin/bash
# batch_process.sh — run the full pipeline across every clip in raw/
for clip in raw/*.mp4; do
  name=$(basename "$clip" .mp4)
  auto-editor "$clip" --output "working/${name}_cut.mp4" --silent-threshold 0.04
  whisper "working/${name}_cut.mp4" --language Vietnamese --output_format srt
  ffmpeg -i "working/${name}_cut.mp4" -vf "subtitles=${name}_cut.srt:force_style='FontName=Montserrat,FontSize=24'" "captioned/${name}.mp4"
  # ... continue through audio, grade, export steps per clip
done
```

## Step 8: Quality Check

Spot-check every 5th video in the batch (not every single one — batch processing exists to save time, but silent failures compound):
- [ ] Caption sync matches spoken audio (drift is the most common batch-script failure)
- [ ] No cut-off subject in 9:16/1:1 crops
- [ ] Audio levels consistent (no clipping, no music overpowering voice)
- [ ] Logo watermark position consistent, not overlapping captions
- [ ] Export file plays correctly on target platform (test upload, don't just trust the file locally)
