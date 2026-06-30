# SMOALD Expense Tracker — Dev Journal

> A development journal for the SMOALD bookkeeping project, in chronological order (oldest first). Append-only — never delete older entries.

---

## 7 June 2026 — First real expenses logged, and the £ vs $ lesson

**Type:** Milestone

**TL;DR**
- Started using the tracker for real by logging the first business costs.
- Learned the hard rule that shapes everything since: the tracker is in £, but receipts often arrive in $ — so conversions must be flagged, not trusted.

**What I built**
Logged the first genuine SMOALD costs into the Expenses tab: two domain purchases (Namecheap for `smoald.com`, and a separate DomainAgents payment), Google Workspace, and PeoplePerHour. Each row got a date, category, supplier, payment method, and amount, and the Summary tab totalled them up automatically.

**Why I did it this way**
A tracker only earns its keep once real numbers go in. Logging these early made the gaps obvious — like the fact that some receipts don't show a £ figure at all.

**What I learned**
- **Always convert to the tracker's currency (£), and flag any conversion as an estimate** to correct against the bank statement later. The Namecheap receipt only showed $57.02, so the £ figure was an estimate until the real charge landed.
- **Keep the receipts, not just the spreadsheet.** HMRC can ask for proof, so receipts get saved as PDFs in a dated folder and kept for around five years.
- **A bank-app screenshot is weaker proof than the supplier's own invoice** — always grab the real receipt where possible.
- Two payments that look similar can be genuinely different transactions (different payee, card, even domain spelling) — worth checking before assuming.

**How We Did It**
Read each receipt, converted USD amounts to £ using the rate shown on the receipt, mapped each cost to an existing category, appended the rows, and saved the receipts as PDFs for the records.

---

## 12 June 2026 — Built the `smoald-expense-tracker` skill

**Type:** Milestone

**TL;DR**
- Turned the manual "log a receipt" routine into a reusable skill, so it happens the same way every time instead of me re-explaining it.
- Settled on one big decision: Google Drive is the single source of truth — no second local master to drift out of sync.

