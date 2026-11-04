# Trueup — Experience

> **Status:** Draft · pending owner approval
>
> **Answers:** how the owner uses Trueup — interaction principles, navigation, one flow per job to
> be done, the screens and their states, how numbers, dates and currencies are displayed, and the
> visual language.
>
> **Does not answer:** why Trueup exists and which capabilities it has (`docs/product.md`) ·
> concepts, rules and invariants (`docs/domain.md`) · measurable quality targets, including use on
> a phone (`docs/quality-attributes.md`) · technology, frameworks and libraries
> (`docs/architecture.md`, `docs/adr/`) · the detailed behaviour of each screen
> (`openspec/specs/`).

---

## 1. How to read this document

- Terms defined in `docs/domain.md`, 2, such as **figure**, **review item** and **reconciled
  period**, and in `docs/quality-attributes.md`, 1, keep their meaning here. Capabilities are
  named as in `docs/product.md`, 7.1, and jobs as in `docs/product.md`, 4 (J1 to J8).
- This document also uses:

| Term | Meaning |
|---|---|
| **Area** | One of the five top-level parts of Trueup that navigation leads to (3.1) |
| **Screen** | A view the owner can open. Section 5 lists them all |
| **Open** | To tap or click something, which shows its detail |
| **Mark** | A short label attached to a figure, an account or a period, stating one of its completeness flags (`IND-2`) or that it contains converted amounts (6.1) |
| **Notice** | A message at the top of the dashboard about Trueup as a whole rather than about one figure (6.3) |
| **Phone layout** · **computer layout** | The single-column arrangement of a screen on a phone, and the multi-column arrangement on a computer (8.5) |

- Wording quoted from screens, such as "+ 22.00 pending review", is illustrative: the language of
  the interface is open (`docs/backlog.md`, B-12).
- Examples are invented. As in `docs/domain.md`, 5.2, the reporting currency in examples is EUR;
  its amounts are written without a currency, and amounts in other currencies with their code.

## 2. Interaction principles

These apply to every screen. They serve the product principles (`docs/product.md`, 6); when one
of them conflicts with a product principle, the product principle wins.

1. **The weekly habit fits a phone.** What the owner does each week — reading the dashboard,
   working through review, entering a transaction, confirming or skipping an expected one — can be
   completed in the phone layout (`docs/quality-attributes.md`, PHN-1 to PHN-3). Reconciliation,
   statement files, configuration and the one-time import are designed for a computer; they open
   on a phone, but may need a computer.
2. **The answer first, the evidence one action away.** Each screen leads with the figure or the
   decision it exists for. Opening a figure shows every input it reads
   (`docs/quality-attributes.md`, AUD-1); opening a transaction shows how it came to be what it is
   (AUD-2).
3. **Marks travel with figures.** A figure is shown with its marks (6.1) wherever it appears, and
   opening a mark shows what it is about.
4. **Pending stays apart.** A proposed transaction never changes a figure (`TX-3`). What it would
   add is shown next to the figure, separately (6.1).
5. **Every answer can become a rule.** Wherever the owner decides an account, category,
   counterparty or description, the same action can save the decision as a rule
   (`docs/product.md`, 6, principle 5).
6. **A field is pre-filled only from what determines it:** the captured record, a rule, a plan,
   an expectation, or the only possible value, as when an account with one currency gives that
   currency. Otherwise the field starts empty (`docs/product.md`, 6, principle 1).
7. **A refusal says why.** When Trueup refuses a change, it says why in plain words, stores none of
   it (`docs/quality-attributes.md`, COR-2), and keeps what the owner entered on screen so she can
   correct it. When Trueup knows beforehand that an action would be refused, such as changing a
   transaction in a reconciled period, it does not offer the action and shows the reason instead.

## 3. Navigation

### 3.1 Areas

| Area | What it holds | Jobs |
|---|---|---|
| **Dashboard** | The notices (6.3) and the questions of `docs/product.md`, 7.3. Trueup opens on it | J1, J2, J4, J5, J6 |
| **Review** | Every transaction captured since the owner's last review (4.3.1) | J2, J3 |
| **Transactions** | The whole record — confirmed, proposed and void transactions — and entering one by hand | J3 |
| **Accounts** | Every account with its balances and, by kind, its cycles, plans, open items and reconciliations | J3, J5 |
| **Settings** | The financial structure, rules, expectations, budgets, currencies, workspace settings, security, the export and the one-time import | J7, J8 |

