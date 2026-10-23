---
name: doc-standards
description: Standards for writing and reviewing Trueup's project documents in docs/ — what each document contains and must not contain, the writing rules, the zero-context review, how findings are classified, the backlog protocol and the approval protocol. Preloaded by the doc-author, doc-reviewer and domain-expert agents; read it before writing or reviewing any project document.
user-invocable: false
---

# Project document standards

These standards say **how** project documents are written and reviewed. They contain no facts
about Trueup: every fact a reader needs must be in the documents themselves.

## 1. Kinds of file in `docs/`

| File | Role |
|---|---|
| Project documents (`product.md`, `domain.md`, `quality-attributes.md`, `experience.md`, `architecture.md`, `adr/*`, `roadmap.md`) | Decisions that apply to the whole project. Approved by the owner and versioned |
| `README.md` | Index: what each document answers, its status, and what to read before working on it. The reading lists live only there |
| `backlog.md` | Decisions and open questions waiting for their target document |
| `reference/*` | Descriptions of external systems and source formats Trueup depends on |

OpenSpec specifications (`openspec/specs/`) are not project documents. They hold the observable
behaviour of each capability. A project document never restates a specification's behaviour in
detail, and a specification never contradicts a project document.

## 2. What each project document contains

| Document | Contains | Never contains |
|---|---|---|
| `product.md` | Purpose; context and problem; the user; jobs to be done; measurable outcomes; product principles; capabilities with priority (MVP, Later, Out); kinds of account in scope; the questions the dashboard answers | Detailed behaviour; definitions of concepts; quality targets; screens; technology |
| `domain.md` | Glossary; concepts, attributes and relations; kinds and their behaviour; the transaction model; lifecycles; invariants with IDs; what is code and what is data | Priorities; physical schema; technology; screens |
| `quality-attributes.md` | Measurable quality scenarios — source, stimulus, response, measure — for freshness, availability, privacy and security, data ownership (export, backup, restore), cost, use on a phone, auditability; modifiability scenarios that must be absorbed by data alone | How any of it is achieved |
| `experience.md` | Interaction principles; navigation; one flow per job to be done; the screen inventory; states (empty, error, stale, incomplete period, pending, unreconciled); visual language; how numbers, dates and currencies are displayed | Business rules; frameworks or libraries |
| `architecture.md` | System context, including external systems; components; data; security; deployment and environments; backup and restore. An index of ADRs | Domain rules; screens |
| `adr/NNNN-title.md` | One technical decision: context, options considered, decision, consequences, and the evidence of the dependency test it relied on. Never edited after approval; a new ADR supersedes it | More than one decision |
| `roadmap.md` | Increments in order: goal, capabilities included, what the owner must provide first, the demonstration that accepts it, status | Behaviour |

Every project document starts and ends the same way. It starts with a header block: a version
or status line, **Answers**, and **Does not answer**, naming the documents that do. It ends with
a section **Changing this document** and a **History** table. Between them, the body holds what
section 2 assigns to the document.

## 3. Writing rules

Each rule has an ID; findings cite it.

- **W1 — Zero context.** The text is fully understandable by an agent whose only context is this
  repository. It never refers to conversations, chats, meetings, earlier drafts, or files outside
  the repository.
