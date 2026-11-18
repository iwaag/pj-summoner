# Which braindump grounds which project: the ground workflow

A **link** is one item of a project's `because`: braindump → project. It says that this braindump
is a reason the project is here (agdev user_catalog p7; `workflow_editor/docs/mycatalog.md`
*Links*). This workflow **suggests** the links for the whole catalog at one commit. The owner
agrees per link on the Catalog page (**OK**, **No**, **OK to all**), and only that writes a
`because`. You write none.

> **Never read `braindumps/archive/`.** Not on Gitea, not in a clone, not through `wfe catalog`,
> not the archive folder's `README.md`, not an earlier run's record that reproduces it. The archive
> holds an earlier mapping of braindumps to projects. Your suggestion is compared with it
> afterwards, and the comparison is worth something only when you did not see it. If you come
> upon archived content by accident, stop reading it and say so in `outcome.md`.

Work in the nodes as in every run: **your first action is `wfe run start <run> read`**; write each
node's artifact in the run folder (`devdocs/runs/ground/<run-id>/`), attach it
(`wfe run attach <run> <file> --node <node> --title …`), complete the node with a short concrete
outcome, then start the next. The request is `request.md` in the run folder.

What you never do here: adopt a rule or answer a statement; add or remove a project; write or change a summonplan, `projects.yaml` or
any `because`; write a review or the index; set or propose tags; edit, rename or archive a
braindump; agree or decline a link; request another run. Your **only write to the installation**
is `wfe catalog links suggest`.

## What a link needs

- **A sentence.** A link needs a sentence in the braindump that calls for what the project does,
  or that names the project. You quote it: the `quote` is copied **exactly** from the braindump's
  text, in its language, untranslated, with nothing left out in the middle. The service refuses a
  quote that is not in the text at the head you read.
- **No link from a theme alone.** "Both are about writing" is not a link. Neither is a project
  that could be useful for the wish when the braindump does not ask for it.
- **The review is evidence, not a ground.** A braindump's `review.md` says what of it became real
  and where: it points you to projects worth reading. The link still needs the braindump's own
  sentence.
- **One braindump may ground several projects, and a project may have several grounds.** Quote
  for each link the sentence that fits that project, not the whole braindump.
- **A project with no such sentence is ungrounded.** That is a valid result (games, experiments,
  things made before the wish was written down): report it, do not look harder for a reason. A
  suggestion with no link at all is a valid result too.
- A project's existing `because`, its plan note and another catalog's `because` are what someone
  wrote before: read them, but each link you suggest stands on a sentence you read yourself.

## Disclosure, and the default licence

The same reading answers a second question: **what does a braindump state about a project's
disclosure?** For every project, one of:

- **stated private** (`"follows": "never-publish"`): a sentence says the project is developed in
  private, is not to be published, stays local. Example: 「一部のプロジェクトは非公開で開発する。」
  together with a sentence that names the project in that braindump.
- **stated public** (`"follows": "publish"`): a sentence says the project, or a group the
  braindump defines and the project belongs to by name, is published, shared on the Web, offered
  in public.
- **nothing**: no sentence says either. Most projects. Leave them out.

Rules of this reading:

- **The quotation is required**, copied exactly like a link's. Quote the sentence that states
  the disclosure; when the project is named in another sentence of the same braindump, quote
  both (the service checks each quote is in the text; give them as one `quote` when they are
  adjacent, otherwise the stating sentence and say in `why` where the project is named).
- **A tag is not a statement about a project.** `private` or `public` on a braindump says what
  happens to the braindump, not to the projects it names. A braindump tagged `public` that names a
  project without saying it is public states nothing about that project.
- "Results of public benefit are published where possible" is a statement about *results*, not
  that a named project is public: report it in `note`, propose nothing from it, unless the
  sentence names the project as one that publishes.
- Both stated for one project (two braindumps disagree): propose both; the service keeps private
  and the page says so. Say it in `note`.
- **Never the archive**, as for links.
- A proposed private statement takes effect at once: the publication tool refuses that project
  until the owner answers **No**. A proposed public statement does nothing until **OK**. So be
  exact on both sides; never stretch a sentence towards public.

**The default licence.** When a braindump states the licence public projects get (for example
「公開プロジェクトは基本的にCC0-1.0」), propose it once as `licence` with its SPDX id and the
sentence. If the same braindump says the preference is the owner's own and must not be carried to
others, say so in `why`: the rule is never shared as style (`share: never`, set by the service).