Signing in comes before any area (`docs/product.md`, 7.1 `access`).

### 3.2 Layouts

- **Phone layout.** A bar fixed at the bottom of the screen holds the five areas, each with an
  icon and a label. Review shows the number of review items waiting for a decision. Entering a
  transaction by hand starts in one action from Dashboard, Review and Transactions.
- **Computer layout.** A bar at the top holds the horizontal logo on the left, Dashboard, Review,
  Transactions and Accounts in the centre, and Settings on the right.
- Wherever a figure, a transaction, an account or a review item appears, opening it leads to its
  screen, and going back returns to where she was.

## 4. Flows

One flow per job to be done (`docs/product.md`, 4). Screens are those of section 5.

### 4.1 J1 — Know where I stand

1. She opens Trueup and signs in, with a second factor on a new device
   (`docs/quality-attributes.md`, SEC-2).
2. The dashboard shows any notice first (6.3), then the section "Am I on track this month?".
3. The section leads with the **pace chart**, the main view of the month's pace: spending this
   month, cumulative day by day, against the previous month's on the same days. It is the
   month-to-date indicator of "Where does the money go?" (`docs/product.md`, 7.3).
4. Below it come net cash flow, savings rate, fixed costs ÷ income, debt service ÷ income, months
   of essential spending covered by liquid assets, and one row per credit card with its credit
   utilisation and available credit side by side.
5. Opening any of them shows its Figure detail.

**Pace chart.** Spending is the figure of `docs/domain.md`, 5.14, in the reporting currency, so
refunds lower it. The current month's line runs to today; the previous month's line covers its
whole month. The chart states both totals on today's day number and their difference: on
12 March, "1,240.00 so far · 1,410.00 by 12 Feb · 170.00 less". When the previous month has no
day with today's number, as on 30 March against February, the comparison uses February's total. In
the first month of the record there is no previous month, and the chart shows the current month
alone; that month, and the previous month's line in the second month, carry the incomplete-period
mark (6.1).

### 4.2 J2 — Know what to do

The dashboard section "What do I need to do?" lists what needs the owner. Each entry opens the
screen that resolves it.

| Entry | Shows | Opens | Order within the entry |
|---|---|---|---|
| Due dates in the next 30 days | Each credit card cycle's payment due date with its amount due in full in each currency, and each plan instalment's due date with its total; each with the days that remain. A due date appears from 30 days before it (`docs/quality-attributes.md`, FRS-5) | The card's Account screen, or the Plan screen | Soonest first |
| Items pending review | The number of review items | Review | — |
| Expected transactions that have not arrived | Each occurrence flagged missing (`docs/domain.md`, 5.12), with its expectation's name and expected amount | Expected transactions | Oldest first |
| Unreconciled accounts | Each unreconciled account with the end of its oldest period not yet reconciled | The account's Reconciliation screen | Oldest period first |

When none of them has an entry, the section says that nothing needs her and, while a notice about
capture is shown, shows that notice too (6.2).

### 4.3 J3 — Keep the record complete and correct

#### 4.3.1 Review

Review lists every transaction captured since the owner's last review (`docs/product.md`, 7.1
`review`), in two parts:

1. **Needs a decision.** Each review item, highest impact first (`REV-2`), with its reason
   (`docs/domain.md`, 5.11), its impact in the reporting currency, its amount in its own currency,
   its date, and what the captured record says.
2. **Recorded automatically.** Each transaction confirmed by a classification rule in *auto* mode
   or by a plan's recognition mode, with its account, amount, category, counterparty and
   description.

Transactions on an account marked for attention (`docs/domain.md`, 4.2) are highlighted in both
parts. A transaction recorded automatically is changed in one action from the list: its category,
counterparty or description is changed in place, without opening another screen, and the change
can become a rule in the same action.

Opening a review item shows the Review item screen: the captured record exactly as received, the
proposed transaction, and the decision its reason asks for.

| Reason | Decision asked |
|---|---|
| No rule matched · Rule in *confirm* mode | The category, counterparty and description, pre-filled by the rule when there is one |
| Unknown identifier | Which account or instrument the identifier belongs to, saved as an identifier mapping |
| Data the capture channel could not read or did not provide | The missing data, such as the date, or the amount applied to a card paid from an account in another currency, proposed from the cycle's amount due in full (`docs/domain.md`, 5.7) |
| Statement line missing from the record | The transaction the line shows, pre-filled from the line |
| Incoming payment that may settle money owed to her | The proposed allocation to open items, confirmed or changed (`docs/domain.md`, 5.5) |
| Captured records that may describe the same transaction | One transaction backed by both records, or two transactions |
| Payment that does not match its plan instalment | How the payment divides into principal, interest, insurance and fees, or that it is an extra principal payment (`docs/domain.md`, 5.6) |
| Captured record dated on its account's creation date, which the opening balance may already include | Whether the opening balance includes it: if it does, the transaction is discarded; if not, it is recorded (`docs/domain.md`, 4.2) |
| Proposed transaction in a reconciled period | A date in a period not yet reconciled, or discarding it (`REC-4`) |

