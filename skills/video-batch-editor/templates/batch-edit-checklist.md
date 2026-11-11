# Batch Video Edit — Run Checklist

Use this per batch run to keep output consistent and catch failures before publishing.

```markdown
## Batch Run: [Campaign/Project Name]
**Date:** [YYYY-MM-DD]
**Clip count:** [N]
**Source folder:** assets/video-batch/[date]-[campaign]/raw/

### Edit Template Used
- Silence-cut threshold: [value]
- Target platform length: [15-34s TikTok / 15-30s Reels / etc.]
- Caption style: [font, size, position, color]
- Music track: [name/source, licensing confirmed: Y/N]
- Color grade/LUT: [name]
- Logo watermark position: [corner, opacity %]

### Per-Platform Export
- [ ] 9:16 exported (TikTok/Reels/Shorts) — subject centered, no crop cut-off
- [ ] 1:1 exported (Facebook/Instagram feed) — subject centered
- [ ] 16:9 exported (YouTube) — native or padded correctly

### Quality Check (every 5th clip minimum, more if batch >15)
- [ ] Caption sync verified against audio (no drift)
- [ ] Vietnamese diacritics render correctly
- [ ] Numbers/prices/dates in captions match source audio exactly
- [ ] Audio levels consistent — no clipping, music doesn't overpower voice
- [ ] No cut-off subject/product in any aspect ratio
- [ ] Watermark doesn't overlap captions or platform UI
- [ ] Hook (first 1-2s) is not covered by intro bumper/branding
- [ ] Test-uploaded to target platform app to verify final in-app appearance

### Failures Found This Run
| Clip | Issue | Fix Applied |
|---|---|---|
| | | |

### Sign-off
- [ ] All clips pass quality check
- [ ] Batch ready for content calendar scheduling
```
