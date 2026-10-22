# Trueup — Domain

> **Status:** Draft · pending owner approval
>
> **Answers:** what each concept in Trueup is, how the concepts relate, which rules always hold
> (invariants), and what is code versus what is data.
>
> **Does not answer:** which capabilities exist and in what priority (`docs/product.md`) ·
> detailed behaviour (`openspec/specs/`) · physical schema and technology (`docs/architecture.md`,
> `docs/adr/`) · screens (`docs/experience.md`).

---

## 1. How to read this document

- **Logical, not physical.** Concepts, attributes and relations; no tables, columns or types.
- **Terms in bold** are defined in section 2 and are used verbatim in specs and code.
- **Invariants** have an ID (`TX-1`) and are listed in section 6. Specs and tests cite them by ID.
- **Kinds are code, instances are data.** This document defines kinds of things. Every concrete
  institution, account, category, rule, amount or currency is data the owner manages. Section 7
  draws the line.
- **Examples are invented.** Names, amounts and currencies in examples are illustrative; no
  example value is a default.

## 2. Glossary

| Term | Meaning |
|---|---|
| **Workspace** | The container of everything one owner records. Every record belongs to exactly one |
| **Owner** | The single person a workspace belongs to |
| **Currency** | A unit of money, with a code, a symbol and a number of decimal places |
| **Reporting currency** | The workspace currency into which figures in several currencies are consolidated |
| **Reference rate** | The exchange rate between two currencies on a date, used only to consolidate figures |
| **Institution** | A bank, card issuer, lender or other entity that holds accounts |
| **Account** | Something with a balance in one or more currencies: a bank account, a credit card, a loan, cash, a receivable, a property |
| **Instrument** | A means of operating on an account, such as a card. It has no balance of its own |
| **Identifier** | A string under which an account or instrument appears in captured data: an account number, a masked card number, an interbank code |
| **Counterparty** | Whom money goes to or comes from: a merchant, a person or an institution |
| **Category** | A classification of income or spending, inside a **group** |
| **Transaction** | One economic event, made of two or more **legs** whose values sum to zero |
| **Leg** | One side of a transaction: an amount in one currency on one account, category or adjustment |
| **Adjustment** | A leg target that is neither an account nor a category: opening balance, revaluation, or unexplained difference |
| **Capture channel** | A way transactions reach Trueup: the staging sheet or manual entry |
| **Captured record** | What a capture channel delivered, kept exactly as received |
| **Rule** | Owner-managed configuration that resolves part of a captured record: an **identifier mapping** or a **classification rule** |
| **Review item** | Something waiting for the owner's decision, with its reason |
| **Plan** | A loan schedule or an instalment plan that spreads a debt over instalments |
| **Statement** | The institution's account of a period: balances, dates and, when uploaded, lines |
| **Reconciliation** | The comparison of an account's recorded balance with the institution's on a date |
| **Expectation** | A transaction the owner expects on a schedule, such as salary |
| **Budget** | A monthly spending limit for one expense category |
| **Cut-off date** | The date from which an account's balance is recorded in Trueup |

## 3. Money

**Currencies are data.** The workspace holds the list of currencies it uses. Adding one is a
configuration change.

**Amounts are exact.** An amount is a decimal with exactly the precision of its currency. It is
never a floating-point number, and it is never stored converted into another currency.

**Two kinds of exchange rate:**

| Rate | Where it comes from | What it is for |
|---|---|---|
| **Transaction rate** | Implied by the legs of a transaction that mixes currencies (5.1) | Recording what actually happened, such as paying a foreign-currency debt in the local currency |
| **Reference rate** | A rate per currency pair and date, from a source decided in `docs/architecture.md` | Consolidating balances and totals into the reporting currency |

A figure that needs a reference rate that does not exist for its date is shown as incomplete. A
rate is never assumed. The spread between a transaction rate and the reference rate is not
recorded as a cost.

## 4. Financial structure

### 4.1 Institution

An institution has a name and holds accounts. It carries no behaviour: nothing in Trueup
depends on which institution an account belongs to.

### 4.2 Account