Any item can instead be discarded, with a reason, which voids its proposed transaction.

**Making a rule.** The decision screen offers to save the decision as a rule in the same action.
Trueup proposes the conditions from the captured record, such as its counterparty text and its
account; she edits them, chooses the mode, *auto* or *confirm*, and confirms the decision and the
rule together.

#### 4.3.2 Entering a transaction by hand

1. From Dashboard, Review or Transactions, she starts a new transaction.
2. She enters its date, the account and amount, and where the money goes or comes from: one or
   more categories, another of her accounts, or shares owed by named people (example 7 in
   `docs/domain.md`, 5.2). Trueup derives the kind from what she enters (`TX-4`); she never
   chooses it.
3. Between two currencies, she enters the amount on each side, such as 18.40 debited and USD 20.00
   applied (example 6 in `docs/domain.md`, 5.2).
4. A purchase financed by a plan opens the plan's fields: number of instalments, interest rate,
   schedule, recognition mode and, when it is per instalment, the expense category
   (`docs/domain.md`, 5.6).
5. She confirms. What she typed is kept as its captured record (`docs/domain.md`, 5.9).

From an account's screen, she records two kinds of entry that start from a value rather than a
movement: a property's new value, recorded as a revaluation; and a long-term savings account's
balance as its institution reports it, for which Trueup shows the difference from the recorded
balance and she chooses the category of that income or spending (example 19 in
`docs/domain.md`, 5.2).

#### 4.3.3 Expected transactions

The Expected transactions screen lists each occurrence flagged missing, then the next occurrence
of each expectation. For a missing occurrence, she:

- **confirms it**, which records the transaction (`docs/domain.md`, 5.12). Its amount is
  pre-filled when the expected amount is exact; when it is a range, she enters it;
- **matches it** to a transaction that arrived with another amount or date, chosen from those on
  its accounts and in its category; or
- **marks it skipped**.

#### 4.3.4 Reconciling

Reconciliation is designed for a computer.

1. An account's screen lists its periods, each with its status: not ended, ready to reconcile,
   waiting for proposed transactions, with an open difference, or reconciled. Reconcile is offered
   only for the oldest period that has ended and is not reconciled (`REC-5`).
2. While the account has proposed transactions dated on or before the end of that period, the
   Reconciliation screen lists them, each opening its review item (`REC-2`).
3. She enters the institution's balance at the end of the period in each currency: the
   statement's closing balance or, for cash, the count. On a credit card she also states whether
   that balance includes the principal of instalments not yet billed (`docs/domain.md`, 5.8), and
   can replace the cycle's computed statement date with the statement's date (`docs/domain.md`,
   5.7).
4. Trueup shows, per currency, the recorded balance, the balance she entered, and the open
   difference (`docs/domain.md`, 5.14).
5. When every currency is equal, the period is reconciled.
6. When they differ and the institution issues a statement, she uploads its statement file.
   Trueup shows its lines matched with the record. A line with no match becomes a review item,
   which she resolves on this screen or in Review. A recorded transaction the statement shows is
   wrong she opens and changes, voids, or dates into the next period. The open difference updates
   after each change.
7. A difference that remains she closes with a transaction she records, in a category she chooses
   or as an unexplained difference (`REC-3`). The period is then reconciled.

She can upload the statement file even when the balances are equal. Opening a reconciled period
shows how it was reconciled (`docs/quality-attributes.md`, AUD-3).

### 4.4 J4 — Control my spending

The dashboard section "Am I within budget?" shows one horizontal progress bar per expense category
group that has a budget for the month (`docs/domain.md`, 5.13). The bar fills in proportion to
Spent in the group against its budget, and states both amounts and the percentage:
"Food · 400.00 of 450.00 · 88.9%".

- A group over budget is shown as over, in words and in the error colour (8.3), with the amount by
  which it is over: "Leisure · 330.00 of 300.00 · 110.0% · 30.00 over". The section begins by
  naming the groups over budget.
