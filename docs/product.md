# Trueup — Product

> **Version:** 1.1 · 2026-10-06 · approved by the owner
>
> **Answers:** why Trueup exists, for whom, what success means, which capabilities it has and in
> what priority.
>
> **Does not answer:** detailed behaviour (`openspec/specs/`) · concepts and invariants
> (`docs/domain.md`) · measurable system qualities (`docs/quality-attributes.md`) · interaction
> and screens (`docs/experience.md`) · technology (`docs/architecture.md`, `docs/adr/`) ·
> delivery order (`docs/roadmap.md`).

---

## 1. Purpose

Trueup is a personal finance application with a single user: its owner. It records her
transactions — read automatically from the bank notifications that an existing email extractor
collects, and entered by hand when no notification exists — and helps her control her spending
against monthly budgets. At month-end each account is checked against its institution; when a
balance does not match, the institution's statement is compared line by line with the record to
find what is missing or wrong.

The result is one reconciled, trustworthy record of her money that tells her what needs her
attention.

The name comes from *true-up*, the accounting practice of adjusting recorded figures to what
actually is.

## 2. Context and problem

**How transactions reach Trueup.** Most of the owner's banks send an email for each operation.
An email extractor that already exists, and is independent of Trueup, reads those emails every
day and writes each one, verbatim and without interpretation, as a row in a spreadsheet: the
**staging sheet**. Trueup reads the staging sheet. What no email reports is entered by hand or
found during month-end reconciliation.

Keeping a true picture of her money is hard for reasons that will not go away:

- **Institutions report unevenly.** Some notify every operation, some notify only certain kinds
  of operation, some send nothing at all. The mix differs per institution and per product, and
  changes over time. For an account with no notifications, transactions only become known when
  she enters them or when its statement is reconciled.
- **Notifications identify counterparties poorly.** A payment processor appears instead of the
  actual merchant; one merchant appears under many codes; a person-to-person payment names only
  a person.
- **No bank connection is available.** There is no open-banking access she can use, so data
  arrives through notifications, statements and manual entry — never through an API.
- **Money moves between her own accounts constantly:** salary to spending accounts, card
  payments, cash withdrawals, a small wallet topped up for everyday payments. Counted as
  spending, these transfers inflate expenses; lost, they break balances.
- **Money is shared and owed.** She pays for groups and is reimbursed later, often partially;
  she buys in instalments; she carries loans and a mortgage. Each of these makes the money she
  spent differ from the money that left her accounts.
- **Some accounts hold money that is not entirely hers,** and money leaving such an account may
  or may not be hers. Ownership cannot be inferred from the account alone.

Her finances are currently kept in a spreadsheet, which shows the limits of that approach:
balances start from zero instead of real opening balances, so net worth is fictional; nothing is
reconciled against the institutions; decisions already made are asked again; and there is no
way to see which accounts are fully captured and which are not.

## 3. User

One owner. Nobody else uses Trueup; other people appear in it only as counterparties.

- Financially literate. Comfortable defining rules and categories herself, and expects to.
- Reviews in short sessions on her phone during the week; analyses on a computer occasionally.
- Wants upkeep to be a short, predictable weekly habit, not a project.
- Her financial structure changes: accounts open and close, cards are replaced, institutions
  change how they report, other currencies appear when she travels or buys abroad.

## 4. Jobs to be done

| Job | When… | I want… | So that… |
|---|---|---|---|
| **J1** Know where I stand | I open Trueup | to know within a minute whether this month is on track | I can correct course early instead of discovering problems at month-end |
| **J2** Know what to do | something needs me — a payment coming due, an item to review, an account out of balance, an expected transaction that has not happened | Trueup to tell me, most important first | I never pay interest or fees through oversight, and nothing stays unresolved |
| **J3** Keep the record complete and correct | money moves | it recorded once, in the right account, currency and category, with the least effort from me, and every decision I make remembered | the record is trustworthy and I never answer the same question twice |
| **J4** Control my spending | I spend during the month | to see, per category, how much of my budget is left and whether I am on pace | I stay within the limits I set for myself |
| **J5** Track what I owe and what I am owed | I take a loan, buy in instalments, or pay for others | to see what remains to be paid or collected, and from whom | no instalment surprises me and no reimbursement is forgotten |
| **J6** Understand where money goes | I look back over weeks or months | income and spending by category and counterparty, with trends and changes | I can decide what to cut, protect or plan for |
| **J7** Maintain my financial structure | my financial life changes | to adapt Trueup myself — accounts, instruments, categories, rules, budgets, currencies | it keeps fitting my life without anyone changing code |
| **J8** Own my data | at any time | to export everything and to recover from a backup | I am never locked in and never lose my history |

## 5. Outcomes

How we know Trueup works. Each one is observable by the owner or computable by Trueup.

