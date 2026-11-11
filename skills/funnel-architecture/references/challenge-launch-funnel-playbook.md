# Challenge Launch Funnel Playbook (3-Day Shift → Build → Launch)

A community-based launch funnel that sells a backend offer (upsell, upgrade, or new product) by having the prospect generate their own proof that the system works, then pitching at peak motivation. This is the pattern behind most "community + challenge" launches in the Vietnamese digital product market (Zalo/Telegram groups running 3-day challenges before opening cart).

## When to Use

- Selling an upgrade/backend offer to an existing customer base (warm audience)
- Launching a new digital product where trust needs to be built fast (cold-to-warm in days, not weeks)
- You have (or can build) a community channel — Zalo group, Telegram group, Facebook group
- The product's value is easier to *feel* by doing a small piece of it than by reading about it

## Why It Outperforms a Direct Pitch

A direct sales page asks a cold prospect to trust a claim. A challenge funnel makes the prospect *experience a small win* first — so by the time the offer appears, they are not evaluating a claim, they are extending a result they already have. Conversion happens at the emotional peak (Day 3), not on cold logic.

```
Direct Pitch:     Ad → Sales Page → "Trust me, this works" → Buy (low trust, low conversion)
Challenge Funnel:  Optin → Community → Do → See Result → "It worked for me" → Buy (self-generated trust)
```

## Full Funnel Architecture

```
1. ENTRY (Optin)
   Landing page / existing customer list
   → Email captured, automated welcome email sent
   → Email contains: community join link + Day 1 access time

2. SELF-JOIN (Friction-reducing handoff)
   User clicks link in email → joins Zalo/Telegram group themselves
   → Group already shows social proof (member count, pinned activity)
   → Admin/bot pins Day 1 content immediately on join

3. DAY 1 — THE SHIFT (Mindset, zero pitch)
   Goal: Create urgency around the PROBLEM, not the product
   Content: "Why this matters now" — reframe their current approach as outdated/costly
   Format: Video/live + pinned text summary + 1 small task ("reply below with your biggest blocker")
   Success metric: Reply/engagement rate in group, NOT clicks

4. DAY 2 — THE BUILD (Value delivery, zero pitch)
   Goal: Walk them through building/doing a REAL piece of the system
   Content: Step-by-step, using the actual tool/method being sold
   Format: Live build or recorded walkthrough + template/checklist to copy
   Success metric: % who report completing the build task

5. DAY 3 — THE LAUNCH (Pitch at peak motivation)
   Goal: Open the offer right after they've seen their own result from Day 2
   Content: "Chạy thử" (run/test what they built) → show output → THEN present offer
   Structure: Recap transformation (Day1 mindset → Day2 build → Day3 result) →
              Present offer as the "next level" of what they just experienced →
              Bonus stack + deadline (24-72h cart close) → CTA
   Success metric: Offer click-through + purchase rate

6. SUPPORTING CHANNELS (Run throughout, not sequential)
   - Q&A lobby ("Sảnh Chat Q&A"): Real-time objection handling, visible to everyone (social proof of responsiveness)
   - Announcements channel: Reinforces deadline, surfaces testimonials from others completing the challenge
   - Admin/AI support persona (a named bot/assistant): Reduces perceived distance, answers FAQs instantly
```

## Content Ratio Per Day (Critical Rule)

**Day 1 and Day 2 must contain ZERO pitch.** The moment a challenge mentions price before Day 3, it collapses into a disguised sales page and loses the self-generated-trust effect. Value only, until the pre-planned pitch moment.

| Day | Value % | Pitch % | Primary Emotion Target |
|---|---|---|---|
| Day 1 (Shift) | 100% | 0% | Urgency / dissatisfaction with status quo |
| Day 2 (Build) | 90% | 10% (soft mention offer exists) | Competence / "I can do this" |
| Day 3 (Launch) | 30% | 70% | Momentum / "I already started, let's finish" |

## Building This for Your Own Product

Use this checklist to design your own 3-day challenge for any offer (course upgrade, SaaS onboarding, service package):

1. **Define the end offer first** (reverse-engineer, same principle as `funnel-stages-and-flow-design.md`) — price, what's included, the one result it delivers
2. **Design Day 2's "build task" as a miniature version of the paid offer** — the free task must produce a real, visible result using the same method the paid product scales up
3. **Write Day 1 as pure reframe** — identify the belief that keeps prospects using an inferior/manual approach, and dismantle it with one clear argument + one story
4. **Script the Day 3 pitch to reference Day 1 and Day 2 explicitly** — "Hôm qua bạn vừa [kết quả cụ thể]. Đây là cách bạn nhân kết quả đó lên." Never pitch a generic offer; pitch the next step from what they just did.
5. **Set a hard deadline for the offer** (24-72h) tied to a real reason (bonus expires, cohort closes, price increases) — not fake scarcity
6. **Staff the Q&A channel actively during all 3 days** — objection handling in public view converts lurkers, not just active repliers

## Automation Points (map to `delivery-setup-guide` skill)

- Optin → welcome email with community join link: use `skills/delivery-setup-guide/references/post-purchase-engagement-sequence.md` pattern (Message 1 template) adapted for "challenge access" instead of "product access"
- No-show tracking (joined group but didn't engage Day 1): trigger a nudge message, same mechanism as `skills/delivery-setup-guide/references/win-back-reengagement-sequence.md` Message A, but on a 24h cadence instead of 30-day
- Day 3 cart-close countdown: reuse the urgency/deadline block from Message 6 (upsell) in `post-purchase-engagement-sequence.md`

## Common Failure Modes

- **Pitching too early** (Day 1 or 2) — kills the self-generated-trust mechanic, reverts to cold pitch economics
- **Day 2 task too complex** — if <50% complete it, Day 3 pitch has no shared reference point; keep the build task achievable in under 30 minutes
- **No real deadline on Day 3** — without urgency, the "peak motivation" window closes and prospects drift to "I'll do it later" (which means never)
- **Q&A channel unstaffed** — visible unanswered questions during the highest-intent window (Day 3) actively kills conversions

## Metrics to Track

```
Day 1 engagement rate = replies or reactions / total joined
Day 2 completion rate = build-task submissions / total joined
Day 3 offer CTR = offer link clicks / total joined
Day 3 conversion rate = purchases / offer link clicks
Overall funnel conversion = purchases / total optins
```

**Reference benchmarks (community challenge funnels, Vietnamese digital products):**
- Day 1 engagement: 20-35% of joined members
- Day 2 completion: 10-20% of joined members
- Day 3 conversion (of those who completed Day 2): 15-30% — this is why the build task matters more than the pitch copy
