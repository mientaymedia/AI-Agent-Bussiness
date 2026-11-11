---
name: network-marketing-crm
description: "Downline and customer tracking system for network-marketing (Amway-style) businesses — PV/BV performance tracking, follow-up cadence, and rank-qualification monitoring using lightweight no-code tools."
---

# Network Marketing CRM Skill

## When to Use

Activate this skill when:
- Setting up tracking for a network-marketing business (Amway or similar PV/BV-based compensation plan) with no CRM yet
- Managing a downline (sponsored IBOs) alongside a personal retail customer list
- Monitoring monthly PV/BV performance against rank-qualification thresholds
- Building a follow-up system so prospects and customers don't fall through the cracks
- Replacing scattered notes/memory with a single source of truth for the business

This is the "Care" layer for a network-marketing business — it doesn't sell or recruit (see `network-marketing-funnel`), it keeps the business from leaking revenue and relationships through poor follow-up.

## Why This Matters More in Network Marketing Than a Normal Business

A network-marketing income depends on **two moving parts that reset every month**: personal PV (product volume you and your retail customers consume) and group PV/BV (your downline's volume). Missing a follow-up with one customer, or losing track of which downline member is close to a rank threshold, has a direct and immediate income consequence — unlike a normal e-commerce business where a missed follow-up just delays one sale.

## Core Data Model

Track three entity types, not just "contacts":

| Entity | Key Fields | Why Separate |
|---|---|---|
| **Downline (sponsored IBO)** | Name, sponsor date, current rank, monthly PV, monthly group PV/BV, rank-qualification target, last contact date, next check-in date | Their performance affects *your* income directly — needs proactive monitoring, not just reactive follow-up |
| **Retail Customer** | Name, product category preference, last order date, order frequency (monthly/occasional), reorder due date, lifetime PV contributed | Recurring product consumption is your personal PV base — track reorder cadence like a subscription business would |
| **Prospect (not yet joined)** | Name, source (how they entered your funnel), stage (invited/attended presentation/considering/declined), last contact date, next follow-up date | Feeds `network-marketing-funnel` — see that skill for the qualification pipeline this connects to |

## Recommended Tool: Spreadsheet First, Not a Paid CRM

Given this is a new/unvalidated line of business (see `references/downline-tracking-system.md` for the full rationale), start with a Google Sheet or Airtable base, not a paid CRM subscription. A network-marketing business with under ~20 downline members and ~50 customers does not need paid CRM software — it needs discipline and a template. Upgrade only once volume outgrows a spreadsheet's usability (typically 50+ active relationships to track).

## Reference Files

- **[Downline Tracking System](references/downline-tracking-system.md)**: Full field list, monthly PV/BV monitoring workflow, rank-qualification alert logic, check-in cadence by relationship stage
- **[Customer Order & Reorder Tracking](references/customer-order-tracking.md)**: Reorder reminder cadence by product category, lifetime-value tracking, win-back triggers for lapsed customers

## Monthly Cycle (Critical — PV Resets Every Month)

```
Day 1 of month: PV/BV counters reset to zero — review last month's close first
Week 1: Check every downline member's prior-month result, log rank movement
Week 2-3: Active follow-up window — this is when volume can still change this month's outcome
Last week: Urgency check — who is close to a threshold and needs a nudge before month-end cutoff
Day 1 next month: Repeat
```

**Do not run this system as a one-time setup.** The entire value is the monthly cadence — a CRM that isn't reviewed weekly is just a spreadsheet nobody looks at.

## Compliance Note

Never record or communicate projected/guaranteed income figures to downline members or prospects based on this tracking data — PV/BV tracking is for your own operational visibility, not for making income claims to others. See `network-marketing-funnel/references/vietnam-mlm-compliance-basics.md` for what is and isn't legal to say when discussing this data with a prospect.

## Usage Examples

**Initial Setup**:
```
Set up my downline and customer tracking sheet. I have 3 downline members and
about 12 retail customers so far, tracking Nutrilite and home care products.
```

**Monthly Review**:
```
Review this month's PV/BV data: [paste or describe]. Who's close to a rank
threshold and needs a check-in before month-end?
```

**Reorder Cadence**:
```
Build a reorder reminder schedule for my customer list based on typical
consumption cycles for [product category].
```
