---
name: ai-video-generation
description: "Fully automated narrated video production — topic explainers, URL/article/GitHub-repo recaps, and side-by-side comparison videos — driven by an AI coding agent writing the script, free TTS narration, and code-based rendering (Remotion or HyperFrames)."
---

# AI Video Generation Skill

## When to Use

Activate this skill when producing a **fully automated, narrated, publish-ready video** from one of three inputs:
- A **topic** (e.g. "Docker", "Content Marketing") → 6-scene animated explainer video
- A **URL** (news article, blog post, or GitHub repo) → narrated recap/summary video
- A **comparison pair** (e.g. "Dev vs DevOps", two product tools) → side-by-side comparison video with an avatar

This is a different production model from `ai-visual-content` (single prompt-generated images/clips) and `video-batch-editor` (post-production editing of raw footage). Here, the AI agent itself writes the narration script, drives free text-to-speech, and renders the final MP4 through code — no manual filming, no manual editing, no paid image/video-generation API required by default.

**Important — this skill requires real local infrastructure, not just prompting:** Node.js, FFmpeg (+ffprobe), and either Remotion or the HyperFrames CLI (which uses Puppeteer/headless Chrome) must be installed in the working project. This is heavier setup than most skills in this kit — budget for a one-time environment setup before the first video.

## The Three Video Types — Decision Matrix

| Type | Input | Best For | Render Engine | Reference File |
|---|---|---|---|---|
| **Topic Explainer** | A single topic/concept | Educational/authority content, "what is X" explainers, product feature deep-dives | Remotion (React-based) | `references/topic-explainer-remotion.md` |
| **URL/Article/Repo Recap** | A news URL, blog post, or GitHub repo link | Repurposing existing content into video, "here's what's new" recaps, dev-tool announcements | HyperFrames (HTML+GSAP) | `references/url-and-repo-to-video.md` |
| **Comparison** | Two concepts/products/tools | "X vs Y" content, decision-help content, positioning against a competitor | HyperFrames (HTML+GSAP) | `references/comparison-video.md` |

All three share the same narration/TTS layer — see `references/tts-and-vietnamese-narration-setup.md` before starting any of them.

## Core Principle: The Agent IS the Content Engine

None of these pipelines call an LLM API internally. **You (the AI agent running this skill) write the script directly** — reading the topic/URL/comparison pair, drafting the narration text scene-by-scene, following the exact structure in the relevant reference file. The code pipeline that follows (TTS → render → encode) is deterministic: same script in, same video out. Get the script right; the rendering is mechanical after that.

```
You (the agent) write the script  →  Deterministic pipeline takes over
   - Read input (topic/URL/pair)       - TTS generates narration audio
   - Draft scene-by-scene text         - Real audio duration measured (not estimated)
   - Follow beat/timing structure      - Visuals timed to match audio exactly
   - Self-check against word counts    - FFmpeg/Puppeteer renders final MP4
```

## Shared Setup (All Three Types)

```bash
# Prerequisites
node --version   # >= 18 (auto-video-gen/auto-compare-video need >=22)
ffmpeg -version  # required for audio duration measurement and encoding
ffprobe -version # required (ships with ffmpeg)

# Free TTS — zero API keys required by default
npm install edge-tts-universal
```

See `references/tts-and-vietnamese-narration-setup.md` for the free-vs-paid TTS decision, Vietnamese voice selection, and the phonetic-normalization rules needed so TTS doesn't mispronounce numbers/percentages/loanwords.

## Reference Files

- **[Topic Explainer (Remotion)](references/topic-explainer-remotion.md)**: 6-scene structure (Hook/Problem/Concept1/Concept2/Impact/Outro) with exact frame-timing budget, React/Remotion coding rules, CLI commands
- **[URL & Repo to Video (HyperFrames)](references/url-and-repo-to-video.md)**: Content-extraction rules, 6-template scene taxonomy (hook/comparison/stat-hero/feature-list/callout/outro), CLI pipeline, caption/hashtag generation
- **[Comparison Video (HyperFrames)](references/comparison-video.md)**: 12-line beat script structure, 3-zone layout contract, avatar animation rules, timing-gap formula
- **[TTS & Vietnamese Narration Setup](references/tts-and-vietnamese-narration-setup.md)**: Provider comparison (free vs paid), voice selection, phonetic normalization table for numbers/units/loanwords

## Best Practices

1. **Write the script before touching any rendering tool.** All three pipelines are deterministic once the script exists — the creative work (and the risk of a bad video) is entirely in the script-writing step.

2. **Never estimate audio duration — measure it.** All three reference pipelines generate the TTS audio first, then read its *actual* duration (via ffprobe or MP3 frame parsing) to time the visuals. A pipeline that assumes "X words ≈ Y seconds" will produce audio/visual drift. Follow this pattern even if adapting the pipeline.

3. **Respect the word-count-per-scene limits in each reference file.** These aren't arbitrary — they're calibrated so captions fit on a 9:16 mobile frame in single readable lines. Overwriting them breaks the visual layout.

4. **Vietnamese TTS needs phonetic help.** Numbers, percentages, currency, and English loanwords (e.g. "Dev" mispronounced) need explicit normalization before being sent to TTS — see the phonetic table in `tts-and-vietnamese-narration-setup.md`. Skipping this step is the most common cause of unusable narration.

5. **Preview before final render.** Every pipeline has a snapshot/preview step (frame snapshot in HyperFrames, `remotion studio` in Remotion) — always check the visual before spending render time on a full video.

## What This Skill Does Not Do

- Does not generate video from raw prompts with no script (see `ai-visual-content` for that)
- Does not edit/caption/re-export existing raw footage (see `video-batch-editor` for that)
- Does not include an LLM API call — script-writing is done by you, the agent running this skill, not by a third-party API inside the pipeline

## Handoff

Rendered videos are publish-ready 9:16 MP4s. Route them through `video-batch-editor` only if further multi-platform re-export or additional branding/watermarking is needed — otherwise they're ready for `content-creation`'s content calendar directly.
