# Project documents

Project documents hold the decisions that apply to the whole of Trueup. Each one answers a
single question. Together they are the context for every specification and every line of code.

## Documents

| Document | Answers | Status | Read in full before working on it |
|---|---|---|---|
| `product.md` | Why Trueup exists, for whom, what success means, which capabilities it has and in what priority | 1.3 | — |
| `domain.md` | What each concept is, how concepts relate, which rules always hold, what is code and what is data | 1.0 | `product.md` |
| `quality-attributes.md` | How well Trueup must work, measurably, and which changes it must absorb without code changes | Not started | `product.md` · `domain.md` |
| `experience.md` | How the owner uses Trueup: flows, screens, states and visual language | Not started | `product.md` · `domain.md` · `quality-attributes.md` |
| `architecture.md` and `adr/` | With what and where Trueup is built; one decision record per technical decision | Not started | `product.md` · `domain.md` · `quality-attributes.md` · `experience.md` · `reference/` |
| `roadmap.md` | In what order Trueup is built, and what the owner provides before each step | Not started | Every document above |

Before working on any document, also read the items addressed to it in `backlog.md`.

## Supporting files

| File | What it is |
|---|---|
| `backlog.md` | Decisions and open questions waiting for the document that will hold them. An item is removed when that document incorporates it |
| `reference/staging-sheet.md` | The structure of the staging sheet Trueup reads, and how to read it completely |
| `reference/spreadsheet-import.md` | The structure of the owner's spreadsheet, and the rules for importing its configuration |

## Reading order

`product.md` → `domain.md` → `quality-attributes.md` → `experience.md` → `architecture.md` →
`roadmap.md`.

A document may refer to a later one by path. A reference to a document whose status is *Not
started* points to where that decision will live.

## Status values

| Status | Meaning |
|---|---|
| *Not started* | Planned; the file does not exist yet |
| *Draft* | Being written; not approved |
| A version number | Approved by the owner. The document's own history says what changed in each version |
