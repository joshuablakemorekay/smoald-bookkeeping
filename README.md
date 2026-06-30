# SMOALD Expense Tracker

A simple bookkeeping system that helps me (a sole trader) record business income and expenses for my UK Self Assessment tax return — without relying on memory.

## What It Does

- Records every business income and expense in **one spreadsheet — the single source of truth**, stored in Google Drive.
- Keeps a **Recurring Expenses** checklist so regular costs (subscriptions, domain renewals) never get forgotten.
- Logs new payments straight from a receipt using a custom Claude skill — paste or forward a receipt and the row gets added for me.
- Converts foreign-currency receipts to GBP and flags them as estimates to confirm against my bank statement.
- Files receipts into the right tax-year folder automatically (UK tax year runs 6 April–5 April).

## Built With

- **Excel spreadsheet (.xlsx)** — the master tracker, with formulas that total income, expenses, and a rough tax estimate.
- **Python with openpyxl** — the script that reads a receipt and writes a new row into the spreadsheet. (openpyxl is a library — a ready-made set of code — for editing Excel files.)
- **Claude skill (a SKILL.md file)** — a set of instructions that teaches Claude to run the whole routine the same way every time.
- **Google Drive** — stores the master file so it's backed up and reachable anywhere.
- **Google Calendar** — a monthly reminder to run the expense check.

## What's in this repo

This is the **public, portfolio version** of the project. My live tracker holds real financial figures, so it stays private — what's published here is safe to share:

- **`template/SMOALD_bookkeeping_TEMPLATE.xlsx`** — a ready-to-use copy of the spreadsheet with the same tabs, formulas and HMRC notes, filled with clearly-labelled **sample data** instead of my real numbers. Download it, clear the sample rows, and it's yours.
- **`README.md`** — this file.
- **`JOURNAL.md`** — a running dev journal of what I built and learned, in chronological order.

The receipt-logging Python script and the Claude skill that drive the routine live privately for now. The bank-import automation (see **What's Next**) is the piece I'll be building in public here.

### What's inside the template

| Tab | What it's for |
| --- | --- |
| **Summary** | Auto-totals for income, expenses and net profit; a rough tax + NIC estimate; key HMRC dates; and a **per-tax-year breakdown** that fills itself in. |
| **HMRC Notes** | Plain-English summary of what HMRC requires — reporting threshold, what to keep, how long, and the Making Tax Digital dates. |
| **Recurring Expenses** | The monthly checklist of repeating costs, with self-explaining notes. |
| **Income** / **Expenses** | Where actual transactions go. Each has a **Tax Year** column that works out the UK tax year from the date automatically. |
| **Lists** | The dropdown options for categories and payment methods. |

## How to Run It

1. Open a chat with the `smoald-expense-tracker` skill installed.
2. Paste, forward, or upload a receipt.
3. Say "log this to my tracker."
4. Claude reads the details, adds the row, saves the file back to Drive, and files the receipt in the matching tax-year folder.
5. Once a month, when the calendar reminder pops up, open the **Recurring Expenses** tab and tick off what's been paid.

## My Journey

*In chronological order (oldest first). The full detail lives in [`JOURNAL.md`](JOURNAL.md).*

### 7 June 2026 — First expenses logged, and the £ vs $ lesson

Started logging real costs (domains, Google Workspace, PeoplePerHour) and immediately hit the rule that shapes everything since. **Key lesson:** the tracker is in £ but receipts often arrive in $ — always convert and flag the conversion as an estimate to confirm against the bank statement. And keep the supplier's own receipt as proof, not just a bank-app screenshot.

### 12 June 2026 — Built the `smoald-expense-tracker` skill

Turned the manual routine into a reusable Claude skill — a `SKILL.md` plus a small Python script — that reads a receipt, converts to £, adds a row, recalculates totals, and files the receipt into the right tax-year folder, with a duplicate guard so nothing is logged twice. **Design decisions:** built it as a portable *skill* rather than a hosted tool; made the same package work in both Claude.ai and Claude Code; and aimed for a hands-off setup via Google Drive for Desktop so Drive stays the single source of truth. **Key lesson:** a skill makes Claude *reliable*, not *autonomous* — I still kick it off with a receipt.

### 30 June 2026 — Recurring Expenses tab + treating Claude as ad-hoc

**What I built**

- A new **Recurring Expenses** tab — a checklist of costs that repeat, so I stop relying on memory.
- A **Type** column labelling each cost: Recurring, Variable, or Ad-hoc.
- Self-explaining notes in the tab (what it is, what it isn't, the rule, and a step-by-step monthly check).
- A monthly Google Calendar reminder to run the check.
- A draft report-only "monthly check" mode for the skill — it flags what's unlogged but never records on its own.
- Confirmed receipts can be read straight from Gmail and auto-filed to Drive — no manual downloading.

**What I learned**

- **A checklist and a record do different jobs** — the recurring tab is the plan, the Expenses tab is the record. Keeping them apart avoids double-counting.
- **Reminding can be automated; recording must not be** — recording always comes from a real receipt, never auto-filled from a list, because the receipt is the proof HMRC wants.
- Claude is a manual (non-auto-renewing) subscription for me, so it's logged as an ad-hoc one-off rather than a fixed monthly cost.

### 30 June 2026 — HMRC-aware tracker + public portfolio repo

I made the tracker do the tax thinking for me: a plain-English HMRC Notes tab, an automatic Tax Year column, and a per-tax-year totals breakdown on the Summary tab. Then I turned the project into this public repo with an anonymised template, so none of my real figures are exposed. **Key lesson:** a spreadsheet on its own is a thin portfolio piece — so I scaffolded the repo now and will build the bank-import automation in public, where the real engineering story lives.

## What's Next

- Wire the "pull from Gmail" step into the skill so receipts are fetched from my inbox and filed automatically as part of the routine (reading from Gmail already works — this makes it a standard step, not a manual ask).
- Wire the "monthly check" mode fully into the skill so it reconciles the recurring list against logged payments.
- Build a Python bank-import automation that reads my bank transactions and updates the sheet on its own — a Smoald Labs portfolio project.