| # | Outcome | Target |
|---|---|---|
| **O1** | Weekly upkeep — reviewing, confirming, reconciling | ≤ 15 minutes in a normal week |
| **O2** | A decision is never needed twice: a resolved item never returns for review, and an answer turned into a rule applies to every later match | 0 re-asked items |
| **O3** | Every active account is reconciled each month against its institution | 100% of active accounts; difference zero, or explained by an explicit adjustment |
| **O4** | No interest or late fees caused by a missed due date or an unknown amount due | 0 per year |
| **O5** | No budget overrun goes unnoticed: a category that exceeds its budget, or is on pace to, is flagged before month-end | Every overrun flagged before month-end |
| **O6** | No reimbursement is forgotten: every amount owed to her stays visible until it is settled or written off | 0 amounts owed that are not visible |

**Leading indicator** — watched, not targeted: the share of captured transactions resolved
without review should rise over time. If it does not, answers are not being turned into rules.

System-level targets — data freshness, availability, privacy, security, cost — are defined in
`docs/quality-attributes.md`.

## 6. Product principles

Every capability and every design choice must be consistent with these. When two conflict, the
lower number wins.

1. **No default fills a gap.** When the data or the owner's configuration does not determine an
   account, category, currency, counterparty or ownership, the item goes to review. Trueup never
   fills the gap with a default value.
2. **Nothing changes a figure without the owner's decision.** A transaction affects balances and
   totals only once confirmed — by her directly, or by a rule she set to apply automatically. A
   rule set to *confirm* only pre-fills the item; it still waits for her.
3. **Capture is literal, judgment is explicit, figures are derived.** What arrives is kept
   exactly as received. Interpreting it is a separate, visible and reversible step. Every
   balance, total and indicator is computed, never entered. A capture error can produce a wrong
   item for review, never a silently wrong figure.
4. **Honest numbers.** Every figure shows when it rests on an incomplete period, an unreconciled
   account, or an account whose capture is incomplete. This applies to budgets too: a category
   whose spending may not all be captured yet says so. Trueup never reassures with partial data.
5. **Answer once.** Any answer can become a rule or a mapping in the same action that gives it.
6. **Configuration, not code.** Her institutions, accounts, instruments and their identifiers,
   currencies, categories, counterparties, rules and budgets are data she manages in Trueup. A change in her financial life must not require a code change, unless it introduces a
   genuinely new kind of instrument or behaviour.
7. **Effort goes where the money is.** Review is ordered by financial impact. Frequent,
   low-value activity is handled by rules, not item by item.

## 7. Capabilities

Each capability's detailed behaviour has exactly one home: `openspec/specs/<capability>/`. The
names below are those folder names.

The MVP is everything needed for Trueup to replace the owner's spreadsheet as the system of
record, plus the reconciliation the spreadsheet never had. Recording starts at a cut-off date,
from opening balances taken from the institutions; transactions recorded before that date stay
in the spreadsheet and are not imported.

### 7.1 MVP

| Capability | Covers | Jobs |
|---|---|---|
| `access` | Signing in. Only the owner can reach any data | all |
| `financial-structure` | Institutions. Accounts of every kind in 7.2, each with its currency or currencies, an opening balance at a cut-off date, and how its transactions are captured. Instruments, such as cards, and every identifier under which an account or instrument appears. Categories and their classification. Counterparties. The owner's settings. A one-time import of the configuration she already keeps: categories, rules and identifiers | J7 |
| `transactions` | Recording transactions in the currency they were executed in. A transaction affects one account (income, expense) or moves money between two accounts (transfer, card payment, withdrawal, top-up) as a single transaction, including between currencies. Splitting one transaction across several categories or shares. Ownership decided per transaction when a third-party account is involved. Derived balances and net worth | J3 |
| `capture` | Bringing transactions in through capture channels: the staging sheet and manual entry. No duplicates. Proof that a read lost nothing. Every captured record kept as received, alongside the transaction derived from it | J3 |
| `classification` | Rules the owner manages that assign account, category, counterparty and description. Each rule either applies automatically or pre-fills the item and waits for confirmation | J3 |
| `review` | Every transaction captured since her last review, listed in two parts. First, what waits for her decision, grouped by reason and ordered by financial impact: no matching rule or a rule that requires confirmation, an unknown identifier, data the capture could not read, an ownership question, a transaction found missing during reconciliation, an incoming payment that may settle an amount owed to her. Then, what was recorded automatically, with transactions on accounts she marks for attention highlighted and changeable in one action. Resolving an item and turning the answer into a rule in one action | J2, J3 |
| `recurring` | Expected transactions, such as salary and fixed payments: expecting them, flagging when one has not arrived, recording it on confirmation | J2, J3 |
| `reconciliation` | At month-end, per account: comparing the recorded balance with the institution's. If it matches, the account is marked reconciled. If not, the owner uploads the statement and Trueup compares it line by line with the record, listing what is missing from the record, what is in the record but not in the statement, and amounts that differ. What is missing goes to review. Any remaining difference is closed by an explicit, visible adjustment | J1, J2, J3 |
| `card-cycles` | Statement closing and due dates; statement balance per currency; minimum and full payment, including instalments due in the cycle; advance warning before each due date | J2, J5 |
| `debt` | Loans and mortgage: outstanding principal, rate, instalment schedule and its revisions; each payment divided into principal, interest, insurance and fees; extra principal payments. Card instalment plans: each instalment is added to the card's amount due until the plan is paid off. Every plan that finances a purchase states when that spending is recognised: in full when the plan starts, or as each instalment is paid | J2, J5 |
| `receivables` | Money owed to the owner. A shared expense is split into her own share, which is her spending, and the shares owed by named people. Money she lends. Settling an amount owed, fully or partially, with the payment that repays it — a repayment is never income. Writing off what will not be repaid | J5, J6 |
| `budgets` | A monthly amount per category; spending against it; pace through the month; warning when a category is over, or on pace to be over, its budget | J4 |
| `dashboard` | The questions in 7.3 | J1, J2, J4, J5, J6 |
| `data-portability` | Export of everything in open formats; backups; restore | J8 |

