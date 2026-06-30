# SMOALD Expense Tracker — Dev Journal

> Newest entries at the top. Append-only — never delete older entries.

---

## 30 June 2026 — Recurring Expenses tab + treating Claude as ad-hoc

**What I built**

- Added a new **Recurring Expenses** tab to my tracker — a checklist of costs that repeat, so I stop relying on memory.
- Added a **Type** column so every cost is labelled clearly: Recurring (fixed monthly or annual), Variable, or Ad-hoc (I decide when to pay).
- Wrote self-explaining notes straight into the tab: what it is, what it isn't, the rule for recurring vs one-offs, and a step-by-step "monthly check" right above the table.
- Set up a **monthly Google Calendar reminder** (the 10th, with a popup the day before and on the day) to nudge me to run that check.
- Drafted a report-only "monthly check" mode for my `smoald-expense-tracker` skill — it flags anything I haven't logged yet but never records anything on its own.
- Logged my 30 June Claude Pro payment (£18) as a one-off in the Expenses tab.
- Removed a Cloudflare entry that turned out to be a colleague's payment, not my company's.

**What I learned**

- **A checklist and a record are two different jobs.** The Recurring Expenses tab is the *plan* (what should go out); the Expenses tab is the *record* (what actually went out). Keeping them separate stops double-counting.
- Recording must always come from a real receipt — never auto-filled from a list. A list can remind me, but it can't prove a payment happened, and that proof is what HMRC wants.
- Claude is a manual subscription for me (no auto-renew), so it behaves like a one-off, not a fixed monthly cost. I marked it "ad-hoc" instead of faking an £18/month line that might not happen.
- A bit of HMRC groundwork: pre-trading costs can still count if they were genuinely for the business, and receipts should be kept for around 5 years.

---
