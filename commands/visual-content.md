---
description: Generate AI product images and video ads from prompts
argument-hint: [type] [product]
---
Generate AI visual content (images or video) for a product or campaign.

<args>$ARGUMENTS</args>

## Types
- `image [product]` - Generate product hero/lifestyle images
- `video [product]` - Generate a short video ad from a prompt
- `batch [product]` - Generate a set of image variations for A/B testing
- `animate` - Turn an existing static product image into a short video (image-to-video)

## Process
1. Activate Attraction Agent
2. Use AI Visual Content skill
3. Build the prompt with the S-C-O-P-E framework, select the right tool for the asset type
4. Generate 3-5 variations, apply the anti-"AI smell" quality checklist
5. Output: Ready-to-use image/video asset, or handoff note to Video Batch Editor for further editing

## Examples
- `/visual-content image skincare serum` — Generate a product hero image with lifestyle context
- `/visual-content video protein shake` — Generate a 15-30s UGC-style video ad
- `/visual-content batch running shoes` — Generate 5 ad image variations for testing
- `/visual-content animate` — Animate an existing static hero shot into a short clip

## Tips
- Always fill out the prompt brief template before generating — consistency across a campaign matters more than any single great image
- Generate 3-5 variations and manually pick the best; never ship the first output
- Route finished video output to `/video-batch` for captioning, music sync, and multi-platform export
