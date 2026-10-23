# Backlog

Decisions and open questions waiting for the document that will hold them. Each item names its
target document. When that document incorporates an item, the item is removed from this file in
the same change.

An item marked **Open** needs the owner's answer. It is never resolved by assumption.

## For `product.md`

- **B-7** — **Open.** The one-time configuration import resolves identifier mappings to
  accounts, so it must also create those accounts, each with its kind and currencies confirmed
  by the owner. `product.md` currently lists categories, rules and identifiers only. Confirm
  whether accounts are part of the import.

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

## For `AGENTS.md` and `openspec/config.yaml` (end of the documentation phase)

- **B-27** — Method for construction: OpenSpec changes; requirements with WHEN/THEN scenarios,
  each turned into a test; three checkpoints per increment — scope agreed, dependencies
  verified, acceptance tests passing; if a problem is fixed by adding data (a rule, a mapping), it
  is not a specification change; test data is synthetic.
- **B-28** — Every `design.md` cites the section of `docs/domain.md` or the ADR it relies on. If a
  change needs a decision no project document holds, it stops and the project document is
  amended first. Decisions in a `design.md` that other changes will depend on are promoted to an
  ADR when the change is archived.