- When Spent in the group is zero or negative, the bar is empty and the amount keeps its sign.
- What proposed transactions would add is shown beside the group: "+ 22.00 pending review".
- There is no projection of where the month will end, and no budget alert or notification.
- An expense category group with no budget for the month appears with its Spent and no bar.

Opening a group shows the Budget group screen: each category in the group with its Spent and its
share of the group's spending. For example: Grocery 300.00 · 75.0%; Restaurants 120.00 · 30.0%;
Delivery −20.00 · −5.0%. When Spent in the group is zero or negative, each category shows only its
Spent (`docs/domain.md`, 5.13). Opening a category lists its transactions in the month.

### 4.5 J5 — Track what I owe and what I am owed

The dashboard section "What do I owe, and what am I owed?" shows the outstanding debt of each
loan, mortgage and instalment plan, the instalments coming due in the next 30 days, and the total
owed to her by each person. Each entry opens its screen:

- **Credit card** (Account screen): for the current cycle, its statement date, payment due date
  and amount due in full in each currency; the total balance; the credit limit, credit utilisation
  and available credit; and its plans with their instalments not yet billed.
- **Loan or mortgage** (Account screen): outstanding principal; the schedule and its earlier
  versions; the remaining total to pay, labelled as information and not as debt; the interest saved
  by each revision (`docs/domain.md`, 5.6).
- **Plan**: its schedule, which instalments are paid, billed or recognised, and what remains.
- **Amounts owed to her**: each person with each open item, its amount and its status. A
  repayment that arrives is allocated through Review (4.3.1). From an open item she writes off what
  will not be repaid, choosing its category (example 9 in `docs/domain.md`, 5.2).

### 4.6 J6 — Understand where money goes

The dashboard sections "Where does the money go?" and "How am I evolving?" show the current month.
The Analysis screen shows the same for any month of the record that she chooses.

- **Spending by category group**: one horizontal bar per group, which opens into one bar per
  category. Never a pie or donut chart: with many categories, a pie does not allow comparison.
- Each group and category states its comparison with its 3-month average: "+12.4% vs 3-month
  average". When the average is zero, the comparison gives the difference as an amount, without a
  percentage.
- **Top counterparties**: the counterparties with the most spending in the month, each with its
  amount.
- **How am I evolving?**: net worth by month, debt by type by month, and income against expenses
  by month, from the first month of the record.

She chooses which categories averages exclude on the Analysis screen, and marks a transaction as
extraordinary on its Transaction screen (`docs/product.md`, 7.3).

### 4.7 J7 — Maintain my financial structure

Configuration is designed for a computer.

**First use.**

1. She chooses the reporting currency, which Trueup states cannot change once the first account
   is created (`MON-4`; `docs/domain.md`, 3), and the workspace time zone.
2. She gives Trueup the staging sheet's location and its read-only credential
   (`docs/reference/staging-sheet.md`; `docs/quality-attributes.md`, SEC-6).
3. She runs the one-time import, or creates her accounts one by one.

**One-time import.** She starts it and confirms it on the same day (`docs/product.md`, 7.1
`financial-structure`). Trueup shows the proposal in sections: accounts, each with the kind and
currencies she confirms and its opening balance; identifiers; categories, with the attributes the
import asks of her; classification rules, in their order; and one field per expense category
group for its monthly budget. What the import proposes and asks is described in
`docs/reference/spreadsheet-import.md`. Nothing in the proposal is active until she confirms the
whole of it.

**Changing the structure.** Settings holds one screen per kind of configuration:

| Screen | What she does there |
|---|---|
| Accounts and institutions | Create an account: kind, institution, currencies, Liquid, and the opening balance in each currency — on a credit card, also its credit limit, statement day, payment due date rule and each plan already running; on money lent or a shared expense, one opening balance per debtor (`docs/domain.md`, 4.2). Add instruments and identifiers, and mark an old card inactive. Mark an account for attention. Close an account (6.4) |
| Categories | Groups and categories, each expense category's three attributes; archive a category already used |
| Counterparties | Counterparties and their aliases |
| Rules | Classification rules in the order they are evaluated (`CLS-1`), which she changes; each rule's conditions, outcome and mode, with its usage: times matched and date last matched |
| Expectations | Each expectation's schedule, expected amount, accounts, category and tolerance in days |
| Budgets | Each expense category group's standing amount from a month on, and its override for one month (`docs/domain.md`, 5.13) |
| Currencies | The workspace's currencies, and the rate she enters for each (`docs/domain.md`, 3) |
| Workspace | The reporting currency, shown as fixed once the first account exists; the time zone; the staging sheet's location and credential |
| Security | The devices where she is signed in; ending a session on another device (`docs/quality-attributes.md`, SEC-3); signing out |