| Attribute | Meaning |
|---|---|
| Kind | One of the kinds in 4.3. Determines behaviour |
| Class | Asset or liability. Derived from the kind |
| Currencies | One or more. Each currency has its own balance |
| Liquid | Whether its balance counts as money available at short notice. Set by the owner, required |
| Capture method | How its transactions reach Trueup: notified, statement only, or manual. Used to show capture coverage |
| Cut-off date | The date of its opening balance |
| Status | Active or closed. A closed account keeps its history |
| Ownership | Own, or third-party (4.5) |
| Attention | Whether its transactions are highlighted in review even when recorded automatically |

A **balance** is always derived (`ACC-2`). Assets have positive balances; liabilities have
negative balances, so net worth is the sum of all balances.

### 4.3 Kinds of account

| Kind | Class | Behaviour that depends on the kind |
|---|---|---|
| Cash | Asset | Usually reconciled by counting |
| Bank account | Asset | Can have instruments (debit cards) |
| Digital wallet | Asset | — |
| Credit card | Liability | Has a credit limit and a cycle rule (5.7). Can hold balances in several currencies. Can carry instalment plans. Can have instruments |
| Loan | Liability | Its balance is outstanding principal. Has a schedule (5.6) |
| Mortgage | Liability | Same as loan |
| Money lent | Asset | Every leg names the person who owes it (5.5) |
| Shared expense | Asset | Same as money lent |
| Deposit · severance fund · pension fund | Asset | — |
| Transport card · membership · gift card | Asset | Prepaid balance. Topping it up is a transfer |
| Property | Asset | Valued by hand: changes of value are revaluations (5.1), never income |
| Deferred spending | Asset | Internal. Created for a plan whose spending is recognised per instalment (5.6) |
| Third-party account | — | Its balance is not tracked (4.5) |

### 4.4 Instruments and identifiers

An **instrument** belongs to exactly one account. A transaction made with a debit card is
recorded on the bank account the card draws on. When a card is replaced, the new card is a new
instrument of the same account, and the old one is kept, inactive, with its history.

An **identifier** is matched after normalisation: spaces and separators removed, letters
compared without case. An **identifier mapping** sends a normalised identifier to one of:

| Target | Effect |
|---|---|
| An account | The leg is recorded on that account |
| An instrument | The leg is recorded on the instrument's account |
| A third-party account | Ownership applies (4.5) |
| Ignore | Captured records with this identifier never become transactions |

An identifier with no mapping produces a review item. Text that an institution writes about an
operation, such as "transfer between your accounts", never decides which accounts are involved
or whose they are; only identifier mappings do (`ID-1`).

### 4.5 Third-party accounts and ownership

A **third-party account** is an account the owner can operate but whose money is wholly or
partly someone else's. Trueup does not track its balance. Activity inside it that involves only
the third party is not recorded.

A third-party account can have an **own-funds account**: an asset account holding the part of
the money in it that belongs to the owner.

When money moves between one of the owner's accounts and a third-party account, the question
is whose money it is. The account's ownership settings answer it:

| Setting | Meaning |
|---|---|
| Default owner | Third party, owner, or none. With *none*, every such transaction is a review item |
| Category when the third party owns it | For example, a family-support category. Money sent to the third party is spending in it |
| Attention | Highlight these transactions in review, so the default can be changed in one action |

| The money is… | Recorded as |
|---|---|
| The third party's (money sent) | Spending in the account's category |
| The owner's (sent by mistake, or savings kept there) | A transfer to the own-funds account |
| The owner's, coming back | A transfer from the own-funds account |
| The owner's, used by the institution for a fee or to cover one of her debts | A leg on the own-funds account: a fee expense, or a payment of that debt |

### 4.6 Counterparties

A counterparty is a merchant, a person or an institution. It has **aliases**: the texts under
which it appears in captured data.

A payment processor is not a counterparty. When captured text names a processor, the actual
counterparty is unknown; a classification rule in *confirm* mode makes this explicit.

A person can be a payee, a payer and a debtor (5.5). It is the same counterparty in all roles.

### 4.7 Categories

Three levels:

| Level | What it is |
|---|---|
| **Type** | Income or expense. A property of the group |
| **Group** | A set of categories of one type |
| **Category** | What a leg is classified as |

Every **expense category** has three attributes, whose values the owner sets:

| Attribute | Values | Used by |
|---|---|---|
| Necessity | Essential · discretionary | Months of essential spending covered by liquid assets |
| Cost behaviour | Fixed · variable | Fixed costs ÷ income |
| Financial cost | Yes · no | Interest and fees; debt service |

