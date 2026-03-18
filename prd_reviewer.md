# PRD Reviewer — Smart Notification Center
**PRD reviewed:** 2026-03-18

---

## RISKY ASSUMPTIONS (top 3)

1. **"Users miss notifications" is assumed, not proven.**
   There's no data cited — no ticket volume, no session recordings, no user research. If users are actually ignoring notifications because they're low-value, adding a bell won't fix anything; it'll just surface noise they've already learned to tune out.

2. **Email digest is assumed to be a simple add-on.**
   Email delivery requires an ESP (SendGrid, SES, etc.), unsubscribe/compliance handling, and backend scheduling infrastructure. If none of this exists, "email digest option" is a hidden 3–4 week project on its own — not a feature checkbox.

3. **6 weeks is assumed to be enough with this team.**
   2 engineers + 1 designer for a notification system (bell UI, panel, read/unread state, backend storage, email digest, real-time or polling updates) is tight. No QA buffer, no mention of existing notification infrastructure. One integration surprise kills the timeline.

---

## MISSING FOR ENGINEERING (top 2)

1. **What is the notification data model and who creates notifications?**
   Engineers need to know: what triggers a notification, what fields it has (title, body, link, timestamp, type, user ID), where it's stored, how it's queried, and whether a backend notification service already exists. None of this is in the PRD.

2. **Does the bell update in real time or on page refresh?**
   This is a 1-day vs. 2-week decision. Real-time requires WebSockets or SSE infrastructure. Polling requires defining intervals and handling stale state. The PRD is silent on this, so engineers will block on it immediately.

---

## WEAK METRICS

**"More users see notifications"**
→ Unmeasurable. Replace with: *Notification panel open rate increases from X% to Y% within 60 days of launch.*

**"Reduce support tickets"**
→ No baseline, no target, no category. Replace with: *Support tickets tagged "missed update" or "didn't see notification" decrease by 30% within 90 days of launch, measured against prior 90-day baseline.*

---

## VERDICT

Not ready for engineering — the data model is undefined, the timeline is unvalidated, and neither success metric can actually be measured.