- **W2 — No traces.** The text states what holds now. It never mentions discarded options,
  earlier wording, or how a decision was reached ("we decided", "previously", "instead of the
  old…"). Rationale that explains a current rule is welcome, written in the present tense.
- **W3 — No instance data.** No real institutions, account or card numbers, people, amounts,
  balances, or the owner's currencies. Examples are invented, presented as illustrative, and
  never become defaults.
- **W4 — Kinds, not instances.** Describe kinds of things; concrete things are data the owner
  manages. Test every statement: *if this changes in the owner's life, must code change?* The
  answer must be no, unless the statement introduces a genuinely new kind with behaviour of its
  own.
- **W5 — One home per fact.** Each fact lives in exactly one file. Other files refer to it by
  path and section, and never restate it.
- **W6 — Separation of concerns.** Each document holds only what section 2 assigns to it.
  Product, domain, quality, experience and architecture content are never mixed.
- **W7 — Unambiguous English.** No word with two plausible meanings in its context. Example:
  "figures" can mean numbers or charts — say "computed figures" or name them. Example: "cut-off
  date" also means a credit card's statement date. State rules by what is allowed, not only by what is forbidden: "amounts are
  exact decimals, such as 109.83" rather than only "amounts are never floating-point".
- **W8 — Define before use.** A term is defined at its first use or in a glossary that comes
  before it, and is then used verbatim. A reference to something that appears later says where
  it is. Invariant IDs are plain text, never links.
- **W9 — Only what is expensive to change.** Decide what would be costly to reverse. Leave cheap
  choices (colours, table order, an extra filter) to construction.
- **W10 — Examples where they clarify.** Prefer a concrete, realistic, invented example to an
  abstract sentence. Amounts in examples add up.
- **W11 — Consistent with approved documents.** Text never contradicts an approved document. If
  an approved document must change, the change is made in that document, explicitly, and
  reported as a change to an approved document.
- **W12 — Owner decisions are never invented.** A fact about the owner's finances or a preference
  that neither the documents nor the backlog contain is an open question, not a sentence.
- **W13 — Concise.** No preamble, no recap, no idea stated twice.

## 4. Zero-context review

Read the document as an agent that knows nothing but this repository. Report every place where
such a reader could:

- not understand a term, abbreviation or ID;
- give a word a different meaning from the intended one;
- need information that is not in the repository;
- follow a reference to something that does not exist or says something else;
- find two statements that cannot both be true.

Whatever cannot be understood from the repository alone is a defect, however obvious it seems to
someone with more context.

## 5. Findings

Each finding has a class and a severity.

| Class | Meaning | Resolved by |
|---|---|---|
| **Defect** | The text breaks a writing rule, the zero-context review, or the document map | The author |
| **Owner decision** | The text needs a fact or preference only the owner can give | The owner |
| **Approved-document change** | An approved document was modified | The owner approves or rejects it |

| Severity | Meaning |
|---|---|
| **Blocking** | Misleads a reader, contradicts another document, contains instance data, or leaves a required item out |
| **Minor** | Clarity or concision only |

Format of a finding:

```
<finding ID> · <class> · <severity> · <rule or check ID, if any>
Where: <path> §<section> — "<quoted text>"
Problem: <what is wrong, in one or two sentences>
Fix: <proposed wording or action>              (defects only)
Question: <one question, with options if any>   (owner decisions only)
```

`doc-reviewer` numbers its findings F-1, F-2, …; `domain-expert` numbers them S-1, S-2, …, so
both can be listed together.

## 6. Backlog protocol

- Every item in `docs/backlog.md` names its target document.
- When a document incorporates an item, the item is removed from the backlog in the same change.
- An item that cannot be incorporated stays, and the author says why.
- An item marked **Open** is an owner decision; it is never resolved by assumption.
- A decision that belongs to a document not yet written goes to the backlog, addressed to that
  document.

## 7. Versions and approval

- Only the owner approves. An agent never changes a version line, a History table or a status in
  `docs/README.md`, and never commits; the `/draft-doc` coordinator does that on approval.
- A document in progress has the header line `> **Status:** Draft · pending owner approval`.
- An approved document has `> **Version:** X.Y · YYYY-MM-DD · approved by the owner`. The first
  approval is 1.0; each later approval adds 0.1 unless the owner sets the number.
- Each approval adds one History row: date, version, and what changed and why, in a sentence or
  two.
- While an approved document has changes not yet approved, its version line stays as it was; the
  pending changes are visible as uncommitted changes in git.
- Each approval is one commit, `docs(<document>): v<X.Y> approved`, and one tag per approved
  document, `<document>-v<X.Y>`.