## Run records, workflows and episodes

The same reading answers a third question: **what does a braindump state about a kind of content
of a project?** agdev's `rules.yaml` has a rule about the *content* `runs` (run records: the
history of a run, its request and its report), `workflows` or `episodes` (development episodes),
for one project or for all (`mycatalog.md`, *Rules on run records, workflows and
episodes*, in agdev's `workflow_editor/docs/`). Look for a sentence that says which run records,
workflows or episodes are worth publishing, or must stay local. Example: a run that develops a
workflow or a script carries the project's intent and is worth keeping public; a run that only
uses an established workflow is not worth publishing and must not be published without intent.

- **The quotation is required**, copied exactly, as for every statement. Quote the sentence that
  states the criterion; give `why` in one line (what it says about which content).
- **For which project.** A statement that names one project, or a group the braindump defines that
  the project belongs to by name, is a rule for that project: give `"project"`. A statement about
  run records (workflows, episodes) in general is a general rule: no `project`. Do not make one
  rule per project from a general sentence.
- **What follows.** `suggest-publish` when the sentence says some runs (or workflows, episodes)
  are worth publishing, under a condition the publisher can read; the sentence is then the
  criterion the publisher reads when it judges the project's runs, and it selects nothing.
  `never-publish` only when the sentence says that kind of content is not published at all (the
  publication tool then refuses a contract that selects it). A sentence that says *some* runs are
  not worth publishing is part of the criterion of a `suggest-publish`, not a `never-publish`:
  everything the criterion does not name stays local anyway. Without a rule, run records and
  episodes stay local.
- A proposed `never-publish` takes effect at once (the tool refuses that content until the owner
  answers **No**); a proposed `suggest-publish` does nothing until **OK**. Be exact on both sides;
  never stretch a sentence towards `never-publish`, or towards publishing.
- **A tag is not a statement**, and neither is a sentence about the project's disclosure itself
  (that is the section above): a statement here is about the content kind.
- Never the archive, as for links.
- They go in `suggestion.json` in the same `disclosure` list as the statements on a project's
  disclosure (see the form in *suggest*). Both kinds are statements: report them together.

## read (study)

1. `wfe catalog status`: the **head commit** (as printed; the short form is accepted). It is "the catalog commit read" for this
   run: everything you quote is the text at that commit. Also the braindumps, this installation's
   plan and "Projects".
2. **The projects**: every entry of this installation's plan (`wfe catalog plan show <plan>`: its
   state, roles, note and present `because`) and every project of the catalog's `projects.yaml`
   (`wfe catalog status`). A project in both is one project. For each:
   - `wfe catalog why <project>`: the links it has now;
   - `wfe projects show <project>`: its intent, goals and workflows;
   - its README only when the intent does not tell you what it does
     (`wfe projects read <project> README.md`);
   - the plan entry's roles and note.

   A project that is not acquired here (no workspace) is read from its plan entry and its
   `projects.yaml` description; say in `reading.md` that this was all.
3. **The braindumps**: every braindump of the catalog, for each
   `wfe catalog braindump show <path>` (the text, the projects it grounds now) and
   `wfe catalog review show <path>` (what became real, where). List what the braindump asks: each
   sentence that wants something to exist or to happen, or that names a project. Keep the owner's
   words. Braindumps tagged `mybraindump` are the owner's conventions about braindumps and tags:
   they are braindumps like the others here, and usually ground no project.
4. Write `reading.md`: the commit; per braindump the sentences that ask or name; per project one
   or two lines on what it does, and its present links. Attach it before you complete the node
   (`wfe run attach <run> reading.md --node read --title "What was read"`). It quotes braindumps
   that are in the catalog, which is fine: this run reads no draft.
5. Complete `read` with the head and the counts (braindumps, projects).

## suggest (do)

1. Go through the projects one by one, then through the braindumps one by one, and decide:
   - **statements on run records, workflows and episodes**: see the section above; put them in
     `disclosure` with `content` instead of a disclosure-only `follows` of `publish`.
   - **links**: braindump → project, with the `quote` and, where the quote alone does not show
     it, a `why` in one line (what the project does that the sentence calls for). A link that
     exists already is left out by the service and counted; you need not list it.
   - **drop**: a link that exists and that you would remove, with a required `why`: the braindump
     has no sentence that calls for or names the project (it changed, or the link was made from a
     theme). Do not suggest a drop because a project is unused or unfinished: that is the review's
     subject.
   - **uncovered**: a braindump, or a part of one, that no project serves, with a `why` that says
     what of it nothing serves. One braindump may be linked for one part and uncovered for
     another. You do not propose a project for it: that is `propose`.
   - **ungrounded**: the ids of the projects left without any link, the existing ones you keep
     and the ones you suggest counted.
