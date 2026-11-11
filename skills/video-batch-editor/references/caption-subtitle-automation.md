# Caption & Subtitle Automation

Auto-captioning pipeline, with specific handling for Vietnamese text — a common failure point in auto-caption tools not tuned for Vietnamese diacritics.

## Why Captions Are Non-Negotiable

85%+ of social video is watched with sound off (commute, workplace, public spaces). A video that only works with audio loses the majority of its reach. Design every batch assuming zero audio — captions are the primary delivery channel for the message, not an accessibility add-on.

## Step 1: Transcription

**Whisper (OpenAI, open-source, runs locally or via API):**
```bash
whisper input.mp4 --language Vietnamese --model medium --output_format srt
```

**Model size tradeoff:**
| Model | Speed | Vietnamese Accuracy | Use When |
|---|---|---|---|
| `base` | Fastest | Lower, misses diacritics on fast speech | Quick draft check only |
| `medium` | Moderate | Good — recommended default | Standard batch runs |
| `large` | Slowest | Best, handles accents/slang better | Final production runs, high-stakes content |

**Common Vietnamese transcription errors to manually check:**
- Missing tone marks on fast/mumbled speech (ả vs á vs à vs ã vs ạ)
- Regional accent misreads (Southern vs Northern pronunciation)
- Mixed Vietnamese-English phrases (common in business/tech niches — "marketing", "content" often transcribed oddly)
- Numbers and currency (VND amounts) — verify these manually, transcription errors here are costly (wrong price shown on screen)

**Always manually review the SRT before burn-in** — do not pipe transcription directly to burn-in without a human pass, especially for any video mentioning price, dates, or contact info.

## Step 2: Caption Styling

Design principles for stop-scroll readability on mobile:

- **Font size:** large enough to read at arm's length on a phone screen without zooming — err larger, not smaller
- **Position:** center-bottom third is standard, but check platform UI overlays (like/comment buttons) don't cover it — center-middle is often safer for TikTok/Reels
- **Contrast:** white text with black/dark outline or semi-transparent background box — never rely on font color alone against a busy background
- **Word-by-word highlight (karaoke-style):** highlighting the currently-spoken word in a contrast color measurably increases watch-through on short-form video — worth the extra styling step for high-priority content
- **Line length:** 1-2 lines max visible at once; break sentences at natural pause points, not mid-clause

## Step 3: Burn-In (ffmpeg)

```bash
ffmpeg -i input.mp4 -vf "subtitles=input.srt:force_style='FontName=Montserrat Bold,FontSize=28,PrimaryColour=&H00FFFFFF,OutlineColour=&H00000000,BorderStyle=3,Outline=3,Shadow=0,Alignment=2,MarginV=80'" output_captioned.mp4
```

**Style parameter reference:**
- `PrimaryColour`: text color (BGR hex, `&H00FFFFFF` = white)
- `OutlineColour`: outline/background color (`&H00000000` = black)
- `BorderStyle=3`: opaque box behind text (better readability than outline-only on busy backgrounds)
- `Alignment=2`: bottom-center; `Alignment=5`: middle-center
- `MarginV`: vertical margin from edge — adjust to clear platform UI overlays

## Step 4: Word-Highlight Karaoke Captions (Optional, Higher Effort)

Requires word-level timestamps (Whisper supports this with `--word_timestamps True`), then a script to generate ASS (Advanced SubStation) format with per-word color animation:

```bash
whisper input.mp4 --language Vietnamese --word_timestamps True --output_format json
# Then use a script (e.g. a Python tool like `autocaption` or custom ASS generator)
# to convert word-level JSON timestamps into an .ass file with highlight animation
```

This is worth the extra step for hero/high-budget content; for daily batch volume, standard SRT burn-in is sufficient.

## Step 5: SRT vs Burn-In — When to Use Each

| Format | Use When |
|---|---|
| Burned-in (permanent, part of video pixels) | TikTok, Reels, Instagram feed — most viewers won't manually enable captions |
| Separate SRT file | YouTube (native caption support, improves SEO/accessibility, viewer can toggle) |
| Both | High-value content — burn in for social cuts, keep SRT for the YouTube long-form version |

## Vietnamese-Specific QA Checklist

- [ ] All diacritics render correctly (spot-check ư, ơ, ệ, ẫ, ữ — commonly mis-rendered by font substitution)
- [ ] Font chosen actually supports full Vietnamese character set (not all "nice" display fonts do — verify before committing to a brand font for captions)
- [ ] Mixed Vietnamese/English phrases read naturally, not garbled
- [ ] Numbers/prices/dates manually verified against source audio
- [ ] Captions don't overlap platform UI (test in-app preview, not just raw file)
