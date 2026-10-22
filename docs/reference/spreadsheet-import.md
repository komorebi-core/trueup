# Reference — Spreadsheet configuration import

> **Status:** Reference. Describes the structure of the owner's spreadsheet and how its
> configuration becomes Trueup data.

Before Trueup, the owner kept her finances in a spreadsheet. Trueup imports its
**configuration** once — categories, classification rules, identifiers and the accounts they
refer to. It never imports the spreadsheet's transactions; recording in Trueup starts at the
start date (`docs/product.md`, 7). Whether accounts are part of the import is pending the owner's
confirmation (`docs/backlog.md`, B-7).

The import is a **proposal**: the owner reviews and confirms it before anything in it becomes
active. Where the spreadsheet does not hold a value Trueup needs, the owner supplies it; the
import never fills it by assumption.

Identify columns by their header text, not by position. The spreadsheet is edited by hand and
its layout can shift.

## Sheets read

| Sheet | Header row | Data from | Columns |
|---|---|---|---|
| `Categories` | 7 | 8 | Type · Group · Position · Monthly Budget · Nature · Key |
| `Merchant Rules` | 6 | 7 | Pattern · Match · Position · Details · Counterparty label · Action · Hits |
| `Account Map` | 6 | 7 | Identifier · Bank · Instrument · Maps to Account · Action · Notes |
| `Accounts` | Two blocks, each with its own header row: assets, then liabilities | — | Assets: Category · name · Currency · Start Value · Current Value · Liquid? — Liabilities: Category · name · Currency · Start Value · Current Value · Credit Limit |

Every other sheet — transactions, budget, dashboard, calculations, trends — is ignored. Columns
holding formulas (`Key`, `Current Value`) are ignored.

## `Categories`

The spreadsheet classifies with three columns — Type, Group, Position — plus a single Nature
column. Trueup has groups, categories and three attributes per expense category
(`docs/domain.md`, 4.7).

| Spreadsheet | Trueup |
|---|---|
| Type `Income` or `Expense` | The type of the group |
| Group | Group |
| Position | Category, with the same name |
| Type `Savings`, `Debt` or `Transfer` | Not a category. These rows describe money moving between the owner's own accounts, which Trueup records as transfers |
| An income position for refunds | Not a category. A refund reduces spending in the category of the original purchase |
| Income positions for reimbursements or loan repayments | Not categories. They settle money owed to the owner |
| An expense position for money lent | Not a category. Money lent is an account |
| An expense position for a full loan or mortgage instalment | Not a category as it stands. Principal is a transfer; interest, insurance and fees become categories with financial cost *yes*. The owner names them |
| Monthly Budget, when greater than zero, on an expense category | The category's standing monthly budget |
| Nature | See below |

**Nature.** The spreadsheet has a single value — `Essential`, `Fixed` or `Discretionary` — where
Trueup has two independent attributes: necessity (essential or discretionary) and cost behaviour
(fixed or variable). The import proposes what Nature implies and asks for the rest:

| Nature | Proposed | Asked of the owner |
|---|---|---|
| `Essential` | Necessity: essential | Cost behaviour |
| `Fixed` | Cost behaviour: fixed | Necessity |
| `Discretionary` | Necessity: discretionary | Cost behaviour |
| Empty, on an expense category | — | Both |

The financial-cost attribute is asked for every expense category.

Category names are imported as written. Renaming them afterwards is a data change.

## `Merchant Rules`

Each row becomes a classification rule. Order is preserved: the spreadsheet's rules apply top to
bottom, first match wins, as Trueup's do (`docs/domain.md`, `CLS-1`).

| Spreadsheet | Trueup |
|---|---|
| Pattern with Match `contains` | Condition: counterparty text contains Pattern, case-insensitive |
| Pattern with Match `starts` | Condition: counterparty text starts with Pattern, case-insensitive |
| Match `amount if unknown`; Pattern is a `\|`-separated list of amounts | Condition: amount is one of the listed values, **and** the operation is a person-to-person payment. The rule is placed after every rule that names a person, so a named person always wins over an amount |
| Position | Category of the outcome |
| Details | Description of the outcome. `{month}` becomes the month-name placeholder |
| Counterparty label | Counterparty of the outcome; Pattern becomes one of its aliases |
| Action `auto` | Mode *auto* |
| Action `always confirm` | Mode *confirm* |
| Hits | Usage count |

A rule whose Position is not imported as a category (see `Categories`) is shown to the owner
before import with the reason; it is not imported until she decides its outcome.

## `Account Map`

Each row becomes an identifier mapping. Identifiers are normalised the same way captured data is:
spaces and separators removed.

| Action | Trueup |
|---|---|
| `register`, Instrument `Account` | Mapping to the account named in Maps to Account |
| `register`, Instrument `Debit card` | An instrument of the account named in Maps to Account, with this identifier |
| `register`, Instrument `Credit card` | Mapping to the credit-card account named in Maps to Account |
| `third-party` | Mapping to a third-party account; its ownership settings are asked of the owner (`docs/domain.md`, 4.5) |
| `ignore` | Ignore mapping |

## `Accounts`

| Spreadsheet | Trueup |
|---|---|
| Name and Currency | An account in that currency. Its kind is proposed from the Category column and confirmed by the owner |
| Two rows that are the same card in two currencies, named alike with a currency suffix | One credit-card account holding both currencies. The pairing is confirmed by the owner |
| Liquid? | Liquid attribute, confirmed by the owner |
| Credit Limit, when greater than zero | Credit limit |
| Start Value | Not imported. Opening balances are entered at the start date from real statements |

Currencies are written in the spreadsheet as symbols. Each symbol is mapped to a workspace
currency by the owner at import time.
