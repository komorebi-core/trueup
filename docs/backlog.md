# Backlog

Decisions and open questions waiting for the document that will hold them. Each item names its
target document. When that document incorporates an item, the item is removed from this file in
the same change.

An item marked **Open** needs the owner's answer. It is never resolved by assumption.

## For `domain.md`

- **B-31** — Section 5.8, outcome *Different*, states an outcome for each discrepancy it lists: a
  statement line missing from the record becomes a review item; a recorded transaction that the
  statement shows was recorded wrongly — a different amount, a duplicate, an operation that never
  took place — is corrected or voided by the owner; only the difference that remains is closed by
  an explicit transaction (`REC-3`). `docs/product.md`, 7.1 `reconciliation`, states this scope.
- **B-32** — Section 4.2: a credit card's opening balance is its total debt, including
  instalments not yet billed. When the institution reports only billed amounts, the owner adds
  the instalments still to be billed.
- **B-33** — Section 5.12: an expectation also covers an occurrence that arrives with another
  amount or date, matched when the owner confirms it, and an occurrence the owner marks as
  skipped, which is no longer flagged as missing. `docs/product.md`, 7.1 `recurring`, states
  this scope.
- **B-34** — Section 4.3: delete the paragraph after the table of kinds, which refers to an
  *ignored identifier* entry in `docs/product.md` 7.2 that no longer exists. Whether the Ignore
  target of an identifier mapping (4.4) stays is B-46.
- **B-35** — The scope facts that `docs/product.md` states are cited from it, not restated as
  this document's own: which accounts' balances are tracked and what a third-party account is
  (7.2 and 2), that an account whose balance is tracked starts from an opening balance on its
  creation date (7), which delivered records are discarded (7.1 `capture`), and which accounts
  and periods reconciliation covers (7.1 `reconciliation`).
- **B-36** — Sections 5.8 and 5.9 cite `docs/product.md`, 7.1 `capture` and `reconciliation`,
  for a transaction's date — that of the first notification that arrives, or the date the owner
  changes it to at reconciliation — and for the period it belongs to. Section 5.8 adds that a
  statement line is matched against the whole record, including transactions in earlier,
  reconciled periods; a line that matches one is never missing from the record.
- **B-37** — `docs/product.md`, 7.1 `reconciliation`, states that a reconciled period is never
  reopened, that its confirmed transactions cannot be changed, and how a transaction that arrives
  dated inside one is resolved. Section 5.4 and `REC-1`, which require reopening a reconciliation
  to correct a transaction inside it, change accordingly: a correction is a new transaction in a
  period not yet reconciled. Section 5.11 gains the review reason for a proposed transaction
  dated inside a reconciled period; discarding it means it never becomes a transaction.
- **B-39** — A change in the balance of a deposit, severance fund or pension fund that none of the
  owner's movements explains is recorded as income or spending in a category; unlike a property's
  change of value (5.1), it is not a revaluation.
- **B-40** — A budget belongs to a category group, not to a category. Section 5.13, `BUD-1`,
  `BUD-2` and everything that implies per-category budgets change accordingly. There is no pace
  figure. Every budget is expressed in the reporting currency. Define how a group's spent amount
  and each category's share of it are computed, and whether that is an invariant, including the
  shares when a category's refunds exceed its purchases in the month, and when a group's spending
  is zero or negative. Only expense category groups have budgets.
- **B-41** — The reporting currency is the default currency in which figures are expressed, and
  budgets are expressed in it. Every amount that a figure combines across currencies, budgets
  included, is converted into it (3).
- **B-42** — Section 4.2, Status, cites `docs/product.md`, 7.1 `financial-structure`: an account
  is closed only when its balance is zero, after a transfer or withdrawal empties it, and no
  transaction is recorded on it after it is closed.
- **B-46** — Section 4.4: decide whether the Ignore target of an identifier mapping is kept. It
  has no current use: the one case it was meant for is a third-party account (4.5).
- **B-47** — Section 5.8 cites `docs/product.md`, 7.1 `reconciliation`, for the accounts that are
  never reconciled, which now include deposits, severance funds and pension funds.

## For `quality-attributes.md`

- **B-8** — Data freshness: a transaction written to the staging sheet is visible in Trueup
  within 24 hours.
- **B-9** — Tenant readiness: adding a second workspace requires no schema redesign and no
  change to existing data.
- **B-10** — Demo isolation: if a demo instance exists, it never has access to real data, not
  even read access.
- **B-11** — **Open.** The monthly cost ceiling for running Trueup.

## For `experience.md`

- **B-12** — **Open.** The language of the user interface.
- **B-13** — Spending by category group is shown as bars that open into categories, never as a
  pie or donut chart: with many categories, a pie does not allow comparison.
- **B-14** — Month-to-date cumulative spending against the previous month, day by day, is the
  main view of the month's pace.
