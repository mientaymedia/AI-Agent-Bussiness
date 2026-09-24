# Voice-of-Customer Mining

Upgrades raw market research ("what's the market size, what are the keywords") into research grounded in real customer language — the words prospects actually use to describe their pain, their hesitation, and their reason to buy. This is the gap that pure keyword/trend research leaves open: it tells you *that* demand exists, not *what to say* to convert it.

## Why This Matters

Generic research tells you a market exists. It does not tell you what headline, what objection-handling line, or what guarantee will make a specific prospect buy. The words customers use in reviews, comments, and complaints are the highest-fidelity input for writing offers and copy — because they are the prospect's own language, not the marketer's guess at it.

```
Keyword research → "there is demand for X"
Voice-of-Customer → "customers say 'I'm hesitant because I don't know how to code'"
                     → your offer headline becomes: 'No coding required'
```

This connects directly to `offer-packaging` and `copywriting`: VoC output is the raw material those skills turn into headlines, bullets, and objection responses.

## The Three Tools

### 1. Voice-of-Customer Extraction (Primary Tool)

Mine real customer language from reviews, comments, forum threads, and support tickets. Sort every extracted quote into exactly one of five buckets:

| Bucket | Definition | What to Search For |
|---|---|---|
| **Đau (Pain)** | The problem they're actively suffering from, in their own words | "vấn đề của tôi là...", "tôi mệt vì...", "khó chịu nhất là..." |
| **Muốn (Want)** | The outcome they're chasing, stated as a desire | "ước gì có...", "chỉ cần...", "tôi muốn..." |
| **Ngại (Hesitation)** | What stops them from buying/acting — objections in their own words | "ngại vì...", "sợ rằng...", "không chắc liệu..." |
| **Lý do quyết định mua (Purchase Trigger)** | The specific moment/reason that tipped them from considering to buying | "tôi mua vì...", "điều thuyết phục tôi là...", "sau khi thấy [X] thì tôi quyết định..." |
| **Từ ngữ khách hay dùng (Verbatim Language)** | Recurring words/phrases, slang, or framing the audience uses that differs from industry jargon | Track repeated exact phrases, not paraphrases |

**Sources to mine (in priority order):**
1. Product/course reviews on the exact niche (Shopee, app stores, course platforms)
2. Facebook group discussions and comments on competitor posts/ads
3. Reddit/forum threads discussing the problem space
4. Support tickets or DMs if you already have any customers
5. Comments under competitor YouTube/TikTok content in the niche

**Extraction rule:** Copy the exact quote, not a summary. A paraphrase loses the actual words a prospect would recognize in your copy. Tag each quote with source + date.

**Translation to offer/copy (the payoff step):**
```
Customer says: "Ngại vì không biết lập trình"
→ Offer headline leads with: "Không cần biết code"

Customer says: "Tôi mệt vì phải tự làm content mỗi ngày"
→ Offer headline leads with: "Ngừng tự viết content mỗi ngày"

Customer says: "Sợ mua xong không dùng được, phí tiền"
→ Guarantee copy addresses this directly: "Hoàn tiền nếu sau 3 ngày bạn chưa thấy kết quả"
```

### 2. Search Query Playbook

A reusable library of query patterns — the point is speed and coverage, not reinventing search terms per project. Run all relevant categories, not just the first one that returns results.

**Demand & sizing queries:**
```
"[niche] thị trường quy mô [năm hiện tại]"
"[niche] market size [current year] Vietnam"
"[niche] tăng trưởng % năm"
```

**Voice-of-Customer queries (pain/want/hesitation):**
```
"[niche] review site:shopee.vn"
"[niche] đánh giá thật"
"[niche] nhược điểm" / "[niche] nhược điểm là gì"
"[niche keyword] site:reddit.com"
"[niche] group facebook -site:facebook.com/ads"
"[competitor name] review kém" / "[competitor name] scam" (surfaces unmet needs directly)
```

**Purchase-trigger queries:**
```
"tại sao tôi mua [niche/product]"
"[niche] lý do chọn"
"[product] review sau khi dùng 1 tháng"
```

**Competitor messaging queries:**
```
"[competitor] facebook ads library"
"[competitor] sales page"
site:facebook.com/ads/library "[competitor name]"
```

Run queries across at least 2 categories (demand + VoC) before writing any market research report — sizing alone is an incomplete research pass under this methodology.

### 3. Source Triangulation (Hard Rule)

**Every number in a research report must carry a source and a date, and must be cross-checked against at least 3 independent sources before being used.** This is a strict upgrade over "search and report whatever the first result says" — it exists specifically to prevent fabricated or unverifiable statistics.

**Enforcement checklist per number:**
- [ ] Source named (URL or named publication, not "industry reports say")
- [ ] Date of the data point stated (not just the search date — the data's own as-of date)
- [ ] Cross-checked against 2 additional independent sources; if they disagree by more than ~20%, report the range and flag the discrepancy rather than picking one
- [ ] If fewer than 3 sources exist for a number, state that explicitly ("chỉ tìm được 1 nguồn, chưa đối chiếu được") rather than presenting it as confirmed

**Why this matters for Henry's positioning:** A research report that presents a fabricated or unverifiable TAM figure is worse than no figure — it produces a confident wrong decision. State uncertainty honestly rather than manufacturing false precision.

## Workflow: How to Run a VoC-Enhanced Research Pass

```
1. Run standard market sizing + trend queries (see SKILL.md Live Research Protocol Step 1)
2. Run Voice-of-Customer queries against review sites, forums, and competitor comment sections
3. Extract 15-30 verbatim quotes minimum; sort into the 5 buckets
4. Identify the 3-5 most-repeated phrases per bucket (repetition = signal, a single quote is anecdote)
5. Cross-check every sizing/demand number against 3 sources; log source + date for each
6. Compile the Voice-of-Customer Table (see template below) as a first-class section of the report, not an appendix
7. Flag 1-3 niches worth deeper investigation, using both demand size AND how strong/specific the VoC signal was
```

## Voice-of-Customer Table Template

```markdown
## Voice-of-Customer: [Niche/Product Name]

**Sources mined:** [list URLs/platforms, with dates accessed]
**Quotes collected:** [N]

### Đau (Pain)
- "[verbatim quote]" — [source, date]
- "[verbatim quote]" — [source, date]

### Muốn (Want)
- "[verbatim quote]" — [source, date]

### Ngại (Hesitation)
- "[verbatim quote]" — [source, date]

### Lý do quyết định mua (Purchase Trigger)
- "[verbatim quote]" — [source, date]

### Từ ngữ khách hay dùng (Verbatim Language)
- [recurring phrase/word] — appeared [N] times across sources

### Translation to Offer/Copy
| Customer language | Implication for offer/copy |
|---|---|
| "[quote]" | [headline/bullet/guarantee this suggests] |
```

## Self-Check Before Delivering a Research Report

- [ ] Voice-of-Customer table present with all 5 buckets populated (or explicitly marked "insufficient data" for any bucket that couldn't be filled)
- [ ] Every number has a source + date
- [ ] Every key number cross-checked against 3 sources (or flagged as unverified if fewer were found)
- [ ] 1-3 niches selected for deeper follow-up, with reasoning tied to both demand size and VoC strength