**What I built**
A skill made of two parts: a `SKILL.md` (the written instructions that teach Claude the routine) and a small Python script, `add_entry.py`, that does the actual spreadsheet edit. Hand it a receipt and it reads the details, converts everything to £, maps the cost to one of my existing categories, appends a row, recalculates the totals, and files the receipt into `SMOALD receipts` → the correct tax-year subfolder (worked out automatically from the receipt's date — UK tax year, 6 April to 5 April).

**Why I did it this way**
I kept doing the same fiddly steps by hand for every receipt. A skill writes that routine down once so it runs identically each time — and it bakes in the rules I'd been applying manually: convert to £ and flag conversions as estimates, add a fresh row for each month of a recurring cost, and a duplicate guard so the same payment can't be logged twice.

**Design decisions**
- **A skill, not a hosted tool.** I built this as a *Skill* — portable instructions plus a small script — rather than a hosted tool or MCP server. The job is a repeatable routine, not custom software that needs hosting, and Skills follow an open standard, so the same package works across Claude apps.
- **Works in two environments.** The same skill runs in Claude.ai (where I do one manual "replace the file" drag at the end) and in Claude Code (where it can save straight into a synced folder, so even that drag disappears).
- **Hands-off via Drive for Desktop.** The cleanest fully-automatic setup is Claude Code saving into a Google Drive for Desktop synced folder — Google then handles the upload in the background, so Drive stays the single source of truth without a second master file to drift out of sync. (I first assumed Claude Code was overkill, then changed my mind once I learned the connector couldn't reliably save a binary file back — a good reminder to update a decision when new information arrives.)

**What I learned**
- **Pre-trading expenses** — costs from *before* the business officially started can still count if they were genuinely for the business (within 7 years of trading), with a note explaining the reasoning.
- **"Wholly and exclusively"** — an expense has to be for the business to be claimable; mixed-use costs are claimed at the business proportion only.
- A skill makes Claude *reliable*, not *autonomous* — it still needs me to kick it off with a receipt; it doesn't run on a schedule by itself.
- The Drive connector can't safely overwrite a binary Excel file, so the honest fallback is the skill handing the file back for a one-drag "replace existing" — better than risking a corrupted tax record.

**How We Did It**
Drafted the SKILL.md and the Python script, tested it on a copy (append worked, the duplicate guard refused a re-entry, totals recalculated), then packaged it so it works in both Claude.ai and Claude Code.

---

## 30 June 2026 — Recurring Expenses tab + treating Claude as ad-hoc

**Type:** Feature

**What I built**
- Added a new **Recurring Expenses** tab to my tracker — a checklist of costs that repeat, so I stop relying on memory.
- Added a **Type** column so every cost is labelled clearly: Recurring (fixed monthly or annual), Variable, or Ad-hoc (I decide when to pay).
- Wrote self-explaining notes straight into the tab: what it is, what it isn't, the rule for recurring vs one-offs, and a step-by-step "monthly check" right above the table.
- Set up a **monthly Google Calendar reminder** (the 10th, with a popup the day before and on the day) to nudge me to run that check.
- Drafted a report-only "monthly check" mode for my `smoald-expense-tracker` skill — it flags anything I haven't logged yet but never records anything on its own.
- Confirmed receipts don't need manual downloading — they can be read straight from Gmail and a copy auto-filed to Drive, taking the friction out of logging.
- Logged my 30 June Claude Pro payment (£18) as a one-off in the Expenses tab.
- Removed an entry that turned out not to be a company expense — a reminder to verify every line genuinely belongs to the business before it counts.

**What I learned**
- **A checklist and a record are two different jobs.** The Recurring Expenses tab is the *plan* (what should go out); the Expenses tab is the *record* (what actually went out). Keeping them separate stops double-counting.
- **Reminding can be automated; recording must not be.** A list can remind me to check, but it can't prove a payment happened — so recording always comes from a real receipt, never auto-filled from the list. That proof is what HMRC wants.
- Claude is a manual subscription for me (no auto-renew), so it behaves like a one-off, not a fixed monthly cost. I marked it "ad-hoc" instead of faking an £18/month line that might not happen.
- A bit of HMRC groundwork: pre-trading costs can still count if they were genuinely for the business, and receipts should be kept for around 5 years.

---

## 30 June 2026 — HMRC notes, auto tax-year totals, and a public portfolio repo

**Type:** Feature / Milestone

**TL;DR**
- Made the tracker HMRC-aware: an HMRC Notes tab, an automatic Tax Year column, and per-tax-year totals.
- Published it as a public portfolio repo with an anonymised template — no real financial data.

**What I built**
Widened cramped columns so text fits, added a plain-English HMRC Notes tab, an auto Tax Year column on Income and Expenses, and a per-tax-year totals table on the Summary tab.

**Why I did it this way**
I wanted filing-time answers to appear on their own, so the totals and tax years are driven by formulas, not manual typing.

**How it works**
A SUMIF formula (a built-in spreadsheet function that adds up numbers matching a label) groups each entry by the UK tax year — 6 April to 5 April — worked out from its date. The boundary logic was tested at the edges (5 April lands in the old year, 6 April in the new one).

**What I learned**
A spreadsheet on its own is a thin portfolio piece — the real engineering story is the automation still to come, so I scaffolded the repo now to build it in public. Anonymising the real sheet into a sample template before publishing kept private financial data out of a public repo.

**How We Did It**
Fixed the column widths, added the HMRC Notes tab from verified gov.uk rules, built the auto Tax Year column and the per-tax-year totals, anonymised the sheet into a sample template, then wrote the docs and published the GitHub repo.

**What's next**
The centrepiece I'm building in public here is a Python bank-import automation: read bank transactions, detect recurring payments, and update the sheet automatically — turning the manual monthly check into something hands-off, with the spreadsheet and skill as the foundation.

---