Indicators read these attributes, never category names or identities (`IND-1`). A category that
has been used is archived, never deleted.

Moving money between the owner's own accounts — transfers, card payments, withdrawals, top-ups,
savings, principal repayments, lending — is never a category (`TX-5`).

## 5. Transactions

### 5.1 Transactions and legs

A **transaction** has:

| Attribute | Meaning |
|---|---|
| Date | When the operation happened, as a calendar date in the workspace time zone |
| Transaction currency | The currency in which leg values are expressed |
| Description | Text shown to the owner |
| Status | Proposed, confirmed or void (5.3) |
| Extraordinary | Whether averages may exclude it |
| Captured records | The records it was derived from, if any |
| Plan instalment | The plan instalment it pays, if any |
| History | Every change made after confirmation |

A **leg** has:

| Attribute | Meaning |
|---|---|
| Target | Exactly one of: an account in one of its currencies · a category · an adjustment |
| Amount | In the leg's own currency |
| Value | The same amount expressed in the transaction currency. Equal to the amount when the currencies match |
| Counterparty | Required on legs of money-lent and shared-expense accounts |
| Note | Optional |

**The values of a transaction's legs always sum to zero** (`TX-1`).

**Sign convention.** An account leg is the change in that account's balance, from the owner's
point of view: positive when she has more or owes less. A category or adjustment leg carries the
opposite sign of the account legs it balances. Spending in a category is the sum of its legs;
income in a category is the negated sum. How amounts are displayed is decided in
`docs/experience.md`.

**Adjustments** are leg targets for events that are neither spending nor income:

| Adjustment | Used for |
|---|---|
| Opening balance | An account's balance at its cut-off date |
| Revaluation | A change in the value of a property |
| Unexplained difference | A reconciliation difference the owner chooses not to classify |

Adjustments never count as income or spending.

### 5.2 Examples

Invented amounts. Reporting currency EUR.

| # | Event | Legs |
|---|---|---|
| 1 | Grocery purchase with card X | Card X −40.00 · Grocery +40.00 |
| 2 | Salary received | Checking +3,000.00 · Salary −3,000.00 |
| 3 | Top-up of a wallet | Checking −100.00 · Wallet +100.00 |
| 4 | Card payment | Checking −500.00 · Card X +500.00 |
| 5 | Purchase in USD on card X (transaction currency USD) | Card X (USD) −20.00 · Subscriptions +20.00 |
| 6 | Paying USD 100.00 of card X's USD balance with EUR 92.00 (transaction currency EUR) | Checking −92.00 · Card X (USD) +100.00, value +92.00 |
| 7 | Dinner for four; her share is 30.00 | Card X −120.00 · Restaurants +30.00 · Shared expense (A) +30.00 · Shared expense (B) +30.00 · Shared expense (C) +30.00 |
| 8 | A repays her | Wallet +30.00 · Shared expense (A) −30.00 |
| 9 | C will not repay | Shared expense (C) −30.00 · Gifts +30.00 |
| 10 | Mortgage instalment: principal 400, interest 550, insurance 50 | Checking −1,000.00 · Mortgage +400.00 · Mortgage interest +550.00 · Mortgage insurance +50.00 |
| 11 | Extra principal payment | Checking −5,000.00 · Mortgage +5,000.00 |
| 12 | Purchase in 6 instalments, recognised at start | Card X −1,200.00 · Electronics +1,200.00 |
| 13a | Tuition financed by the school, recognised per instalment: plan starts | Loan −3,000.00 · Deferred spending +3,000.00 |
| 13b | Each instalment of 13a | Checking −300.00 · Loan +300.00 · Deferred spending −300.00 · Education +300.00 |
| 14 | Refund of part of a grocery purchase | Card X +25.00 · Grocery −25.00 |
| 15 | Money sent by mistake to a third-party account, marked as hers | Checking −1,000.00 · Own funds in T +1,000.00 |
| 16 | The institution charges a fee on her funds in T | Own funds in T −10.00 · Bank fees +10.00 |
| 17 | Money sent to T for the third party | Checking −200.00 · Family support +200.00 |
| 18 | Opening balances at cut-off | Checking +2,500.00 · Opening balance −2,500.00 ; Card X −800.00 · Opening balance +800.00 |
| 19 | Cash counted 35.00 short; she classifies it | Cash −35.00 · Cash spending +35.00 |

