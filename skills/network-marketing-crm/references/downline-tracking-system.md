# Downline Tracking System

Full field structure and workflow for tracking a network-marketing downline using a spreadsheet, before any volume justifies paid CRM software.

## Why Spreadsheet-First (Not a Paid CRM)

This applies the OCP resource-priority order: eliminate unnecessary work → standardize process → use a template → then automate → only then pay for tooling. A new network-marketing line has no validated volume yet — paying for CRM software before proving the business model wastes cash better spent on product samples or a first event. Revisit this decision only once tracking 50+ active relationships makes a spreadsheet genuinely slow to use.

## Sheet 1: Downline Roster

| Column | Type | Notes |
|---|---|---|
| Tên | Text | Full name |
| Ngày sponsor | Date | When they joined under you |
| SĐT / Zalo | Text | Primary contact channel |
| Hạng hiện tại | Dropdown | Founders/Silver/Platinum/Gold/Emerald/Diamond (adjust to actual Amway Vietnam rank names) |
| PV cá nhân tháng này | Number | Resets monthly — re-enter fresh each month, don't carry over |
| PV nhóm tháng này | Number | Their downline's total, if they have their own team |
| Ngưỡng hạng mục tiêu | Number | The PV/BV threshold they're working toward this month |
| % đạt ngưỡng | Formula | `=PV nhóm tháng này / Ngưỡng hạng mục tiêu` — sort by this to see who's closest |
| Lần liên hệ gần nhất | Date | |
| Lần check-in tiếp theo | Date | See cadence table below |
| Ghi chú | Text | Personal context — family situation, motivation, blockers |

## Check-In Cadence by Relationship Stage

| Stage | Cadence | Trigger |
|---|---|---|
| First 30 days (new IBO) | Every 3-4 days | Onboarding is the highest-churn window — most who quit, quit here |
| Active, consistent volume | Weekly | Maintain momentum, catch problems early |
| Below target, mid-month | Every 2-3 days as month-end approaches | Time-sensitive — PV resets, missed thresholds can't be recovered |
| Inactive (0 PV, 60+ days) | Monthly, low-pressure | Don't nag — a light "thinking of you" check-in beats silence, but don't over-invest time in someone who's disengaged |

## Sheet 2: Monthly Rank-Qualification Dashboard

Rebuild this view at the start of each month from Sheet 1:

```markdown
| Tên | PV nhóm tháng này | Ngưỡng | % đạt | Trạng thái | Hành động |
|---|---|---|---|---|---|
| [Name] | [current] | [target] | [%] | 🟢 On track / 🟡 At risk / 🔴 Behind | [specific next action] |
```

Sort by "% đạt" ascending — the people furthest behind need attention first, not the people already succeeding (who need less of your time, not more).

## Alert Logic (Manual or via a Simple Script)

```
IF % đạt < 50% AND days_remaining_in_month < 10:
  → 🔴 Behind — needs a direct conversation about what's blocking them, this week
IF % đạt between 50-85% AND days_remaining_in_month < 10:
  → 🟡 At risk — a nudge + specific suggestion (which 2-3 customers to follow up with) can close the gap
IF % đạt > 85%:
  → 🟢 On track — light touch, congratulate, ask if they need anything
```

## Automating the Reminder (If Using Google Sheets)

```javascript
// Google Apps Script: daily check for overdue check-ins
function checkOverdueFollowUps() {
  const sheet = SpreadsheetApp.getActiveSheet();
  const data = sheet.getDataRange().getValues();
  const today = new Date();
  const overdue = [];

  data.forEach((row, i) => {
    if (i === 0) return; // skip header
    const nextCheckIn = new Date(row[9]); // "Lần check-in tiếp theo" column
    if (nextCheckIn < today) {
      overdue.push(row[0]); // name
    }
  });

  if (overdue.length > 0) {
    // Send to Telegram via the notification-setup-guide skill's bot pattern
    // or simply log/email a daily digest
    Logger.log('Overdue check-ins: ' + overdue.join(', '));
  }
}
```

Pairs naturally with `skills/notification-setup-guide/` if Henry wants a daily Telegram digest of overdue check-ins instead of manually opening the sheet.

## Common Pitfalls

- **Tracking headcount instead of PV/BV**: number of downline members means nothing if their PV is zero — always sort/prioritize by performance data, not roster size
- **Treating this as a one-time setup**: the sheet only has value if reviewed weekly; schedule a recurring calendar block, don't rely on remembering
- **No distinction between "new" and "established" downline in check-in frequency**: burning out trying to weekly-check-in 30 established members while a brand-new IBO churns from neglect in their first week
- **Carrying PV numbers forward month to month**: PV resets — always re-enter fresh, a stale carried-over number produces false confidence about where the business actually stands
