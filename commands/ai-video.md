---
description: Generate a fully automated narrated video from a topic, URL, or comparison
argument-hint: [type] [input]
---
Generate a narrated, publish-ready video using AI-agent-driven scripting and code-based rendering.

<args>$ARGUMENTS</args>

## Types
- `topic [subject]` - 6-scene animated explainer video (Remotion), ~50-60s
- `recap [url]` - Narrated recap of an article/blog post/GitHub repo (HyperFrames), ~48-72s
- `compare [A] vs [B]` - Side-by-side comparison video with avatar narrator (HyperFrames), ~30-40s

## Process
1. Activate Attraction Agent
2. Use AI Video Generation skill
3. Fill the script brief template, write the scene/line script following the type's exact structure
4. Normalize numbers/currency/loanwords for Vietnamese TTS pronunciation
5. Generate a short TTS test clip to catch mispronunciations before the full script
6. Run the deterministic pipeline (TTS → render → encode) per the type's reference file
7. Output: publish-ready 9:16 MP4 + companion caption/hashtag files

## Examples
- `/ai-video topic Docker` — Generate a 6-scene explainer video about Docker
- `/ai-video recap https://github.com/some/repo` — Generate a narrated recap of a GitHub repo
- `/ai-video compare Dev vs DevOps` — Generate a comparison video between two concepts

## Tips
- Requires Node.js, FFmpeg, and either Remotion or HyperFrames set up in the working project — this is infrastructure-heavier than other commands in this kit, budget for one-time setup
- Write and lock the script before running any rendering step — the pipeline is deterministic once the script exists
- Always measure real TTS audio duration to time visuals — never estimate from word count
- Route the output to `/video-batch` only if further re-export or branding is needed