### 4.8 J8 — Own my data

- On the Data screen, in Settings, she asks for an export of everything, and Trueup produces it
  for her to download (`docs/quality-attributes.md`, PRT-1).
- How she starts restoring a backup is decided in `docs/architecture.md`
  (`docs/backlog.md`, B-74). The first time she opens Trueup after a restore, a notice states the
  moment of the backup the workspace returned to, and that her work after that moment is lost
  (`docs/quality-attributes.md`, PRT-3).

## 5. Screens

"Phone and computer" means every action on the screen can be completed in the phone layout.
"Computer" means the screen opens on a phone but may need a computer (2, principle 1).

| Screen | Area | Shows, and what she does there | Designed for |
|---|---|---|---|
| Sign-in | — | Signing in; a second factor on a new device | Phone and computer |
| Dashboard | Dashboard | Notices; the questions of `docs/product.md`, 7.3, as sections: "Am I on track this month?" first (4.1), "What do I need to do?" second (4.2). The order of the other sections is a construction choice | Phone and computer |
| Figure detail | Any | Every input of a figure (`docs/quality-attributes.md`, AUD-1), its marks and the reference rates it used (7.2); for a figure made of other figures, each of them | Phone and computer |
| Budget group | Dashboard | 4.4 | Phone and computer |
| Analysis | Dashboard | 4.6, for a month she chooses | Phone and computer |
| Expected transactions | Dashboard | 4.3.3 | Phone and computer |
| Review | Review | 4.3.1 | Phone and computer |
| Review item | Review | The captured record as received, the proposed transaction, the decision and the rule (4.3.1) | Phone and computer |
| Transactions | Transactions | Every transaction, newest first, with search and filters; proposed and void ones marked as such | Phone and computer |
| Transaction | Transactions | Its legs, kind, marks and converted amounts; what confirmed it, its captured records, its review items and their resolutions, its changes, and its void reason (`docs/quality-attributes.md`, AUD-2). Changing, splitting, voiding and marking extraordinary, unless it is in a reconciled period | Phone and computer |
| New transaction | Transactions | 4.3.2 | Phone and computer |
| Accounts | Accounts | Accounts by group of `docs/product.md`, 7.2, each with its balance in each currency and its marks; closed accounts apart | Phone and computer |
| Account | Accounts | Balance in each currency, transactions, periods and their status; by kind, as 4.3.2 and 4.5 state | Phone and computer |
| Plan | Accounts | 4.5 | Phone and computer |
| Amounts owed to her | Accounts | 4.5 | Phone and computer |
| Reconciliation | Accounts | 4.3.4 | Computer |
| Reconciled period | Accounts | `docs/quality-attributes.md`, AUD-3 | Phone and computer |
| Capture reads | Settings | Each read of the staging sheet: its time, the two counts compared, and whether it was processed (`docs/quality-attributes.md`, AUD-4). Also opened from the notices and from "Can I trust these numbers?" | Phone and computer |
| Settings screens | Settings | 4.7 | Computer |
| One-time import | Settings | 4.7 | Computer |
| Data | Settings | 4.8 | Computer |

## 6. States

### 6.1 Marks

| Mark | Applies when | Shown as |
|---|---|---|
| Incomplete period | The period has not ended; or it began before the creation date of the workspace's first account; or the figure averages a month that is incomplete (`docs/domain.md`, 5.14) | The extent of the period: "March, to date"; "March, from 9 Mar" |
| Unreconciled | The figure counts an account that is unreconciled (`docs/product.md`, 6, principle 4) | "2 accounts unreconciled", opening the list. On an account: "Unreconciled since 31 Mar" |
| Pending | Proposed transactions would change the figure (`TX-3`, `BUD-2`) | Their amount, signed, beside the figure: "400.00 + 22.00 pending review" |
| Converted | The figure combines amounts converted into the reporting currency | "Converted", opening the reference rates it used (7.2) |
| Missing rate | An amount the figure needs has no reference rate (`MON-3`) | The figure without those amounts, and beside it the amounts not converted, in their own currency, with an action to enter a rate: "1,240.00 + USD 35.00 not converted" |