- **B-15** — A converted amount states each reference rate it used, with its date or the mark
  that the owner entered it. A transaction is always shown in its own currency, optionally with
  an approximate converted amount beside it.
- **B-16** — Review lists every transaction captured since the last review: first the items that
  need a decision, then those recorded automatically, with transactions on accounts marked for
  attention highlighted and changeable in one action.
- **B-17** — Pending amounts are shown next to the figures they would change, apart from the
  confirmed amount (for example, "+ 22.00 pending review").
- **B-45** — The dashboard shows each group budget's progress as a chart, so the owner sees at a
  glance how much of it she has spent (for example, 90%) or that she is over it. There is no pace
  projection and no budget alert or notification. Opening a group's chart shows its detail: the
  categories in the group, the amount spent in each, and each one's percentage of the group's
  spending. Complements B-13 and B-14.

## For `architecture.md` and `adr/`

- **B-18** — A demo, if built, is a separate deployment with its own database, seeded with
  synthetic data and reset periodically. It never shares a database, credentials or files with
  production. **Open:** its purpose — private staging, public showcase, or both — which decides
  when it is built.
- **B-19** — Tenant-ready precautions, from the first version: every record belongs to a
  workspace; uniqueness rules are scoped to the workspace; settings, capture sources,
  credentials and stored files are per workspace, never global; the database supports row-level
  security, even if it is not enabled while there is one workspace; an isolation test becomes
  mandatory the day a second workspace exists.
- **B-20** — How money is stored — an exact decimal type, or integer minor units — is decided in
  an ADR.
- **B-21** — Every external dependency — reading the staging sheet, extracting statement lines,
  hosting, database, authentication — is tested end to end, writes included, before any
  specification depends on it. The evidence goes in the ADR.
- **B-22** — Statement extraction depends on each institution's statement format. Each format is
  validated by the owner against a real statement on her own machine; real statements are never
  committed.

## For `roadmap.md`

- **B-23** — AI suggestions stay in Later. Whether to build them is decided after measuring how
  many captured transactions still need review once rules are in use.
- **B-24** — Owner prerequisites before first use: an opening balance for every account whose
  balance is tracked, as of the date the account is created (`docs/domain.md`, 4.2); a property
  whose opening balance is the mortgage's outstanding principal on the property's creation date.
- **B-25** — The spreadsheet remains the system of record until Trueup replaces it. There is no
  period without a system.
- **B-26** — Trueup is deployed from the first increment, so that technical decisions are tested
  in real use from the start.

## For `reference/staging-sheet.md`

- **B-29** — The section "Keys" states that a row dated before the creation date of an account it
  maps to, or before the creation date of the first account when it maps to none, is never
  registered: Trueup recognises it by its external key and skips it. It cites `docs/domain.md`,
  5.9, and `CAP-5` instead of restating the rule, including how a row with no operation date
  (column J empty) is dated. Recognising these rows never writes to the sheet, which stays
  read-only (`docs/product.md`, 7.5).

## For `reference/spreadsheet-import.md`

- **B-43** — The import creates the accounts of the `Accounts` sheet, each with its kind and
  currencies confirmed by the owner. The Category column of `Accounts` only proposes each
  account's kind; it is not an expense category of `Categories`. Start Value is imported as the
  account's opening balance. The owner fills it, before importing, with the account's updated
  balance: its balance at the moment she confirms the import, which becomes each imported
  account's creation date — the value she would otherwise enter by hand. The import is started
  and confirmed on the same day. A money-lent or shared-expense account's Start Value is split by
  debtor; a credit card's includes instalments not yet billed (B-32); a third-party account has
  none, and her own funds in it, if any, open an account of her own funds. The section
  `Accounts` and the paragraph that says accounts are pending confirmation change accordingly.
- **B-44** — The Monthly Budget column of `Categories` is per category, and budgets belong to
  expense category groups (B-40), so it is not imported as a budget. While reviewing the import,
  the owner enters each expense category group's monthly budget (`docs/product.md`, 7.1
  `financial-structure`). The `Categories` table changes accordingly.

## For `AGENTS.md` and `openspec/config.yaml` (end of the documentation phase)

- **B-27** — Method for construction: OpenSpec changes; requirements with WHEN/THEN scenarios,
  each turned into a test; three checkpoints per increment — scope agreed, dependencies
  verified, acceptance tests passing; if a problem is fixed by adding data (a rule, a mapping), it
  is not a specification change; test data is synthetic.
- **B-28** — Every `design.md` cites the section of `docs/domain.md` or the ADR it relies on. If a
  change needs a decision no project document holds, it stops and the project document is
  amended first. Decisions in a `design.md` that other changes will depend on are promoted to an
  ADR when the change is archived.
- **B-30** — Each capability's detailed behaviour has exactly one home,
  `openspec/specs/<capability>/`, where `<capability>` is the capability's name in
  `docs/product.md`, 7.1 or 7.4.
