# Catalog upkeep: the review and index workflows

Two workflows keep the user's catalog honest about what is real. Both write through `wfe catalog`
only, as you (`--author summoner-agent`: the Git author is your signature), and both say which
catalog commit they read. Neither asks a person before writing: a review and the index are your
reading, signed and dated, and the person can correct or replace either.

Work in the nodes as in every run: **your first action is `wfe run start <run> <first node>`**; write
each node's artifact in the run folder (`devdocs/runs/<workflow>/<run-id>/`), attach it
(`wfe run attach <run> <file> --node <node> --title …`), complete the node with a short concrete
outcome, then start the next. The request is `request.md` in the run folder
(`braindump.md` when the installation's core is older than the rename).

What you never do here: edit or remove a braindump; change a challenge (its state, summary, files or
trials); write a summonplan; apply anything. If a braindump or a challenge looks wrong, say so in
your review or in the node outcome.

## What the catalog holds

`workflow_editor/docs/mycatalog.md` in the core describes it. In short:

- a **braindump** (`braindumps/episodes/<path>/braindump.md`) is what the person wants and why;
- a **review** (`review.md` beside it) is free text on how far that became real;
- a **challenge** (`challenges/<id>/`, `workflow_editor/docs/challenges.md`) is a problem someone met
  during work, with trials and evidence; its `summary.md` has a `state` (`open`, `resolved`,
  `withdrawn`, or the writer's own word);
- **`challenges.md`** at the root is the index of what is not yet real, from both.

A braindump and a challenge are two kinds. A braindump comes from above (a wish); a challenge comes
from below (a problem met while working). Do not turn one into the other, and do not merge them into
one list without saying which is which.

## review

The request names one braindump path, or asks for every braindump that needs a review.

### select (study)

1. `wfe catalog status`: note the head commit. It is "the catalog commit read" for this run.
2. Targets: the named braindump; otherwise every braindump whose line says `no review` or `the
   braindump changed after its review`. No target: complete every node with `Nothing to review:
   every braindump has a current review`, and stop.
3. Write `targets.md`: the commit, each target with its reason.

### read (study)

For each target:

1. `wfe catalog braindump show <path>` (the text, the projects it grounds, the plans that name it)
   and `wfe catalog review show <path>` (the present review, if any, and its commit).
2. List what the braindump asks: each sentence that wants something to exist or to happen. Keep the
   person's words.
3. For each project the braindump grounds: `wfe catalog why <project>`, `wfe projects show <project>`
   (its intent, goals, workflows), its README (`wfe projects read <project> README.md`), and its runs
   (`wfe projects runs <project>`): whether it is used, and for what. `wfe summon status` says which
   roles are in effect. Read a run's result when a claim depends on it
   (`wfe projects run <project> <workflow>/<run>`).
4. Tracked content is not everything: a search that finds nothing is no evidence that a feature is
   absent (`wfe help projects`). When you could not establish something, it is **not checked**, not
   "not real".
5. What cited the braindump: `wfe catalog braindump show <path>` lists "cited by" when the core
   records it (a core from user_catalog p4 on): each conversation message and each run whose request
   named the braindump as its ground, with the commit it was cited at. They are evidence of work
   started toward the braindump, not of its result: read the run or the conversation
   (`wfe projects run <project> <workflow>/<run>`, `wfe conversation show <id>`) before you say what
   became real, and name them in the review. A braindump nobody cited may still be real through
   the projects it grounds.
6. Open challenges that name one of these projects (`wfe challenge list --project <project> --state
   open`) are evidence of what is not yet real: name them by id.
7. Write `evidence.md`: per target, each thing asked, what you found and where (project, file, run,
   command), or that it was not checked.

### write (do)

1. Write the review as free Markdown, in English unless the braindump's present review is in
   another language. No fixed form; this order serves most braindumps:
   - **first line**: `_Review by summoner-agent, <date>, catalog <full head commit>._`
   - what the braindump asks, against what exists now and where (a table reads well when there are
     several things);
   - what is not real;
   - what was not checked.
   State what exists and is used. Do not judge how well a project does its work unless a record
   shows it. No host names, addresses, paths outside a repository, or credentials.
2. Put it:

   ```sh
   wfe catalog review put <path> --file <review file> --author summoner-agent [--base <commit of the present review>]
   ```

   `--base` when the braindump had a review: a refusal means someone changed the review while you
   worked; read it again and decide whether yours still adds something. Do not overwrite a review a
   person wrote after your reading without taking it in.
3. Write `outcome.md`: per target, the commit written, or why no review was written. Attach it and
   complete the node with the count.

## index

### read (study)

1. `wfe catalog status`: the head commit ("the catalog commit read"), the braindumps and whether each
   has a review.
2. Every braindump with its review: `wfe catalog braindump show <path>`, `wfe catalog review show
   <path>`.
3. Every challenge that is not resolved or withdrawn: `wfe challenge list --state open`, and also
   `--state custom` and `--state unspecified`; then `wfe challenge show <id>` for each: its
   statement, its trials and what they reached.
4. The present index, if any: `wfe catalog index show` (its commit is your `--base`).
5. Write `reading.md`: the commit; per braindump what its review says is not real (or that it has no
   review, or that it changed after its review); per challenge what is still missing. Attach it
   before you complete the node (`wfe run attach <run> reading.md --node read --title "What was
   read"`): the index's first run completed `read` without it, and its reading could not be opened
   from the run view.

### write (do)

1. Write `challenges.md` whole. It replaces the present one; it is not appended to.
   - **first line**: `_Index by summoner-agent, <date>, catalog <full head commit>._`
   - a short line on what the file is: what is not yet real, from braindumps and from challenges.
   - **one entry per thing that is not yet real**, each with: its kind (`braindump` or `challenge`),
     a link, and one or two lines on what is missing. Links are relative to the catalog's root:
     `braindumps/episodes/<path>/braindump.md`, `challenges/<id>/summary.md` (or `challenges/<id>/`
     when it has no summary).
   - A braindump whose review says several things are not real may have several entries, or one that
     names them; choose what a reader can scan.
   - A braindump with **no review** is listed under its own heading as *not reviewed*: you do not
     judge it here. One whose review is older than its last change is listed with that note.
   - A braindump whose review says everything it asks is real has no entry. You may end with a short
     list of those, so that the reader sees they were read.
   - Group as a reader would look for them (by kind is the default). Keep it short: the index is for
     finding, the reviews and challenges hold the detail.
   - A challenge's title and state are quoted as they are. You do not resolve, withdraw or rewrite a
     challenge by listing it differently.
   - No host names, addresses, paths outside the repository, or credentials: parts of the catalog
     are published.
2. Put it:

   ```sh
   wfe catalog index put --file <the file> --author summoner-agent [--base <commit of the present index>]
   ```

3. Complete the node with the commit and the number of entries of each kind.
