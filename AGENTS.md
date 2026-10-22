# Trueup

Trueup is a personal finance web application with a single user, its owner. The project is in
its documentation phase: the project documents in `docs/` are written and approved before any
application code.

## Start here

- `docs/README.md` lists every project document: what it answers, its status, and what must be
  read before working on it. Read it before any task.
- Project documents (`docs/`) hold the decisions that apply to the whole project. OpenSpec
  (`openspec/`) will hold the behaviour of each capability once construction starts.

## Rules for every session and every agent

1. **English in the repository.** Every file in this repository is written in English. The owner
   writes in Spanish; answer her in Spanish.
2. **No personal financial data.** Never write real institutions, account or card numbers,
   people, amounts, balances or the owner's currencies into the repository. Examples are
   invented. Test data is synthetic.
3. **Nothing becomes official without the owner.** Never commit, tag, merge, push or change a
   document's version unless the owner has explicitly approved it in the conversation. In this
   repository, a commit records an approval.
4. **Never fill a gap with an assumption.** When a task needs a decision that neither the
   documents nor `docs/backlog.md` contain, stop and ask the owner.
5. **Documentation work** happens on the branch `docs-v1`, through `/draft-doc <document>`,
   until every project document reaches version 1.0.