An account or period that is never reconciled (`docs/domain.md`, 5.8) never shows the unreconciled
mark (`IND-2`).

### 6.2 Empty

- With no account yet, the dashboard shows the first-use steps (4.7) in place of its sections.
- An empty list says what would appear in it: "Nothing captured since your last review".
- While a notice about capture is shown (6.3), an empty Review and an empty "What do I need to
  do?" show it too: an empty list never suggests all is well while transactions may have stopped
  arriving.
- A figure with nothing to count is shown as zero, with its marks. It is distinct from a figure
  that cannot be shown (6.4) and from a ratio with no value (7.3).

### 6.3 Notices

| Notice | When | States |
|---|---|---|
| Capture stopped | Trueup has not processed a complete read of the staging sheet for more than five days (`docs/quality-attributes.md`, FRS-2) | Its cause — Trueup could not process the staging sheet — and the date of the last processed transaction |
| Extractor stopped | The email extractor's latest run is more than five days old (`docs/quality-attributes.md`, FRS-3) | Its cause — the email extractor has not run for more than five days — and the date of the last processed transaction |
| Restored | The first time she opens Trueup after a restore (4.8) | The moment of the backup, and that her work after it is lost |

A capture notice is shown at the top of the dashboard every time she opens Trueup while its cause
holds, and cannot be dismissed; when both causes hold, both are shown. It opens Capture reads. It
puts no mark on accounts or figures. For example: "Transactions may have stopped arriving: Trueup
could not process the staging sheet. Last processed transaction: 2 Mar 2026."

### 6.4 Refusals and errors

- **Refused change.** A change that would break an invariant is refused with an informational
  message that gives the reason (`docs/quality-attributes.md`, COR-2). Changing a confirmed
  transaction so that it would fall in a reconciled period — a new date, or a leg moved to another
  account — and confirming a transaction dated in one, including an occurrence of an expectation,
  are refused with the message that the period is already reconciled, so nothing can be recorded
  or changed in it (`docs/domain.md`, 5.8, `REC-1`): "March 2026 is already reconciled for
  Checking, so nothing can be recorded or changed in it." Closing an account whose balance is not
  zero in each currency is refused, showing those balances (`ACC-5`).
- **Interrupted action.** When an action is interrupted, Trueup shows whether it was applied: its
  result is visible, or the screen says it was not applied and still holds what she entered
  (`docs/quality-attributes.md`, COR-3).
- **Figure that cannot be shown.** A figure Trueup cannot compute or load is shown as unavailable,
  with a way to try again: never as zero, and never as an earlier value presented as current.
- **Statement file that cannot be read.** Trueup says so and extracts nothing from it; she can
  upload it again.

## 7. Numbers, dates and currencies

### 7.1 Amounts

An amount is shown with exactly its currency's number of decimal places (`MON-1`), with its digits
grouped in thousands: 1,240.00. In lists and tables, amounts are aligned on their decimal point,
in the typeface for amounts in lists (8.4). A negative amount has a minus sign before it, never
parentheses.

| What | How its sign is shown | Example |
|---|---|---|
| An account's balance | As `docs/domain.md`, 4.2, defines it: positive for assets, negative for liabilities, so net worth is their sum | Card X −1,100.00 |
| What is owed: an amount due, outstanding debt, what a card owes, an amount owed to her | Positive, with a label that names who owes it | Amount due in full 800.00 |
| A transaction, on one account's screen | The change in that account's balance (`docs/domain.md`, 5.1) | −40.00 for a grocery purchase; +3,000.00 for a salary |
| A transaction, in the whole record | The change in the balance of the account it is on; a transfer names both accounts, from → to, with its amount unsigned | Checking → Wallet 100.00 |
| Spending, income, Spent | Positive; negative only when refunds exceed purchases (`docs/domain.md`, 5.13) | Delivery −20.00 |
| A pending amount; a difference from a comparison | With + or −, or with a word that states the direction | + 22.00 pending review; 170.00 less |

### 7.2 Currencies and conversions

- A transaction is always shown in its own currency. A transaction between two currencies shows
  both amounts: "18.40 → USD 20.00".
- An amount in a currency other than the reporting currency always names its currency.
- Beside a transaction's amount in another currency, a screen may show its approximate amount in
  the reporting currency, marked ≈ and never in place of it: "USD 20.00 ≈ 18.00".