### 5.3 Kind of a transaction

The kind is derived from the legs, never chosen (`TX-4`):

| Legs | Kind | Sub-kind, derived from the kinds of the accounts involved |
|---|---|---|
| Only accounts | **Transfer** | Card payment (4) · top-up (3) · withdrawal · saving · debt payment (11) · lending |
| Accounts and expense categories | **Expense** | — |
| Accounts and income categories | **Income** | — |
| A mix | One kind per leg group | 7: expense and lending · 10: debt payment and expense |
| Only adjustments and accounts | **Adjustment** | Opening (18) · revaluation · reconciliation |

### 5.4 Lifecycle

```
captured record ──► proposed transaction ──► confirmed transaction
                           │                       │
                           └──► review item        └──► corrected (history kept) or void
```

| Status | Affects figures | Meaning |
|---|---|---|
| Proposed | No | Built from a captured record; waits for a decision or for a rule |
| Confirmed | Yes | Confirmed by the owner, or by a rule she set to apply automatically |
| Void | No | Discarded with a reason, such as a duplicate; kept for audit |

Proposed transactions are shown as pending next to every figure they would change (`TX-3`).

A confirmed transaction can be corrected. Every correction is kept with its time and previous
values (`TX-6`). A correction inside a reconciled period requires reopening that reconciliation
(`REC-1`).

**Spending is recognised on the transaction date, in full**, unless the purchase is financed by
a plan that recognises it per instalment (`TX-7`).

### 5.5 Money owed to the owner

A leg on a money-lent or shared-expense account names its debtor. Each positive leg is an
**open item**. A repayment is allocated to one or more open items, fully or partially; an item's
status — open, partly settled, settled, written off — is derived from its allocations. A write-off
moves what remains to a category the owner chooses (example 9). A repayment is never income
(`RCV-1`).

An incoming payment from a person with open items produces a review item proposing the
allocation.

### 5.6 Debt and plans

**Loans and mortgages.** The balance is outstanding principal. Interest not yet accrued is never
part of it (`DEBT-1`).

A **schedule** lists the instalments: due date, principal, interest, insurance, fees and total.
When a captured payment matches the instalment due, it is split according to the schedule
(example 10); when it does not match, it becomes a review item. An extra principal payment is all
principal (example 11); the lender's revised schedule is recorded as a new version, and earlier
versions are kept (`DEBT-2`).

Derived from the schedule: the remaining total to pay, shown as information and not as debt; and
the interest saved by a revision, as the difference in total interest between two versions.

**Instalment plans** spread a purchase over instalments on a credit card or with another lender.
A plan has: the account, the originating transaction, the amount, the number of instalments, the
rate (which may be zero), the schedule, and the **recognition mode**:

| Recognition mode | Spending is recognised | Example |
|---|---|---|
| At start | In full, on the purchase date | 12 |
| Per instalment | As each instalment is paid, through a deferred-spending account | 13a, 13b |

On a credit card, each instalment billed in a cycle is part of that cycle's amount due (5.7).

### 5.7 Credit cards

A credit card has:

- a **credit limit**, in one currency, shared by all its currencies;
- a **cycle rule**: the closing day and how the due date follows from it.

Each **cycle** has a closing date and a due date, computed from the rule and replaced by the
statement's when one is uploaded. Per currency, a cycle has:

| Figure | Source |
|---|---|
| Total balance | The account balance, including instalments not yet billed |
| Amount due in full | The total balance at the closing date, minus instalments not yet billed. Computed; replaced by the statement's when uploaded |
| Minimum payment | The statement, or entered by the owner. Unknown otherwise |

Credit utilisation = total balance across all currencies, converted at the reference rate ÷
credit limit.

### 5.8 Statements and reconciliation

A **statement** belongs to an account and a period. It has a closing date and, per currency, a
closing balance; on a credit card, also a due date and a minimum payment. When the owner uploads
the statement file, Trueup extracts its **lines** (date, description, amount, currency) and keeps
the file.

A **reconciliation** compares, for one account and currency on one date, the institution's
balance with the recorded balance of confirmed transactions.

