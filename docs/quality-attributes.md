# Trueup — Quality attributes

> **Version:** 1.0 · 2026-10-28 · approved by the owner
>
> **Answers:** how well Trueup must work, measurably — freshness, availability, privacy and
> security, data ownership, cost, use on a phone, correctness of money and auditability — and
> which changes it must absorb as data, without code changes.
>
> **Does not answer:** why Trueup exists and which capabilities it has (`docs/product.md`) ·
> concepts, rules and invariants (`docs/domain.md`) · flows and screens (`docs/experience.md`) ·
> how any target is achieved (`docs/architecture.md`, `docs/adr/`) · detailed behaviour
> (`openspec/specs/`).

---

## 1. How to read this document

- Each quality is stated as **scenarios**. A scenario names its **source** (who or what acts),
  its **stimulus** (what happens), the **response** Trueup gives, and the **measure** that decides
  whether the response is good enough. Every measure is pass or fail.
- Each scenario has an ID, such as `FRS-1`. Specifications, tests and ADRs cite scenarios by ID.
  No prefix used here is a prefix of the invariants in `docs/domain.md`, 6.
- Terms defined in `docs/domain.md`, such as **figure** (`docs/domain.md`, 2), keep their meaning
  here. This document also uses:

| Term | Meaning |
|---|---|
| **Workspace data** | Every record of the owner's workspace (`docs/domain.md`, `WS-1`) and every file she uploads to it, such as a statement file. Backups and diagnostic records, such as logs and error reports, hold copies of workspace data. Wherever a scenario keeps workspace data from someone or somewhere (SEC-1, SEC-4, SEC-5, SEC-7), it keeps these copies too; the export, backups and retention (PRT-1, PRT-2, PRT-4) do not include them |
| **Credential** | A means of proving identity: the owner's means of signing in, and Trueup's credentials to the staging sheet and to the services it uses. A credential is never workspace data, even when it is kept as configuration of the workspace |
| **Complete read** | A read of the staging sheet proven complete (`CAP-3`) as `docs/reference/staging-sheet.md`, "Reading the sheet completely", describes |
| **Available** | The owner can sign in and use every capability in `docs/product.md`, 7.1 |
| **The owner's work** | What she enters or decides in Trueup: transactions she enters, confirmations, changes, review resolutions, reconciliations with the statements, counts and statement files she records for them, rules and configuration |
| **Design volume** | Up to 300 transactions a month over 10 years: a workspace of up to 36,000 transactions |
| **Production** | The one deployment that holds the owner's workspace data. Every other deployment — development, test, a demo — is an **other environment** |

A scenario holds in production unless it says otherwise.

## 2. Freshness

How long an operation takes to reach the staging sheet depends on the email extractor, which
Trueup does not control (`docs/product.md`, 2). Freshness is therefore measured from the moment a
row is written to the staging sheet.

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| FRS-1 | Email extractor | Writes a row to the staging sheet | Trueup reads it, and the owner sees its **outcome**: a transaction recorded automatically; a review item; the row joining, as another captured record, the transaction already recorded for the same operation (`CAP-4`); or nothing, when `CAP-5` discards the row | 100% of rows, within five days of being written. While no complete read can be processed (FRS-2) or Trueup is not available (AVL-1), the five days count from when that ends |
| FRS-2 | The staging sheet or Trueup's access to it | No read is both complete and processed: the sheet cannot be reached, Trueup's credentials to it no longer work, a read is not proven complete, or its processing fails | Nothing from a read not proven complete is processed (`CAP-3`). Once more than five days have passed since the last complete read that was processed, the dashboard shows a notice stating its cause — Trueup could not process the staging sheet — and the date of the last processed transaction (`docs/product.md`, 7.3) | From five days after the last complete read that was processed until the next one, the notice is shown every time the owner opens Trueup: 0 visits without it |
| FRS-3 | Email extractor | Stops running, while reads of the staging sheet still succeed: the latest run recorded in the sheet's `Import Log` (`docs/reference/staging-sheet.md`) is more than five days old | The dashboard shows a notice stating its cause — the email extractor has not run for more than five days — and the date of the last processed transaction (`docs/product.md`, 7.3) | While the latest run is more than five days old, the notice is shown every time the owner opens Trueup: 0 visits without it |
| FRS-4 | The owner; a classification rule in *auto* mode; a plan's recognition mode | The owner confirms, changes or voids a transaction, or changes data that a figure reads, such as a budget or a category's attributes; or a rule or a recognition mode confirms a transaction | Every figure the change affects reflects it | 0 figures shown after the change that leave it out (`docs/product.md`, 5, O5) |
| FRS-5 | Time | A moment arrives that changes a figure or what needs the owner: the start of the statement date of a cycle that bills an instalment of a plan recognised per instalment; the end of the last day of an occurrence's tolerance; the end of a period; a card cycle's payment due date, or a plan instalment's due date, coming within 30 days | Trueup records the recognition dated on that statement date, confirmed by its plan's recognition mode (`docs/domain.md`, 5.6); flags the occurrence as missing (`docs/domain.md`, 5.12); at the end of a period, flags its figures and accounts as `docs/product.md`, 6, principle 4, states (`IND-2`); and lists the due date among the due dates in the next 30 days (`docs/product.md`, 7.3) | 0 figures, flags or lists of due dates shown from that moment, in the workspace time zone, that leave it out. While Trueup is not available (AVL-1), this holds from when it is available again |

