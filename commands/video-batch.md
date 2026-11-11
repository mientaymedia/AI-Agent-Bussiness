---
description: Batch edit multiple videos — cut, caption, sync music, export
argument-hint: [action]
---
Run automated batch editing across a folder of video clips.

<args>$ARGUMENTS</args>

## Actions
- `process [folder]` - Run the full batch pipeline (cut, caption, audio, grade, export)
- `caption [folder]` - Auto-transcribe and burn in captions only
- `export [file]` - Re-export one already-edited video into all 3 aspect ratios
- `template [name]` - Define/save a reusable edit template

## Process
1. Activate Attraction Agent
2. Use Video Batch Editor skill
3. Apply the standard pipeline: silence-cut → caption → audio sync → color grade/watermark → multi-ratio export
4. Spot-check every 5th video against the quality checklist
5. Output: Publish-ready video files in 9:16, 1:1, and 16:9

## Examples
- `/video-batch process assets/video-batch/260315-launch/raw` — Run the full pipeline on a folder of raw clips
- `/video-batch caption assets/video-batch/260315-launch/working` — Auto-caption an already-cut batch
- `/video-batch export final-demo.mp4` — Re-export one video into all platform aspect ratios
- `/video-batch template testimonial` — Save current edit settings as a reusable "testimonial" template

## Tips
- Design every video assuming sound-off — captions carry the message, audio is a bonus
- Batch clips of the same content type together (don't mix talking-head and product-demo in one template run)
- Recompose per aspect ratio instead of letterboxing — a centered subject in 16:9 can get cropped out in 9:16