| Outcome | What happens |
|---|---|
| Equal | The account is reconciled to that date |
| Different | The statement's lines are matched with recorded transactions by amount, date and description. Lines with no match become review items. Recorded transactions with no line, and amounts that differ, are listed |
| Different after review | The remaining difference is closed by an explicit transaction: a category she chooses, or an unexplained difference |

On a credit card, the total balance is compared with the institution's total debt. If the
institution reports only billed amounts, instalments not yet billed are excluded from the
comparison.

An account with proposed transactions dated on or before the reconciliation date cannot be
reconciled (`REC-2`). Once reconciled, confirmed transactions on that account dated on or before
the date are locked (`REC-1`).

### 5.9 Capture

A **captured record** holds: its channel, its external key, every field exactly as delivered, the
time it was read, and the source's own parse status when there is one.

- The external key is the operation number reported by the source, normalised (leading zeros
  removed). When there is none, the channel provides another unique key.
- A read of a channel is processed only after proving it is complete against an independent
  count provided by the source (`CAP-3`).
- One captured record backs at most one transaction. One transaction may be backed by several
  records — for example, when both institutions notify the same transfer (`CAP-4`).

Manual entry is a capture channel too: what the owner typed is kept as its captured record.

### 5.10 Rules

An **identifier mapping** resolves accounts (4.4).

A **classification rule** resolves category, counterparty and description:

| Part | Content |
|---|---|
| Conditions | Any combination of: counterparty text (contains, starts with, equals; case-insensitive) · amount (a list of values or a range) · kind of operation as reported by the capture channel · account or instrument · currency · direction |
| Outcome | Category · counterparty · description, which may contain placeholders such as the month name |
| Mode | *Auto*: the transaction is confirmed. *Confirm*: the outcome only pre-fills a review item |
| Position | Rules are evaluated in the owner's order; the first match wins (`CLS-1`) |
| Usage | Times matched and date last matched |

A captured record that no rule matches becomes a review item. No category is ever assigned by
default (`CLS-2`).

### 5.11 Review items

A review item has: its **reason**, the captured record or proposed transaction it concerns, its
**impact** in the reporting currency, and, once resolved, the **resolution**: what the owner
decided and whether it became a rule.

| Reason |
|---|
| No rule matched |
| Rule in *confirm* mode |
| Unknown identifier |
| Data the capture channel could not read |
| Ownership of money in a third-party account |
| Statement line missing from the record |
| Incoming payment that may settle money owed to her |
| Captured records that may describe the same transaction |
| Payment that does not match its plan instalment |

Review items are ordered by impact (`REV-2`). A resolved item never reopens for the same
captured record (`REV-1`).

### 5.12 Expectations

An **expectation** describes a transaction the owner expects: a schedule, an expected amount
(exact or a range), the accounts, the category, and a tolerance in days. A confirmed transaction
that fits is matched to it. When the tolerance passes with no match, the expectation is flagged
as missing. Confirming it records the transaction — the way to record income that no channel
notifies.

### 5.13 Budgets

A **budget** is a monthly amount, in the reporting currency, for one expense category
(`BUD-1`). It can have a standing amount and per-month overrides.

| Figure | Definition |
|---|---|
| Spent | The category's confirmed legs in the month, converted at the reference rate of each transaction date. Refunds reduce it |
| Pending | Proposed legs in the category, shown apart (`BUD-2`) |
| Pace | Spent ÷ (budget × fraction of the month elapsed) |

### 5.14 Derived figures

| Figure | Definition |
|---|---|
| Balance of an account on a date | Sum of its confirmed legs up to that date, per currency |
| Net worth | Sum of all balances, converted at the reference rate of the date |
| Income · spending | Income-category and expense-category legs of the period. Adjustments and transfers excluded |
| Net cash flow | Income − spending |
| Savings rate | (Income − spending) ÷ income |
| Fixed costs ÷ income | Spending in categories with cost behaviour *fixed* ÷ income |
| Debt service ÷ income | Payments to loans and mortgages in the period, principal and costs together ÷ income |
| Months of essential spending covered | Balances of liquid accounts ÷ average monthly spending in *essential* categories over the last three complete months |
| Interest and fees | Spending in categories with financial cost *yes* |
| Capture coverage | Per account, from its capture method and the state of its reconciliations |

Every figure carries its completeness: incomplete period, unreconciled accounts, accounts with
incomplete capture, pending items, missing reference rates (`IND-2`).

## 6. Invariants