## 3. Availability

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| AVL-1 | Any cause | Trueup is not available while the email extractor keeps writing rows | When Trueup is available again, it reads every row written meanwhile, each with its outcome as in FRS-1, and captures none twice (`CAP-2`) | 0 rows without an outcome; 0 rows captured twice |
| AVL-2 | Any cause, including restoring a backup (PRT-3) | Trueup becomes unavailable | Trueup is available again | 0 periods of unavailability longer than 24 hours in a row |

## 4. Privacy and security

Only the owner can reach any workspace data (`docs/product.md`, 7.1 `access`). These scenarios
make that measurable.

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| SEC-1 | Anyone not signed in as the owner | Asks for workspace data or a credential through any entry point Trueup exposes | Refused, revealing nothing — not even whether a record exists | 100% of such requests refused; 0 workspace data or credentials returned |
| SEC-2 | The owner | Signs in | Trueup admits her once it has verified her identity, with a second factor on a **new device**: one she has not signed in on before, or one whose session she ended from another device (SEC-3) | 0 sign-ins on a new device without a second factor |
| SEC-3 | Someone holding a phone or computer the owner was signed in on, such as a lost phone | Uses Trueup on it after she has signed out on it, or ended its session from another device | Refused, as in SEC-1 | 0 requests from an ended session served after the moment she ends it |
| SEC-4 | Someone observing the network between the owner's device and Trueup, or between Trueup and a service it uses | Captures the traffic | Obtains no workspace data and no credential | 0 workspace data or credentials readable in captured traffic |
| SEC-5 | Anyone other than the owner and the operators of the services that store workspace data | Reads a stored copy of workspace data, or a credential, outside Trueup's interface: in the database, stored files, backups, or diagnostic records | Obtains nothing readable. The copies rely on the standard protection of the service that stores each of them; no key that only the owner holds is used, so that service's operators are not kept from them | 0 workspace data or credentials readable by such a person |
| SEC-6 | Trueup | Reads the staging sheet | It can read the sheet and nothing else, neither in the sheet nor elsewhere at the service that holds it (`docs/product.md`, 7.5) | Trueup's access to the staging sheet allows 0 writes, changes or deletions, and 0 reads of any other file at that service |
| SEC-7 | Any other environment, including a demo if one exists | Runs | Holds no workspace data, except, in a development environment on the owner's own machine: a real statement file she validates the statement file reader against, never committed to the repository (`docs/backlog.md`, B-22); and what that environment reads from the real staging sheet, read-only, during a **reader test run** — a run of the automated tests of the staging-sheet reader, such as its dependency test (`docs/backlog.md`, B-21) — deleted when that run ends. Has no access to production, not even read access. No other environment reads the real staging sheet | 0 workspace data in it, other than those exceptions; 0 production credentials valid in it; 0 credentials to the real staging sheet, except a read-only one in a development environment on the owner's own machine; 0 reads of the real staging sheet outside a reader test run; 0 rows read from it left in the environment once the run that read them ends |

## 5. Data ownership

The owner can export everything and recover from a backup, at any time (`docs/product.md`, 4,
J8).

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| PRT-1 | The owner | Asks for an export of everything | Trueup produces it, with no help from anyone | It contains 100% of workspace data, including void transactions, the history of every change, captured records and statement files, and no credential. Every file Trueup produces for it is in a documented, non-proprietary format that common software, such as a spreadsheet or a text editor, opens without Trueup, and the export describes its own structure; statement files are exported as uploaded. Every account balance recomputed from the exported legs equals the balance Trueup shows, to the last decimal place of its currency |
| PRT-2 | Time | A backup is due | Trueup backs up all workspace data, and keeps one copy of each backup outside every provider that holds the workspace data it copies | 0 intervals longer than seven days between two backups; every backup kept for 30 days; 0 backups without such a copy |
| PRT-3 | The owner | Restores a backup after workspace data is lost or damaged | The workspace returns to its state at the moment of the backup, and Trueup tells her that moment. The next complete read that is processed reads every row for which the restored workspace holds no captured record or kept key (`CAP-2`), each with its outcome as in FRS-1 | 100% of workspace data as it was at the moment of the backup is restored. Every balance and total for a date on or before that moment equals what it was then, until the next read records rows dated on or before that moment. 0 rows of the staging sheet without an outcome. Only the owner's work after the backup she restores is lost: at most seven days of it when she restores the latest backup (PRT-2) |
| PRT-4 | Time | Years pass | Trueup removes nothing on its own; changes keep their history (`TX-6`) | 0 workspace data removed by Trueup, for the life of the workspace |

