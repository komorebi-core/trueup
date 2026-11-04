# Backlog

Decisions and open questions waiting for the document that will hold them. Each item names its
target document. When that document incorporates an item, the item is removed from this file in
the same change.

An item marked **Open** needs the owner's answer. It is never resolved by assumption.

## For `experience.md`

- **B-12** — **Open.** The language of the user interface.

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
- **B-22** — The reader of statement files, whose single format B-72 sets, is validated by the
  owner against a real statement file of hers, in a development environment on her own machine
  (`docs/quality-attributes.md`, SEC-7); real statement files are never committed.
- **B-74** — How the owner starts restoring a backup (`docs/quality-attributes.md`, PRT-3): from
  inside Trueup, or with the tools of the services that hold the backups; and whether Trueup lists
  its backups to her. Whichever way, the first time she opens Trueup after a restore, it states the
  moment of the backup the workspace returned to (`docs/experience.md`, 4.8 and 6.3).

## For `roadmap.md`

- **B-23** — AI suggestions stay in Later. Whether to build them is decided after measuring how
  many captured transactions still need review once rules are in use.
- **B-24** — Owner prerequisites before first use: an opening balance for every account she
  creates (`docs/domain.md`, 4.2); a property
  whose opening balance is the mortgage's outstanding principal on the property's creation date;
  each plan already running — loan, mortgage, or instalment plan on a card or with another
  lender — with its remaining schedule and, when it recognises its spending per instalment, its
  expense category (`docs/domain.md`, 4.2 and 5.6); for every credit card, its statement day and
  payment due date rule, and no other cycle data (`docs/domain.md`, 5.7).
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

## For `reference/statement-file.md` (written once the owner has generated the file)

- **B-72** — Every statement file the owner uploads, from any institution, is an Excel file in a
  single format, which she generates herself from the institution's statement; an institution's
  own statement format is not Trueup's concern (`docs/quality-attributes.md`, MOD-13; B-22). The
  file carries only lines, one per line of the statement: each transaction line the statement
  has, and each plan instalment (`docs/domain.md`, 5.6) that the statement bills or shows paid
  (`docs/domain.md`, 5.8). It carries no closing balance, which the owner enters by hand when she
  reconciles, and no other data of the statement: a payment due date, an amount due in full or a
  minimum payment on the institution's statement is ignored (`docs/domain.md`, 5.8).

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