### 7.2 Kinds of account

The kinds of account in scope. What each kind is and how it behaves is defined in
`docs/domain.md`.

| Group | Kinds |
|---|---|
| Available money | Cash · bank account · digital wallet |
| Credit | Credit card |
| Debt | Loan · mortgage |
| Receivable | Money lent · shared expense |
| Long-term savings | Deposit · severance fund · pension fund |
| Prepaid | Transport card · membership · gift card |
| Other assets | Property, valued by hand |
| Special | Third-party account · ignored identifier |

Instruments in scope: debit card · credit card.

### 7.3 Dashboard in the MVP

| Question | Indicators |
|---|---|
| Am I on track this month? | Net cash flow (transfers excluded) · savings rate · fixed costs ÷ income · debt service ÷ income · months of essential spending covered by liquid assets · credit utilisation |
| Am I within budget? | Spending against budget per category · pace through the month · categories over, or on pace to be over, budget |
| What do I need to do? | Due dates in the next 30 days · items pending review · unreconciled accounts · expected transactions that have not arrived |
| What do I owe, and what am I owed? | Outstanding debt by loan, mortgage and instalment plan · instalments coming due · amounts owed to her, by person |
| Where does the money go? | Spending by category group, opening into categories · comparison with the 3-month average · top counterparties · month-to-date spending against the previous month, day by day |
| How am I evolving? | Net worth by month · debt by type · income against expenses by month |
| What does money cost me? | Interest and fees by month and year · credit utilisation per card · subscriptions in the reporting currency |
| Can I trust these numbers? | Capture coverage per account · date of last reconciliation · open differences |

Averages can exclude categories she selects and transactions she marks as extraordinary.

### 7.4 Later

| Capability | Covers |
|---|---|
| `ai-suggestions` | Category and rule suggestions from an AI service, only for counterparties her rules do not know. Suggestions only: principle 2 applies |
| `payables` | Money she owes to people, as opposed to institutions |
| `subscriptions` | Detecting recurring charges and their price changes |
| `custody` | Balances of money she holds on behalf of others |
| `targets` | Reference targets for dashboard indicators, such as a minimum savings rate |
| `goals` | Savings goals and reserve funds |
| `tags` | Free labels that cut across categories |
| `digest` | A periodic summary delivered to her |

### 7.5 Out of scope

| Not in Trueup | Why |
|---|---|
| Direct connection to financial institutions | No usable open-banking access |
| Reading email, or changing the email extractor | The extractor stays independent and literal; Trueup consumes the staging sheet read-only |
| Several people sharing one set of finances | Built for one owner |
| Serving several independent owners, as a multi-tenant service | Not in the MVP. The design must not preclude it |
| Investment tracking — holdings, prices, returns | Not a current need. An investment account's balance can still be recorded as an account |
| Tax filing | Not a current need |

## 8. Changing this document

Changes are explicit: a line in the history below with the date, what changed and why. Moving a
capability between MVP and Later is a change to this document, not to a spec. A capability that
appears during construction is added here before any change is proposed for it.

## History

| Date | Version | Change |
|---|---|---|
| 2026-09-30 | 1.0 | First approved version |
| 2026-10-06 | 1.1 | Defining the domain refined the scope. Property added as a kind of account, valued by hand, so mortgage debt has its asset. Each financing plan states when its spending is recognised. The spreadsheet's configuration is imported, its transactions are not: recording starts at a cut-off date. Indicator targets moved to Later. Review lists every new transaction, not only those that need a decision |