Because a backup is kept for 30 days, damage found more than 30 days after it was made cannot be
undone from a backup. For example, damage made on 2 March and found when reconciling March on
8 April, 37 days later, cannot be undone: no backup from before the damage is left.

## 6. Cost

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| CST-1 | Every service Trueup uses, such as hosting, database, storage of files and backups, including the copy outside every provider that holds the workspace data it copies (PRT-2), authentication, and a demo if one exists | A month of use at the design volume | Every service is used within its free tier while that free tier lets Trueup meet every target in this document. When a service ends or reduces its free tier so that it no longer does, Trueup pays for that service from then on, and no target is lowered | 0 recurring cost in any month, except for a service whose free tier no longer lets Trueup meet every target, from the month that happens |

## 7. Use on a phone

The owner reviews in short sessions on her phone and analyses on a computer occasionally
(`docs/product.md`, 3).

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| PHN-1 | The owner, on her phone over a mobile connection | Opens Trueup to know whether the month is on track (`docs/product.md`, 4, J1) | Trueup shows the indicators of "Am I on track this month?" (`docs/product.md`, 7.3) | Within one minute of opening Trueup, signing in included, she has read them, with a workspace at the design volume |
| PHN-2 | The owner, on her phone | Works through review (`docs/product.md`, 7.1 `review`): resolves items, turns an answer into a rule, changes a transaction recorded automatically | Every action can be completed on the phone | 100% of review actions; 0 that need horizontal scrolling, zooming or a computer |
| PHN-3 | The owner, on her phone | Enters a transaction by hand, or confirms an occurrence of an expectation or marks it skipped (`docs/product.md`, 7.1 `recurring`) | Every action can be completed on the phone | 100% of these actions; 0 that need horizontal scrolling, zooming or a computer |

Every other action may need a computer, such as reconciliation, uploading a statement file,
configuration and the one-time import.

## 8. Correctness of money

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| COR-1 | Trueup | Computes a figure | The figure is the result of its definition in `docs/domain.md`, with converted amounts and ratios rounded as `docs/domain.md`, 3, "Rounding", states | 0 differences between a figure and its recomputation from an export (PRT-1), from the inputs AUD-1 lists, each reference rate taken exact — from the exported legs it comes from, or as the owner entered it — to the last decimal place of the figure's currency or, for a ratio, the precision at which it is displayed (`docs/experience.md`). For example, legs of 0.10, 0.20 and 33.33 give 33.63 |
| COR-2 | The owner, a rule, a read of a capture channel, or any other part of Trueup | Attempts a change that would break an invariant in `docs/domain.md`, 6 | The change is refused and nothing of it is stored. When the change is the owner's, she is told why it is refused, such as that the period is already reconciled (`REC-1`) | 0 records, at any moment, that break an invariant |
| COR-3 | A failure: a lost connection, a crash, a restart | Interrupts an action of the owner's, or a read being processed | The action is applied whole or not at all, and the owner can see which. An interrupted read is processed again: it reads every row, each with its outcome as in FRS-1, and captures none twice (`CAP-2`) | 0 actions applied in part, such as a transaction stored with only some of its legs; 0 rows without an outcome; 0 rows captured twice; 0 operations notified by more than one source counted twice in any figure (`docs/product.md`, 7.1 `capture`) |
| COR-4 | Two capture channels, two institutions, or two notifications from one institution, under different keys | Report the same operation | One transaction backed by both captured records (`CAP-4`), or a review item with reason *Captured records that may describe the same transaction* (`docs/domain.md`, 5.11) | 0 operations counted twice in any figure, unless the owner confirmed both transactions |

