# Backlog

Decisions and open questions waiting for the document that will hold them. Each item names its
target document. When that document incorporates an item, the item is removed from this file in
the same change.

An item marked **Open** needs the owner's answer. It is never resolved by assumption.

## For `domain.md`

- **B-1** — State the rule on amounts positively: amounts are exact decimals — for example,
  109.83 — kept with exactly their currency's number of decimal places; binary floating-point
  types are never used for money. Align `MON-1`.
- **B-2** — Redefine the reference rate. It is the rate implied by the owner's most recent
  transaction that mixes the two currencies, on or before the date. Before any such transaction
  exists, the owner enters one rate at the start date. There is no external rate source and no
  daily rate record. Every converted figure states the rate it used and that rate's date. The
  difference between a rate used to estimate a figure and the rate of a later payment is not
  recorded as a cost. Update the glossary, section 3 and `MON-3`, and remove the mention of a
  rate source in `docs/architecture.md`.
- **B-3** — Rates are stored; conversions are not. The rate of a transaction that mixes
  currencies is recorded with that transaction, implied by its two stored amounts. Converted
  amounts are never stored: they are computed when a figure that combines currencies is shown —
  budgets, dashboard totals, net worth, credit utilisation. A transaction is always shown in its
  own currency. Align `MON-2`.
- **B-4** — Add an example: a charge in a foreign currency on a multi-currency credit card, then
  that balance paid from an account in another currency, with the rate taken from the payment
  (illustrative: a charge of USD 20.00; a payment debiting EUR 18.40 and applying USD 20.00;
  rate 0.92). If the payment's notification gives only the amount debited and not the amount
  applied in the card's currency, the missing amount is a review item, or comes from the
  statement.
- **B-5** — Rename *cut-off date* to *start date* everywhere: glossary, account attributes,
  adjustments and examples. In credit-card English, "cut-off date" also names a card's statement
  date, so the current term is ambiguous.
- **B-6** — List a credit card's own attributes explicitly in 4.2–4.3 and 5.7: credit limit (one,
  in one currency, shared by all the card's currencies); **statement date** (the day of the month
  the cycle closes); **payment due date rule** (a fixed day of the following month, or a number
  of days after the statement date). Each cycle's actual dates are computed from these and
  replaced by the statement's when one is uploaded. Use "statement date" and "payment due date"
  consistently instead of "closing day" and "closing date".

## For `product.md`

- **B-7** — **Open.** The one-time configuration import resolves identifier mappings to
  accounts, so it must also create those accounts, each with its kind and currencies confirmed
  by the owner. `product.md` currently lists categories, rules and identifiers only. Confirm
  whether accounts are part of the import.
- **B-29** — Rename *cut-off date* to *start date* (see B-5): in 7, the definition of the MVP, and
  in the `financial-structure` row.

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
- **B-15** — A converted figure states the rate and its date. A transaction is always shown in
  its own currency, optionally with an approximate converted amount beside it.
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
- **B-24** — Owner prerequisites before first use: an opening balance for every account at the
  start date, taken from real statements; a property recorded at the mortgage's outstanding
  principal at the start date.
- **B-25** — The spreadsheet remains the system of record until Trueup replaces it. There is no
  period without a system.
- **B-26** — Trueup is deployed from the first increment, so that technical decisions are tested
  in real use from the start.

## For `AGENTS.md` and `openspec/config.yaml` (end of the documentation phase)

- **B-27** — Method for construction: OpenSpec changes; requirements with WHEN/THEN scenarios,
  each turned into a test; three checkpoints per increment — scope agreed, dependencies
  verified, acceptance tests passing; if a problem is fixed by adding data (a rule, a mapping), it
  is not a specification change; test data is synthetic.
- **B-28** — Every `design.md` cites the section of `docs/domain.md` or the ADR it relies on. If a
  change needs a decision no project document holds, it stops and the project document is
  amended first. Decisions in a `design.md` that other changes will depend on are promoted to an
  ADR when the change is archived.
