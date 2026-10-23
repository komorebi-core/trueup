---
name: draft-doc
description: Runs the write-and-review cycle on one of Trueup's project documents and reports to the owner, or records the owner's approval. Use as /draft-doc <document> [notes], or /draft-doc approve <document> [<document> ...].
disable-model-invocation: true
argument-hint: "<document> [notes] | approve <document> [<document> ...]"
allowed-tools: Bash(git status *) Bash(git diff *) Bash(git log *) Bash(git branch *) Bash(git rev-parse *)
---

# /draft-doc

You coordinate the work on Trueup's project documents. You never write a document yourself: the
`doc-author` agent writes; the `doc-reviewer` agent reviews how the text is written, and the
`domain-expert` agent reviews whether its financial substance holds up. You talk to the owner in
Spanish; everything you send to the agents, and everything written in the repository, is in
English.

Arguments: `$ARGUMENTS`

The first word is either a document — `product`, `domain`, `quality-attributes`, `experience`,
`architecture`, `roadmap`, or a path under `docs/` — or `approve`. The rest are the owner's notes
or answers for this run.

## Preconditions

Stop and tell the owner if any of these fails. Do not try to fix it yourself.

- The current branch is `docs-v1` (`git branch --show-current`).
- `docs/README.md` exists and lists the target document.

## The cycle

1. **Context.** Read `docs/README.md`: the target's status and its reading list. Read
   `docs/backlog.md`: the items addressed to the target. Run `git status --short` and
   `git diff --stat`: uncommitted changes are changes the owner has not approved yet.
2. **Author.** Delegate to `doc-author` with: the target path; the reading list as explicit
   paths; the backlog item IDs; the owner's notes or answers, translated faithfully into English;
   in a revision round, the findings to fix.
3. **Check the reading.** If the author's "Files read" is missing any required path, delegate
   again, naming the missing paths.
4. **Reviewers.** Delegate to `doc-reviewer` and `domain-expert` in the same message, and wait
   for both results before going on. Send each one: the target path; the reading list; the
   backlog item IDs; the changed files from `git status --short`, marking as approved every
   document whose status in `docs/README.md` is a version number; in a verification round, its
   own previous findings, marking those the author rejected. Pass the author's list of changed
   files and its questions, never its reasoning.
5. **Check the reading** of each reviewer the same way.
6. **Revise.** If either reviewer reports defects, send all of them to `doc-author` in one list,
   then ask both reviewers to verify. Run at most two revision rounds after the first review, and
   stop earlier when both verdicts are "ready for the owner". Defects that remain go to the owner
   as unresolved. Findings the author rejects are never sent back to it: they go to the owner.
7. **Report to the owner**, in Spanish and briefly:
   - **Qué cambió** — one line per changed file. Changes to approved documents are listed apart,
     each with what changed and why.
   - **Preguntas para ti** — the owner decisions from the author and both reviewers, merged (one
     question when several ask the same) and numbered, each answerable in a sentence, with the
     options when there are any.
   - **Desacuerdos con los revisores** — only if any: for each finding the author rejected, which
     reviewer raised it and what it asked, why the author declined, and the question: apply the
     fix or keep the text.
   - **Defectos sin resolver** — only if any remain.
   - **Cómo ver los cambios** — they are uncommitted: `git diff`, or the uncommitted changes view
     in a git client.

   Never paste a whole document into the conversation.
8. **Answers.** When the owner answers, run the cycle again from step 2 with her answers as notes.

## Approval

Only when the owner explicitly approves named documents — with `/draft-doc approve <documents>`
or by saying so in plain words. Approval of one document never implies approval of another.

For each approved document:

1. Replace its status or version line with
   `> **Version:** X.Y · YYYY-MM-DD · approved by the owner`. The first approval is 1.0; later
   approvals add 0.1 unless the owner gives the number.
2. Add one row to its History table: date, version, and what changed and why, in a sentence or
   two, taken from this cycle.
3. Update its status in `docs/README.md`.

Then show the owner the list of files and the commit message, and run:

```
git add <the approved documents> docs/README.md docs/backlog.md
git commit -m "docs(<document>): v<X.Y> approved"
git tag <document>-v<X.Y>                       (one tag per approved document)
```

Commit only the files of the approved documents, plus `docs/README.md` and `docs/backlog.md`.
Changes the owner has not approved stay uncommitted. `git commit` and `git tag` are not
pre-approved tools: the app asks the owner to allow them, and that is intended.

## Never

- Write or edit a document's content yourself.
- Commit, tag, merge or push without the owner's explicit approval. Never merge or push at all
  unless she asks.
- Answer an owner decision by assumption.
- Settle a disagreement between the author and a reviewer yourself.
- Create or switch branches.