- A converted amount states each reference rate it used, with the date of the transaction the rate
  comes from or the mark that the owner entered it (`MON-3`). The rates are stated on the Figure
  detail and Transaction screens, and on every other screen through the converted mark (6.1):
  "1 USD = 0.92000 EUR · rate of 14 Mar 2026"; "1 USD = 0.92000 EUR · entered rate".
- A reference rate is shown in units of the reporting currency per unit of the other currency
  (`docs/domain.md`, 3), rounded to five decimal places, halves away from zero: 18.40 ÷ 21.00 =
  0.876190… is shown as 0.87619. The amounts of the transaction it comes from are not shown next
  to it. Calculations use the rate exact (`docs/domain.md`, 3, "Rounding";
  `docs/quality-attributes.md`, AUD-1).
- A figure about one currency, such as a card's amount due in full in it, is shown in that
  currency (`docs/domain.md`, 3).

### 7.3 Ratios

- Percentages are shown with one decimal place, halves away from zero, as converted amounts are
  rounded (`docs/domain.md`, 3): savings rate, fixed costs ÷ income, debt service ÷ income, credit
  utilisation, a category's share, the part of a budget spent, and a comparison with an average.
  With income 3,000.00 and spending 2,025.00, the savings rate is 975.00 ÷ 3,000.00, shown as 32.5%.
- Months of essential spending covered is shown with one decimal place: liquid balances of
  9,000.00 against an average of 2,100.00 give 4.2857…, shown as 4.3 months.
- A ratio whose denominator is zero, such as the savings rate before any income in the month, has
  no value: it is shown as "—" with the reason ("no income yet this month"), never as 0% or as
  infinite.

### 7.4 Dates and times

- Dates are calendar dates in the workspace time zone (`docs/domain.md`, 5.1). A transaction has a
  date and no time.
- The month is written as a word, abbreviated where space is short, never as a number, so that day
  and month cannot be confused: 3 Mar 2026. The year may be left out within the current year.
- A calendar month is named by month and year, "March 2026"; a card cycle by its first and last
  days, "26 Feb – 25 Mar 2026".
- A due date shows the days that remain: "14 Apr · in 5 days".
- A moment — the time of a read, of a change in a transaction's history, of a backup — is shown as
  a date and a time to the minute, in the workspace time zone.

## 8. Visual language

### 8.1 Character

Calm, light and precise: a soft grey canvas, white cards, near-black for primary actions, and one
raspberry accent. Controls are pill-shaped and cards generously rounded. The tone is concise and
transparent.

### 8.2 Logo

The logo is a **symbol** followed by the **wordmark** "Trueup", a capital T followed by lowercase
letters in a rounded geometric sans-serif, drawn as outlines rather than set in a typeface.

The symbol is two pieces that fit together into a square, separated by an even gap of 4 units, on
a grid of 32 × 32 units, with x growing to the right and y downwards:

- the first piece: (2, 2) → (18, 2) → (18, 14) → (10, 14) → (10, 30) → (2, 30);
- the second piece: (22, 2) → (30, 2) → (30, 30) → (14, 30) → (14, 18) → (22, 18).

| Variant | Symbol | Wordmark | On |
|---|---|---|---|
| Main | Raspberry #D82E5A | Black #111111 | Light backgrounds |
| Dark | Raspberry #D82E5A | White #FFFFFF | Dark backgrounds |
| Monochrome | Black #111111 | Black #111111 | Light backgrounds, one colour |
| Monochrome inverse | White #FFFFFF | White #FFFFFF | Dark backgrounds, one colour |

- The symbol is also used alone, in the same variants. The logo's background is transparent.
- **Clear space:** 8 grid units around the symbol's visible outline: 2/7 of its visible height.
- **Minimum size:** the symbol 16 × 16 px, where its gap is 2 px and its narrowest part 4 px; the
  horizontal logo 144 px wide.
- **App icon:** a square filled entirely with raspberry #D82E5A, with the white symbol inside its
  central safe circle, at 1024, 512 and 192 px. The browser icon is the symbol at 16, 32 and 64 px.
- The geometry of the pieces, the wordmark, the space between symbol and wordmark, and their
  proportions are never altered.

### 8.3 Colour

Trueup has a light and a dark theme, with the same roles.

