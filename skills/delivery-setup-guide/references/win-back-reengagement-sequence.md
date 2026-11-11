# Win-Back & Re-engagement Sequence

Automated sequence for customers who go quiet — no product usage, no reply, no repeat purchase after the post-purchase engagement sequence ends (Day 14+). Goal: recover lapsed customers before they churn permanently or ask for a refund.

**Trigger conditions (any one qualifies a customer for win-back):**
- No login/access-link click for 30 days after purchase
- Post-purchase Message 6 (upsell) sent with zero click, and 30+ days since purchase
- Subscription/renewal product: no renewal within 7 days of expiry
- Abandoned checkout: payment session created but never completed (VietQR scanned, not paid, within 24h)

**Sequence Timeline:** 3 messages over 30 days, triggered independently from the initial 14-day post-purchase sequence.

```
Day 30 (Silent) → Message A: Check-in + Value Reminder
Day 45 (No response) → Message B: Obstacle Removal + Incentive
Day 60 (No response) → Message C: Last Call / Downgrade Offer
```

---

## Message A: Check-in + Value Reminder (Day 30)

### Timing
30 days after purchase with no engagement signal (no access-link click logged)

### Channel
Email (primary) + Telegram if opted in

### Email

**Subject:** 👀 [{{productName}}] Bạn còn ở đó không?

**Body:**
```
Xin chào {{customerName}},

Mình để ý bạn chưa quay lại {{productName}} kể từ khi mua.
Không sao cả — bận rộn là chuyện bình thường.

Nhắc lại nhanh những gì đang chờ bạn:
- {{keyBenefit1}}
- {{keyBenefit2}}
- {{keyBenefit3}}

→ Truy cập lại ngay: {{accessLink}}

Chỉ cần 15 phút hôm nay để lấy lại đà. Bạn đã trả tiền cho kết quả này rồi — đừng để nó nằm im.

Trả lời email này nếu bạn đang bị vướng ở bước nào, mình hỗ trợ trực tiếp.

{{yourName}}
```

### Automation Trigger
```javascript
async function checkWinBackEligibility() {
  const day30Orders = await db.orders.findAll({
    where: {
      status: 'completed',
      createdAt: { $lte: daysAgo(30), $gte: daysAgo(31) }
    }
  });

  for (const order of day30Orders) {
    const hasEngaged = await hasAccessLinkClick(order.id);
    if (!hasEngaged) {
      await sendWinBackMessageA(order);
      await logEngagementMessage(order.id, 'winback_a', 'sent');
    }
  }
}
```

---

## Message B: Obstacle Removal + Incentive (Day 45)

### Timing
15 days after Message A, only if still no engagement

### Channel
Email + Telegram

### Email

**Subject:** 🎁 [{{productName}}] Mình gỡ rào cản cho bạn rồi

**Body:**
```
Xin chào {{customerName}},

Mình đoán có gì đó đang cản bạn quay lại {{productName}}. Thường là 1 trong 3 lý do:

1. "Không có thời gian" → Đây là phiên bản rút gọn 15 phút: {{quickStartLink}}
2. "Không biết bắt đầu từ đâu" → Đây là lối tắt: {{shortcutGuideLink}}
3. "Bị kẹt kỹ thuật" → Đặt lịch hỗ trợ 1-1 miễn phí 15 phút: {{bookingLink}}

Ngoài ra, để khuyến khích bạn quay lại tuần này, mình tặng thêm:
🎁 {{winbackBonus}} (giá trị {{bonusValue}}) — chỉ cần bạn truy cập lại trước {{deadlineDate}}

→ {{accessLink}}

{{yourName}}
```

### Automation Trigger
```javascript
async function sendWinBackMessageB(order) {
  const bonus = getWinbackBonus(order.product.id);
  await sendEmail({
    to: order.customer.email,
    subject: `🎁 [${order.product.name}] Mình gỡ rào cản cho bạn rồi`,
    template: 'winback-b-incentive',
    variables: { ...order, bonus }
  });
  await logEngagementMessage(order.id, 'winback_b', 'sent');
}
```

---

## Message C: Last Call / Downgrade Offer (Day 60)

### Timing
15 days after Message B, only if still no engagement. Final message in the win-back sequence — do not send further automated emails after this.

### Channel
Email only

### Email

**Subject:** 👋 [{{productName}}] Email cuối cùng về việc này

**Body:**
```
Xin chào {{customerName}},

Đây là email cuối mình gửi về {{productName}} theo hướng này.

Hai lựa chọn:

A. Bạn vẫn muốn dùng → {{accessLink}} — mình giữ nguyên hỗ trợ cho bạn, bất cứ lúc nào.

B. Không còn phù hợp lúc này → Không sao. Trả lời email này với chữ "STOP", mình sẽ ngừng nhắc và chuyển bạn sang danh sách chỉ nhận nội dung giá trị (không bán hàng).

Cảm ơn bạn đã tin tưởng {{storeName}}.

{{yourName}}
```

### Automation Trigger
```javascript
async function sendWinBackMessageC(order) {
  await sendEmail({
    to: order.customer.email,
    subject: `👋 [${order.product.name}] Email cuối cùng về việc này`,
    template: 'winback-c-lastcall'
  });
  await logEngagementMessage(order.id, 'winback_c', 'sent');
  await markCustomerStatus(order.customer.id, 'winback_completed');
}
```

---

## Abandoned Checkout Variant (24h trigger)

For VietQR payments where the QR was generated but never paid:

**Timing:** 2 hours after QR generation if no SePay webhook confirmation received, then once more at 24 hours.

**Message (Telegram/Zalo preferred — customer is mid-intent, needs speed):**
```
Chào {{customerName}}, bạn vừa quét mã QR cho {{productName}} nhưng giao dịch chưa hoàn tất.

Nếu gặp lỗi thanh toán, đây là link QR mới: {{newQRLink}}
Cần hỗ trợ ngay? Nhắn tin tại đây: {{supportLink}}
```

```javascript
async function checkAbandonedCheckouts() {
  const pending = await db.paymentSessions.findAll({
    where: { status: 'pending', createdAt: { $lte: hoursAgo(2) } }
  });
  for (const session of pending) {
    if (!session.abandonedReminderSent) {
      await sendAbandonedCheckoutReminder(session);
      await markReminderSent(session.id);
    }
  }
}
```

## Integration with Post-Purchase Sequence

Win-back is a **separate, independent trigger** from `post-purchase-engagement-sequence.md` — it does not run for customers who are actively engaging (clicking links, replying, using the product). Check engagement signals before triggering; never send win-back to an already-active customer.

```
Post-Purchase Sequence (Day 0-14) → engaged? → Continue normal lifecycle, no win-back
                                   → silent?  → Win-Back Sequence (Day 30/45/60)
```

## Metrics to Track

```javascript
async function getWinBackMetrics(startDate, endDate) {
  return {
    winbackAReopenRate: await getOpenRate('winback_a', startDate, endDate),
    winbackBReactivationRate: await getReactivationRate('winback_b', startDate, endDate),
    winbackCUnsubscribeRate: await getUnsubscribeRate('winback_c', startDate, endDate),
    abandonedCheckoutRecoveryRate: await getRecoveryRate(startDate, endDate),
    overallWinBackConversionRate: await getOverallReactivationRate(startDate, endDate)
  };
}
```

**Benchmarks:**
- Message A reopen/reactivation: 15-25%
- Message B reactivation (with incentive): 8-15%
- Message C: mostly list hygiene (unsubscribe/downgrade), not revenue
- Abandoned checkout recovery: 20-35% (much higher — customer already had payment intent)
