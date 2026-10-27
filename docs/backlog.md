# Backlog

Decisions and open questions waiting for the document that will hold them. Each item names its
target document. When that document incorporates an item, the item is removed from this file in
the same change.

An item marked **Open** needs the owner's answer. It is never resolved by assumption.

## For `domain.md`

- **B-52** — **Open.** In 5.7, the owner enters by hand, as cycle data and with no statement, the
  payment due date and, per currency, the amount due in full and the minimum payment of each card
  cycle that no statement serves. When she enters them for the cycle in which the card is created
  is undecided: that cycle's amount due in full and minimum payment do not exist yet when the card
  is created. Options: (a) once that cycle closes, before its payment due date; until then the
  computed amount due in full applies and the minimum payment is unknown; or (b) when she creates
  the card, as estimates.

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
- **B-50** — Available credit per card (`docs/domain.md`, 5.14) is a dashboard indicator
  (`docs/product.md`, 7.3); this document decides where the dashboard shows it.
- **B-51** — When the owner tries to change a confirmed transaction so that it would fall in a
  reconciled period, or to confirm a transaction dated in one, Trueup refuses and shows an informational
  message: the period is already reconciled, so nothing can be recorded or changed in it
  (`docs/domain.md`, 5.8, `REC-1`).

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
- **B-24** — Owner prerequisites before first use: an opening balance for every account she
  creates (`docs/domain.md`, 4.2); a property
  whose opening balance is the mortgage's outstanding principal on the property's creation date;
  each plan already running — loan, mortgage, or instalment plan on a card or with another
  lender — with its remaining schedule and, when it recognises its spending per instalment, its
  expense category (`docs/domain.md`, 4.2 and 5.6); for a credit card created between a
  statement date and its payment due date, that cycle's payment due date, amount due in full and
  minimum payment, entered by hand with no statement (`docs/domain.md`, 5.7).
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
  debtor; a credit card's is its total debt, including purchases since its last statement and the
  principal of instalments not yet billed (`docs/domain.md`, 4.2). The section `Accounts` and
  the paragraph that says accounts are pending confirmation change accordingly.
- **B-44** — The Monthly Budget column of `Categories` is per category, and budgets belong to
  expense category groups (`docs/domain.md`, 5.13, `BUD-1`), so it is not imported as a budget. While reviewing the import,
  the owner enters each expense category group's monthly budget (`docs/product.md`, 7.1
  `financial-structure`). The `Categories` table changes accordingly.
- **B-49** — The `Account Map` table changes. The row `ignore` → "Ignore mapping" is removed:
  identifier mappings have no Ignore target (`docs/domain.md`, 4.4), and the owner deletes the
  `ignore` rows from the spreadsheet before importing. The row `third-party` → "Mapping to a
  third-party account; its ownership settings are asked of the owner" is removed: Trueup has no
  third-party accounts, the spreadsheet has no `third-party` rows, and an account outside the
  owner's finances has no identifier mapping (`docs/domain.md`, 4.5).

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
