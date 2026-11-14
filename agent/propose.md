# The propose workflow

A `propose` run serves one request: "these are my wishes; which projects should this installation
have?" It may come from a new installation that has only the summoner, or from one that has many
projects. You always start from what the installation has now.

Work in the nodes: start a node (`wfe run start`) when you begin its work, write and attach its
artifact (`wfe run attach <run> <file> --node <node> --title …`), complete it with a short concrete
outcome, then start the next. **Your first action is `wfe run start <run> gather`**, before you read
anything: the node times are how the person follows you, and reading, deciding and writing first
and recording the nodes afterwards in a batch hides the work. The run folder is `devdocs/runs/propose/<run-id>/`; write artifacts
there. Use `$AGDEV_WORKSPACE` for scratch (catalog clones).

## gather (study)

1. Read the request: `braindump.md` in the run folder, and who wrote it (`wfe run show <run>`:
   the input's author). That person is the **requester**.
2. The catalog: `wfe catalog status` (repository, head commit, this installation's plan, braindumps
   with their reviews, the projects registered here and their grounds), and every braindump in full
   with its review (`wfe catalog braindump show <path>`, `wfe catalog review show <path>`).
3. The plan: `wfe catalog plan show <plan>` and its file as YAML (`wfe catalog plan show <plan> --json`
   holds the text). No plan named: say so; the proposal will be a new plan (below).
4. What is in effect: `wfe summon status`.
5. The catalogs the request and the braindumps point at (guide, "Catalogs"), each pinned to the
   commit you read. For each project you may propose, read its README at the catalog's commit
   (`https://raw.githubusercontent.com/<owner>/<repo>/<commit>/README.md` on GitHub): what it
   does, what it needs on the host, its licence. The proposal states these.
6. Write `inputs.md`: catalog head, plan and entries, braindumps (path and title), catalogs read (URL,
   commit, entries) or why one could not be read, and the request's wishes as a numbered list in
   the requester's words. Mark each wish as *in the catalog* (which braindump) or *only in the
   request*.

## propose (do)

1. For each wish, decide what it calls for (guide, "Reading wishes": a person's style → their
   catalog as a whole; starting to develop → the empty core roles; a concrete need → the entries
   that serve it), then find the catalog entries. A project already present (guide rule 4) is not
   added; note that the wish is already served by it. What nothing serves is **uncovered**.
2. Write `proposal.yaml`, a complete `ag.summonplan.v1`:
   - `name`: this installation's plan (or, with none, `installation`); `intent`: kept, or one line
     from the braindumps when there was none;
   - `grounds`: the current catalog head (apply sets it again after adding a braindump);
   - every existing entry as it is (rule 3), then the added entries: `project`, `source`, `commit`,
     `roles` from the catalog, `state: summoned`, `because` from this installation's braindumps,
     including the one you offer (rule 6), and a `note` naming the catalog URL and commit.
3. `wfe catalog plan check proposal.yaml`. The only issue allowed is a `because` naming the braindump
   this proposal adds (it does not exist yet). Fix anything else.
4. Write `proposal.md` (below), attach both files, complete with one line: what is added, kept,
   uncovered.

### proposal.md

1. **What you asked** — the wishes, short.
2. **What this installation has** — catalog commit, plan, its entries in one line each, catalogs read
   (URL and commit) and any that could not be read.
3. **Proposed additions** — per project: what it is and what it will do here; the braindump
   sentences that call for it (quoted, with the braindump's path); whose catalog lists it, whose code it is
   (source URL, commit, licence if the README states it); the roles it fills; what it needs on the
   host if its README says so. Or: "No addition", with why.
4. **Kept** — every existing entry is kept; any `summoned` entry moved to a newer catalog commit,
   and why. Suggested removals, if any, are only suggestions: the plan keeps them.
5. **Roles** — filled after apply, by which project; still empty, and what that means.
6. **Uncovered** — each wish no catalog covers. Creating a project for it is a separate request.
7. **New braindump** — when a wish exists only in the request (covered or not): the path and the
   full text that would be added, author the requester.
8. **If you agree / if not** — on agreement: the braindump added, the plan written to the catalog, and
   `wfe summon apply` (what it acquires, registers and wires). On "change …": a revised proposal.
   On decline: nothing is written.

## agree (do)

- If `proposal.yaml` equals the current plan (apart from `grounds`) **and** there is no new
  braindump, nothing would be written: complete `No change: <why in one line>` without asking, and
  go on (apply completes `No change` too).
- Otherwise ask the requester, from this node:
  `wfe run ask <run> agree --to person --question "<the question>"`. The question says in a few
  lines what would be added (projects, roles, braindump), that `proposal.md` and `proposal.yaml` in
  the run folder hold the details, and what each answer leads to: **agree** (apply now), **change …**
  (a revised proposal), **decline** (nothing written). Wait; the answer resumes you here.
- Take the answer up and act on it:
  - *agree*: complete with who agreed and their words in short.
  - *change*: `wfe run adjust <run> --completing agree --outcome "Changes asked: …"` adding
    `propose-2` (do) → `agree-2` (do) before `apply`, with the person's words as the reason. Do the
    same work there. Repeat while changes are asked.
  - *decline* (or anything that is not agreement): complete `Declined: <their words>`. Apply
    writes nothing.
  - An unclear answer: ask again, from the same node.

## apply (do)

Only after agreement to the exact `proposal.yaml` last presented. Otherwise complete
`No change: <declined | nothing to propose>` and go to record.

1. The new braindump, if any: `wfe catalog braindump add <path> --file <file> --author "<requester>"`.
2. Set `grounds` in `proposal.yaml` to the catalog head now (`wfe catalog status --json`, `head`). Edit
   the line with your file-editing tool or a short `python3` rewrite, not `sed -i` (its syntax
   differs between systems). Read the line back, then `wfe catalog plan check proposal.yaml` (no
   issues).
3. `wfe catalog plan put proposal.yaml --author "<the person who agreed>"`; when the installation had
   no plan, `wfe catalog plan use <name>`. `wfe catalog plan show <name>` must show `grounds` = the head
   you set; if not, correct the file and put it again.
4. `wfe summon apply --yes` (it shows the dry run, then applies). Keep its output in `apply.txt`.
   A refused or failed entry is reported, not repaired by other means: apply can be run again, and
   continues where it stopped.
5. `wfe summon status`; append it to `apply.txt` and attach.

Nothing is restarted after apply: the observer takes the recovery role from `roles.json` at its next
look (apply prints an `observer:` line when that role changed). Do not ask anyone to restart a
service.

## record (do)

Write `outcome.md` and attach it:

- the decision: agreed (by whom, their words), declined, or nothing to propose;
- the catalog commits: the braindump commit and the plan commit;
- apply, per entry: outcome (`done`, `nothing`, `refused`, `failed`) and actions; deployments; the
  `observer:` line if any;
- the roles in effect after apply, and any still empty;
- uncovered wishes, and follow-ups for the person.

The node's outcome is two lines: what the installation has now, or why nothing changed.
