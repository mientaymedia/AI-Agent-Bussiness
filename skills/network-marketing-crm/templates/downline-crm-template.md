# Downline & Customer CRM — Sheet Template

Copy this structure into Google Sheets (3 tabs). Column headers exactly as below so future automation (Apps Script reminders, `/network-crm` command output) can rely on consistent field names.

```markdown
## Tab 1: Downline Roster
Tên | Ngày sponsor | SĐT/Zalo | Hạng hiện tại | PV cá nhân tháng này | PV nhóm tháng này | Ngưỡng hạng mục tiêu | % đạt ngưỡng | Lần liên hệ gần nhất | Lần check-in tiếp theo | Ghi chú

## Tab 2: Customer Roster
Tên | SĐT/Zalo | Danh mục sản phẩm ưa thích | Ngày đặt hàng gần nhất | Chu kỳ đặt hàng (ngày) | Ngày dự kiến đặt lại | Tổng PV đã đóng góp | Trạng thái

## Tab 3: Monthly Dashboard (rebuild at start of each month, pulling from Tab 1)
Tên | PV nhóm tháng này | Ngưỡng | % đạt | Trạng thái (🟢/🟡/🔴) | Hành động tiếp theo
```

## First-Setup Checklist

- [ ] Tab 1 created with all downline members currently sponsored (even if 0-3 people — start the habit now)
- [ ] Tab 2 created with all known retail customers
- [ ] Each customer's "Chu kỳ đặt hàng" estimated from their actual order history (leave blank until 2nd order if unknown — don't guess a number with only 1 data point)
- [ ] Weekly recurring calendar block set for CRM review (pick a fixed day/time)
- [ ] Month-end recurring block set 5 days before month close for the rank-threshold urgency pass

## Monthly Reset Checklist (Run on Day 1 of Each Month)

- [ ] Screenshot/archive last month's Tab 1 PV columns before clearing (for historical trend reference)
- [ ] Clear "PV cá nhân tháng này" and "PV nhóm tháng này" columns to zero
- [ ] Rebuild Tab 3 dashboard from fresh Tab 1 data
- [ ] Review last month's 🔴 Behind cases — did they recover, plateau, or churn? Note the pattern
