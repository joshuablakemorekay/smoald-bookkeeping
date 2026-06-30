# SMOALD Expense Tracker — Dev Journal

> Newest entries at the top. Append-only — never delete older entries.

---

## 30 June 2026 — HMRC notes, auto tax-year totals, and a portfolio repo

**Type:** Feature / Milestone

**TL;DR**
- Made the tracker HMRC-aware: an HMRC Notes tab, an automatic Tax Year column, and per-tax-year totals.
- Published it as a public portfolio repo with an anonymised template — no real financial data.

**What I built**
Widened cramped columns so text fits, added a plain-English HMRC Notes tab, an auto Tax Year column on Income and Expenses, and a per-tax-year totals table on the Summary tab.

**Why I did it this way**
I wanted filing-time answers to appear on their own, so the totals and tax years are driven by formulas, not manual typing.

**How it works**
A SUMIF formula (a built-in spreadsheet function that adds up numbers matching a label) groups each entry by the UK tax year — 6 April to 5 April — worked out from its date.

**What I learned**
A spreadsheet on its own is a thin portfolio piece — the upcoming bank-import automation is the real engineering story, so I scaffolded the repo now to build it in public.

**How We Did It**
Fixed the column widths, added the HMRC Notes tab from verified gov.uk rules, built the auto Tax Year column and the per-tax-year totals, anonymised the sheet into a sample template, then wrote the docs and published the GitHub repo.

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