## 9. Auditability

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| AUD-1 | The owner | Asks where a figure comes from | Trueup shows every input its definition reads: the confirmed legs it counts, each with its transaction; the reference rates it used (`MON-3`); every other recorded value it reads, such as a budget, a credit limit, an account's or a category's attributes, a plan's schedule, or a balance from a statement or a count; and its completeness flags (`IND-2`). For a figure made of other figures, it shows each of them | 100% of figures; the inputs shown give the figure by its definition. A reference rate is shown rounded (`docs/backlog.md`, B-73), so the rate shown alone need not reproduce the figure to its last decimal place; the recomputation of COR-1 uses the exact rate |
| AUD-2 | The owner | Asks how a transaction came to be what it is | Trueup shows the captured records it was derived from, exactly as received (`CAP-1`); what confirmed it (`docs/domain.md`, 5.1, "Confirmed by"), showing a confirming rule as it is now, or that it was deleted (`docs/domain.md`, 5.10); each review item that concerned it and its resolution; every change after confirmation, with its time and previous values (`TX-6`); and, when it is void, the reason | 100% of confirmed and void transactions |
| AUD-3 | The owner | Asks how a period of an account was reconciled | For each currency: the balance compared — the statement's closing balance or the count —; the recorded balance it was compared with, which on a credit card whose institution reports only billed amounts leaves out the principal of instalments not yet billed (`docs/domain.md`, 5.8); and the transaction that closed any difference in that currency (`REC-3`). Also the statement file, when she uploaded it | 100% of reconciled periods, in each of their currencies |
| AUD-4 | The owner | Asks whether a read of the staging sheet lost anything | Trueup shows, for each read, its time, the two counts compared to prove it complete (`CAP-3`), and whether it was processed | 100% of reads |

## 10. Modifiability

### 10.1 Changes in the owner's financial life

Each scenario below is a change in the owner's financial life (`docs/product.md`, 3, and 4,
J7). For every one, the source is the owner and the response is the same: she makes the change
herself, in Trueup, by changing data (`docs/domain.md`, 7), or, where the table says so, nothing
in Trueup changes. The measure is the same too: 0 code changes, 0 deployments, 0 changes to
stored data made other than by her through Trueup, and every history recorded before the change
kept. Which changes need code is stated in `docs/domain.md`, 7.

| ID | Stimulus | Data she changes |
|---|---|---|
| MOD-1 | She opens an account, of any kind in `docs/product.md`, 7.2, at an institution already in Trueup or a new one | The institution; the account with its currencies and opening balance; its instruments, identifiers and identifier mappings |
| MOD-2 | A card is replaced, or an account appears under a new identifier | A new instrument or identifier and its mapping; a replaced card stays as an inactive instrument with its history (`docs/domain.md`, 4.4) |
| MOD-3 | An institution starts or stops notifying an account, or changes how its notifications are laid out | Identifier mappings; classification rules. A change in how notifications are laid out changes nothing in Trueup: the email extractor handles it, and Trueup reads only the staging sheet (`docs/reference/staging-sheet.md`) |
| MOD-4 | A new counterparty appears, or a known one appears under new text | Counterparties and their aliases; classification rules |
| MOD-5 | She transacts in a currency not yet in the workspace | The workspace's list of currencies and, if she chooses, the rate she enters for it (`docs/domain.md`, 3) |
| MOD-6 | She reorganises her categories | Categories, groups and their attribute values; a category already used is archived |
| MOD-7 | A budget changes, from a month on or for one month only | A standing amount from a month on, or a monthly override (`docs/domain.md`, 5.13) |
| MOD-8 | She takes a loan, a mortgage or an instalment plan, or a lender revises a schedule | The plan and its schedule; a revision is a new schedule version (`DEBT-2`) |
| MOD-9 | A recurring income or payment starts, changes or ends | Expectations |
| MOD-10 | She closes an account | The account's status (`ACC-5`) |
| MOD-11 | She moves to another time zone | The workspace time zone |
| MOD-12 | Her classification rules stop fitting how she spends | Classification rules, their order and their mode |
| MOD-13 | An institution she has an account with issues its statements in a format of its own, or changes that format | Nothing in Trueup: she generates every statement file she uploads in a single format, whatever the institution (`docs/backlog.md`, B-72) |

### 10.2 A second workspace

Serving several owners is out of scope, and the design must not preclude it (`docs/product.md`,
7.5).

| ID | Source | Stimulus | Response | Measure |
|---|---|---|---|---|
| MOD-14 | The owner, as the person who runs Trueup | Adds a second workspace | The existing workspace keeps working unchanged, and neither workspace reaches the other's data | 0 changes to how the existing workspace's data is structured; 0 changes to the existing workspace's data; 0 data of one workspace returned to someone signed in to the other |

## 11. Changing this document

Changes are explicit: a line in the history below with the date, what changed and why. A
scenario's ID is never reused. A specification or ADR that needs a quality target this document
does not hold stops, and this document changes first.

## History

| Date | Version | Change |
|---|---|---|
| 2026-10-28 | 1.0 | First approved version. Sets measurable targets for freshness, availability, privacy and security, data ownership, cost, use on a phone, correctness of money and auditability, and lists the changes in the owner's financial life that Trueup absorbs as data, without code changes |
