# Changing the catalog's side of a publication: the catalog-publication-edit workflow

The publisher (project `publisher`, workflow `publish`) publishes the catalog — the target
`mycatalog`, the owner's `<owner>/mycatalog` — like any project: it reads what the public list
selects, edits, assesses and pushes with the publication tool. **You are the catalog's owning
agent.** When the publication needs the catalog to change, the publisher delegates the change to
this workflow, as it delegates a project's change to that project's `publication-edit`. The
procedure of `publisher:agent/source-change.md` applies, with one difference: the catalog is not
a project repository that WFE integrates. **You write the catalog only through `wfe catalog`**,
and each command is one commit on the catalog's `main` at once.

Work in the node: **your first action is `wfe run start <run> apply`**; write `outcome.md` in the
run folder (`devdocs/runs/catalog-publication-edit/<run-id>/`), attach it, and complete the node
at the end. The request is `request.md` in the run folder.

## What you may change

| The change | Command |
| --- | --- |
| the public list, `[[file.edit]]`, `forbidden`, an `[[allow]]` | `wfe catalog publication put publication/github.toml --file <f> --author summoner-agent --base <the base commit> --message "publication: … (publisher/publish/<run>)"` |
| a public-only text (`publication/overlay/README.md`, …) | `wfe catalog publication put publication/overlay/<file> --file <f> …` (`--remove` to remove one) |
| a braindump's own files in the list / `rules.yaml` in the list | `wfe catalog publication add <path> \| rules.yaml --author summoner-agent` |
| a review that tells more than a public reader should have | `wfe catalog review put <path> --file <f> --author summoner-agent --base <its commit>` (see *Reviews*) |

`rules.yaml` is published in a **derived public form**, made by the publication tool: only the rules
whose source braindump the same candidate selects, that are not `never-publish` on a project and not
`share: never`; without `source.commit`; `adopted.by` the public account. It is one plain
`[[file]] rules.yaml` in the list; a `[[file.edit]]` on it is refused. Never trim it by hand.

Never: a braindump's text or tags (the owner's words: if one is the problem, say so in your
outcome and the publisher asks the owner), a rule (`rules adopt | retire`), a link, a plan,
`projects.yaml`, the index. Never `git push` to the catalog, never edit the service's clone.

## Procedure

1. Read the request: the purpose, the catalog's base commit, the patch (a `git format-patch` of
   the catalog repository, or the files whole), what to check.
2. `wfe catalog status` (head) and `wfe catalog publication status`. When the head is not the
   base and a file of the patch changed after the base (`wfe catalog publication status`,
   `git log <base>..` in a clone), do not overwrite it: apply your change onto what is there if
   it still fits, else refuse it with what changed.
3. To apply a patch, clone the catalog into the run envelope
   (`git clone <the catalog's Gitea URL> "$AGDEV_WORKSPACE/catalog"`, the URL from `wfe catalog
   status`), `git am --3way` or `git apply --3way` there, and write each resulting file with the
   command above. The clone is a scratch copy: nothing of it is pushed.
4. Judge as the catalog's owner:
   - **Nothing the owner keeps private goes into the list**: `wfe catalog disclosure` (projects
     kept private) and `wfe catalog publication check` (braindumps kept private by a tag rule).
     The service refuses a contract the rules refuse; do not work around a refusal.
   - **A name of a private project is allowed where the public text needs it** (the owner's
     decision); what tells more than the name — what the project is, its state, its runs, its
     files — is removed or generalized. The check lists every selected file that names one
     (`project …` lines): read each place.
   - **A name of a private project** (label `private:<id>`): the publication tool refuses a
     candidate that names one unless the contract has an `[[allow]]` for that file with
     `found = "private:<id>"` and a `why` (why the public text needs the name). Where a name is
     needed, add that `[[allow]]` with `wfe catalog publication put publication/github.toml`; what
     tells more than the name is still rewritten, never allowed. `wfe catalog publication check`
     shows each name as kept (with its `why`) or refused.
   - The public text says what the catalog says: an edit must not change what a braindump means.
5. Each command is one commit in your name; name the publisher run in `--message` where the
   command takes one.
6. `outcome.md`: per file the catalog commit, what you changed relative to the request and why,
   what you refused and why. Attach it (`wfe run attach <run> outcome.md --node apply --title
   "Outcome"`). Complete `apply` with the last catalog commit (full) and counts. The publisher
   fetches the catalog's `main` and builds from that commit.

## Reviews

A review (`review.md`) is your own text about the owner's installation, published beside a public
braindump. When the publisher asks for a review to be rewritten (it quotes a braindump that is not
public, tells what a private project is or how far it got, names hosts or run ids), rewrite the
review as a review — what became real, where — with those parts generalized or left out, and say
in the review that it is the public form. Do not invent; a shorter review is fine.

Write `outcome.md` in English. No host names, addresses, paths outside a repository, or
credentials in anything you write to the catalog.