| ID | Invariant |
|---|---|
| **WS-1** | Every record belongs to exactly one workspace. Uniqueness is scoped to the workspace. No setting is global |
| **MON-1** | Amounts are exact to their currency's precision. No floating-point arithmetic on money |
| **MON-2** | A leg's amount is stored in its own currency. Conversions are derived, never stored in its place |
| **MON-3** | No conversion without a recorded rate. A figure missing a reference rate is shown as incomplete |
| **ACC-1** | An account's class is derived from its kind |
| **ACC-2** | A balance is the sum of confirmed legs, opening balance included. It is never entered |
| **ACC-3** | An instrument has no balance; its legs are recorded on its account |
| **ID-1** | Accounts and ownership are resolved only through identifier mappings and ownership settings, never from text written by an institution |
| **ID-2** | A normalised identifier maps to at most one target in a workspace |
| **TX-1** | The values of a transaction's legs sum to zero |
| **TX-2** | Each leg targets exactly one account-currency, category or adjustment |
| **TX-3** | Only confirmed transactions change figures. Proposed ones are shown as pending |
| **TX-4** | A transaction's kind is derived from its legs |
| **TX-5** | Money moving between the owner's own accounts is never income or spending |
| **TX-6** | A confirmed transaction is never overwritten or deleted: corrections keep history; removal is a void with a reason |
| **TX-7** | Spending is recognised on the transaction date in full, unless its plan recognises it per instalment |
| **OWN-1** | Whose money moves to or from a third-party account is decided by the owner or by that account's default owner; otherwise it is a review item |
| **RCV-1** | Repaying money owed to the owner is never income |
| **RCV-2** | Allocations to an open item never exceed its amount |
| **DEBT-1** | A loan's or mortgage's balance is outstanding principal; future interest is never part of it |
| **DEBT-2** | A revised schedule is a new version; earlier versions are kept |
| **CARD-1** | A credit card has one credit limit, shared by its currencies |
| **CAP-1** | A captured record is never modified |
| **CAP-2** | A captured record is unique per channel and external key; reading it again creates nothing |
| **CAP-3** | A read is processed only after it is proven complete |
| **CAP-4** | A captured record backs at most one transaction; a transaction may be backed by several records |
| **CLS-1** | Classification rules are evaluated in the owner's order; the first match wins |
| **CLS-2** | No category, account, counterparty or ownership is assigned by default; what no rule or setting resolves becomes a review item |
| **REV-1** | A resolved review item never reopens for the same captured record |
| **REV-2** | Review items are ordered by impact in the reporting currency |
| **REC-1** | Confirmed transactions on a reconciled account, dated on or before the reconciliation date, are locked; changing them requires reopening the reconciliation |
| **REC-2** | An account cannot be reconciled while it has proposed transactions dated on or before the reconciliation date |
| **REC-3** | A reconciliation difference is closed only by an explicit transaction |
| **BUD-1** | Budgets exist only for expense categories, per month, in the reporting currency |
| **BUD-2** | Budget figures count confirmed legs; proposed legs are shown apart |
| **IND-1** | Indicators read category attributes and account settings, never category or account names or identities |
| **IND-2** | Every figure carries its completeness flags |

## 7. Code and data

| Code defines — changing it needs a change proposal | Data defines — the owner changes it in Trueup |
|---|---|
| The kinds of account and their behaviour (4.3) | Institutions, accounts, instruments, identifiers, identifier mappings |
| The leg model, adjustments and sign convention | Currencies, reporting currency, time zone, reference rates |
| Transaction statuses, kinds and their derivation | Categories, groups and their attribute values |
| The three category attributes and their values | Counterparties and their aliases |
| Review reasons | Classification rules, their order and mode |
| Recognition modes | Plans, schedules and their versions |
| Reconciliation outcomes and locking | Expectations, budgets |
| Indicator definitions (5.14) | Liquidity, capture method, ownership settings and attention per account |
| Completeness flags | Exclusions for averages; extraordinary transactions |

A new kind of account with behaviour of its own, or a new indicator, is a code change. A new
account, category, rule, currency or institution is not.

## 8. Changing this document

Changes are explicit: a line in the history below with the date, what changed and why. A change
here that affects an invariant names the invariant. A specification never contradicts this
document; when it needs to, this document changes first.

## History

| Date | Version | Change |
|---|---|---|