| Role | Light | Dark |
|---|---|---|
| Primary: primary buttons, the active area in the computer layout, a screen's highest-emphasis card | #111827 | #F9FAFB |
| On primary | #FFFFFF | #111827 |
| Accent | #D82E5A | #FB7185 |
| Accent, dark | #B52E51 | — |
| Accent, subtle background | #FCECEF | — |
| Card | #FFFFFF | #18181B |
| Canvas: the page behind the cards | #F4F5F7 | #27272A |
| Raised container | #E5E7EB | #3F3F46 |
| Headings | #111827 | #F4F4F5 |
| Body text | #4B5563 | #D4D4D8 |
| Muted text | #9CA3AF | — |
| Outline | #D1D5DB | #52525B |
| Hairline border | #E5E7EB | #3F3F46 |
| Error | #DC2626 | #F87171 |
| Success | #10B981 | — |
| Warning | #F59E0B | — |

Values marked — in the dark theme are chosen in construction.

What colours mean:

- The accent marks the brand, the current selection, focus, and the highlight of transactions on
  accounts marked for attention (subtle background). It never means good or bad.
- Error marks a group over budget and an error. Warning marks a notice (6.3). Success marks a
  reconciled period. Marks (6.1) and refusals (6.4) are neutral.
- Colour is never the only carrier of meaning: every state also has a word or a symbol.

### 8.4 Typography

Plus Jakarta Sans for all text and for headline figures; JetBrains Mono for amounts in lists and
tables and for moments, so that digits align.

| Role | Computer, size / line height (px) | Phone layout | Weight |
|---|---|---|---|
| Display | 40 / 48 | 28 / 36 | 700 |
| Headline large | 32 / 38 | 24 / 30 | 700 |
| Headline medium | 22 / 28 | Same | 600 |
| Headline small | 18 / 24 | Same | 600 |
| Body large · medium · small | 16 / 24 · 14 / 20 · 12 / 16 | Same | 400 |
| Label large · medium · small | 14 / 20 · 12 / 16 · 11 / 14 | Same | 600 |
| Hero figure: the leading figure of a section | 36 / 42 | 28 / 34 | 700 |
| Stat figure: an indicator | 24 / 30 | Same | 700 |

Large sizes are slightly tightened, from −0.75 px of letter spacing at 40 px to −0.1 px at 18 px;
labels are slightly opened, 0.1 to 0.2 px.

### 8.5 Layout, space and shape

- **Computer layout:** content at most 1280 px wide, in a grid of several columns of cards.
  **Phone layout:** one column, in the same order of importance.
- **Space:** an 8 px grid, with steps of 4, 8, 16, 24 and 32 px, 40 px between sections, and a
  24 px page margin.
- **Shape:** buttons, filters, navigation items, marks and progress bars are pills, fully rounded.
  Cards have corners of 16 to 24 px. Radii come from the steps 6, 10, 14, 18 and 24 px.
- **Depth:** white cards float on the canvas with a hairline border (#E5E7EB at 70% opacity) and a
  very soft shadow (offset 0 × 8 px, blur 24 px, spread −4 px, black at 4% opacity).
- **Navigation, computer layout:** the areas sit in a pill-shaped container in the canvas colour,
  with a hairline border; the active area is a primary-coloured pill with on-primary text; the
  others are in body text, darkening when pointed at.
- **Navigation, phone layout:** the bottom bar is white at 95% opacity over a blurred background,
  with a hairline top border; the active area's icon and label are in the accent colour and bold;
  the others are muted. Icons are outlined.

### 8.6 Motion

Motion responds to an action and never decorates.

| Motion | Duration | Easing |
|---|---|---|
| A state change, such as a selection | 120 ms | Ease-out |
| A surface entering, such as a panel or a dialog | 220 ms | cubic-bezier(0.16, 1, 0.3, 1) |
| A surface leaving | 140 ms | cubic-bezier(0.7, 0, 0.84, 0) |

### 8.7 Charts

- Comparisons between groups or categories are horizontal bars; amounts over time are lines or
  vertical bars. Spending by category group is never a pie or donut chart (4.6).
- Every chart states its key values as numbers, not only as shapes.
- Every chart fits the phone layout without horizontal scrolling.
- In the pace chart (4.1), the previous month's line differs from the current month's in style as
  well as colour.

### 8.8 Voice

- Concise, plain and factual. Messages state what happened and what she can do.
- Buttons name their action with a verb: "Confirm", "Make it a rule", "Reconcile", "Export".
- No reassurance that the data does not support: "All good" is never shown while a mark or a notice
  applies.

## 9. Changing this document

Changes are explicit: a line in the history below with the date, what changed and why. A flow or
screen that appears during construction is added here before any specification depends on it. A
specification never contradicts this document; when it needs to, this document changes first.

## History

| Date | Version | Change |
|---|---|---|
