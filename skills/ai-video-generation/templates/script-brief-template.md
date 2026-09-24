# AI Video Generation — Script Brief Template

Fill this out before writing any scene text. Forces the right structure per video type before touching the rendering pipeline.

```markdown
## Video Brief: [Project/Campaign Name]

**Type:** [Topic Explainer / URL-Repo Recap / Comparison]
**Input:** [Topic name / Source URL / Concept A vs Concept B]
**Target runtime:** [50-60s Topic Explainer / 48-72s Recap / 30-40s Comparison]
**Render engine:** [Remotion / HyperFrames]
**Language:** [Vietnamese / English / Mixed]

### Script (per type structure — see relevant reference file)
[Topic Explainer: 6 scenes, Hook/Problem/Concept1/Concept2/Impact/Outro]
[Recap: 5-8 scenes, 1 hook + 3-6 body + 1 outro, mapped to templates: hook/comparison/stat-hero/feature-list/callout/outro]
[Comparison: exactly 12 lines, Hook(2)/Question(1)/RevealA(3)/RevealB(3)/Comparison(2)/Payoff(1)]

Scene/Line 1: [text]
Scene/Line 2: [text]
...

### TTS
- Provider: [edge (default, free) / vbee / elevenlabs / lucylab]
- Voice: [vi-VN-HoaiMyNeural / vi-VN-NamMinhNeural / other]
- Phonetic normalization check: [list any numbers/currency/loanwords that need respelling]

### Visual
- Theme: [dark-neon / light-pro / brand-specific]
- Brand elements: [channel name, avatar/logo, handle]
- Source og:image (Recap only): [Y/N, fallback: gradient]

### Self-Check Before Rendering
- [ ] Hook contains a specific claim, not a vague teaser
- [ ] Word/line counts match the type's structure limits
- [ ] Numbers/currency/loanwords normalized for TTS
- [ ] Test TTS clip generated and checked for mispronunciation
- [ ] Preview/snapshot reviewed before full render
```
