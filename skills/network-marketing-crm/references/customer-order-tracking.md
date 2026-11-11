# Customer Order & Reorder Tracking

Retail customers (people who buy product but aren't downline/IBOs) are a distinct tracking category — their value is recurring consumption, not rank progression.

## Sheet: Customer Roster

| Column | Type | Notes |
|---|---|---|
| Tên | Text | |
| SĐT / Zalo | Text | |
| Danh mục sản phẩm ưa thích | Dropdown | e.g. Nutrilite (thực phẩm bổ sung), Artistry (mỹ phẩm), Home care — adjust to actual product lines carried |
| Ngày đặt hàng gần nhất | Date | |
| Chu kỳ đặt hàng thường lệ | Number (days) | e.g. 30 days for a monthly supplement, 60-90 for skincare — derive from their own order history after 2-3 orders, don't guess upfront |
| Ngày dự kiến đặt lại | Formula | `= Ngày đặt hàng gần nhất + Chu kỳ đặt hàng thường lệ` |
| Tổng PV đã đóng góp | Number | Running total — useful for identifying your highest-value customers |
| Trạng thái | Dropdown | Active / Due for reorder / Lapsed (60+ days past due) |

## Reorder Reminder Cadence

Send a reminder **before** the predicted reorder date, not after — the goal is to catch the customer while they still have product, not after they've already switched to buying elsewhere or run out and lost the habit.

```
Reminder 1: 5-7 days before predicted reorder date
  "Chào [Tên], sắp hết [sản phẩm] chưa ạ? Mình đặt trước để không bị gián đoạn nhé."

Reminder 2 (if no response): 2-3 days after predicted date
  "[Tên] ơi, mình còn đủ [sản phẩm] dùng không? Đặt giúp mình 1 đơn nhé, có gì mình ship liền."

Reminder 3 (lapsed, 30+ days past due): treat as win-back, not a routine reminder
  Softer tone, ask if their needs changed rather than just repeating the ask
```

## Lapsed Customer Win-Back (60+ Days Past Due)

Reuse the mechanics from `skills/delivery-setup-guide/references/win-back-reengagement-sequence.md`, adapted:

```
Message A (Day 60): Check-in + reminder of the specific benefit they valued
Message B (Day 75): Ask directly if something changed (price, product fit, life circumstance) —
  genuine curiosity, not a sales pitch
Message C (Day 90): Last light-touch message, offer to stay in a low-frequency
  "just tips" list instead of routine reorder asks
```

## Prioritization: Not All Customers Deserve Equal Attention

Segment using two axes — order frequency and total PV contributed:

| Segment | Definition | Treatment |
|---|---|---|
| **Core** | Orders every cycle, high total PV | Proactive, personal — these are your PV base, protect the relationship |
| **Occasional** | Orders but irregularly | Standard reminder cadence, no extra effort |
| **One-time** | Single order, no repeat | Light win-back attempt once, then deprioritize — don't spend disproportionate time chasing a single past purchase |

## Metrics to Watch Monthly

```
Reorder rate = customers who reordered on schedule / customers due to reorder this month
Lapsed rate = customers now 60+ days past due / total active customers
Average PV per customer = total customer PV this month / number of active customers
```

A declining reorder rate is an earlier warning sign than a revenue drop — it shows up before the PV/BV numbers do, because it reflects behavior change before the cumulative effect hits the monthly total.
