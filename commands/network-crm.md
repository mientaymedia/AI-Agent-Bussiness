---
description: Set up downline & customer tracking for a network-marketing business
argument-hint: [action]
---
Set up or run the network-marketing (Amway-style) downline and customer CRM.

<args>$ARGUMENTS</args>

## Actions
- `setup` - Create the initial downline + customer tracking sheet
- `review [month]` - Run the monthly PV/BV rank-qualification review
- `reorder` - Build/update the customer reorder reminder schedule
- `checkin` - Generate the list of overdue/due check-ins for this week

## Process
1. Activate Deliver Agent
2. Use Network Marketing CRM skill
3. Apply the downline tracking system + customer order tracking frameworks
4. Output: Ready-to-use spreadsheet structure or review report, PV/BV data sourced from what the user provides (never estimated)

## Examples
- `/network-crm setup` — Create the 3-tab downline/customer/dashboard sheet template
- `/network-crm review this month` — Sort downline by rank-threshold progress, flag who's behind
- `/network-crm reorder` — Build reorder reminder cadence for the customer list

## Tips
- PV resets every month — never carry last month's numbers forward
- Prioritize by % toward rank threshold, not headcount
- Pair with `/notification` to push overdue check-ins to Telegram automatically