2. Write `suggestion.json` in the run folder:

   ```json
   {
     "links": [
       {"braindump": "mystyle", "project": "ponderer",
        "quote": "<the sentences of the braindump that say so, copied exactly>",
        "why": "ponderer holds the conversations the sentence asks for"}
     ],
     "drop": [
       {"braindump": "budget/first-thoughts", "project": "warden",
        "why": "the braindump no longer mentions recovery; nothing in it calls for warden"}
     ],
     "uncovered": [
       {"braindump": "soundtool", "why": "it asks for a tool that makes game sound effects; no project here does"}
     ],
     "ungrounded": ["pj-tetris"],
     "disclosure": [
       {"project": "pj-tetris", "follows": "never-publish", "braindump": "private_projects",
        "quote": "<the sentence that states it, copied exactly>",
        "why": "the braindump names pj-tetris in the list it says is developed in private"},
       {"content": "runs", "follows": "suggest-publish", "braindump": "agdev_philosophy/p4",
        "quote": "<the sentence, copied exactly>",
        "why": "says which runs carry the project's intent"}
     ],
     "licence": {"licence": "CC0-1.0", "braindump": "policy",
                 "quote": "<the sentence, copied exactly>",
                 "why": "the owner's own preference, not to be carried into others' styles"},
     "note": "optional: what a reader of the whole suggestion should know"
   }
   ```

   `braindump` is the braindump's path as `wfe catalog status` prints it; `project` is the id in
   the plan or in `projects.yaml`. Empty lists are written as `[]`; no licence stated: leave
   `licence` out. An item with `content` (`runs`, `workflows` or `episodes`) is a statement on
   that content, with `follows` `suggest-publish` or `never-publish`; add `"project": "<id>"` when
   it is about one project, leave it out for a general rule. An item without `content` is a
   statement on a project's disclosure, as before.
3. Send it. This is the only write:

   ```sh
   wfe catalog links suggest --file suggestion.json --head <full head commit> \
     --author summoner-agent --run summoner/ground/<run>
   ```

   The service refuses: a head that is not a commit of the catalog; a braindump that is not in
   the catalog at that head; a project that is neither in the plan nor in `projects.yaml`; a link
   without a quote, or whose quote is not in the braindump's text at that head (white space is
   compared loosely, words are not); a drop of a link that does not exist; a statement on disclosure whose
   `follows` is not `never-publish` or `publish`, a statement with a `content` that is not `runs`,
   `workflows` or `episodes` or whose `follows` is not `suggest-publish` or `never-publish`, or
   whose quote is not in the text. It says which item and
   why: correct `suggestion.json` (copy the quote again from `wfe catalog braindump show`, or
   leave the link out when it has no sentence) and send it again. Do not change `--head` to make a
   quote fit: when the catalog moved while you worked, read the changed braindumps again at the
   new head and say so in `outcome.md`.
4. The suggestion now waits for the owner on the Catalog page; a newer suggestion replaces it.
   Nothing was written to any `because`, and you agree nothing (`wfe catalog links agree` and
   `decline` are the owner's).
5. Write `outcome.md`: the head; what the service answered (links kept, links left out because
   they exist); per project its suggested and dropped links in one line each; the uncovered; the
   ungrounded; the statements on disclosure (project, private or public, the braindump), the
   statements on run records, workflows and episodes (content, project or all, follows, the
   braindump) and the licence; what you could not read; and one line that you did not read `braindumps/archive/`.
   Attach `suggestion.json` and `outcome.md`
   (`wfe run attach <run> suggestion.json --node suggest --title "The suggestion"`, and the same
   for `outcome.md`).
6. Complete `suggest` with the head and the counts: links, drops, uncovered, ungrounded,
   statements (private, public, and on runs, workflows or episodes with their `suggest-publish` and
   `never-publish`), licence or none.

Write `reading.md` and `outcome.md` in English; the quotes stay in the braindump's language. No
host names, addresses, paths outside a repository, or credentials.
