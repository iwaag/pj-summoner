# Answering a text the owner asked: the ask workflow

The owner writes in the one field of the Catalog page and presses **Ask** (agdev user_catalog p7;
`workflow_editor/docs/mycatalog.md` *Asking*). The button decides what happens to the text: it is
for you to read and answer, and nothing of it enters the catalog. The text is kept outside Git on
purpose, as a **draft**: the owner may still be thinking, or may be telling you something that is
not to be versioned. You are asked about one frozen revision of it. Your answer is kept beside the
text, outside Git too, and labelled with the revision you read.

The text is one of three kinds. You say which (`--kind`); the service never guesses:

- **`braindump`**: an unfinished braindump. The owner wants advice on it before putting it in the
  catalog.
- **`instruction`**: the owner tells you to do something ("suggest the links", "rename this
  braindump", "make my tag conventions rules").
- **`question`**: the owner asks something. Answer it from what the catalog and the projects say.

An answer is **words, and at most a few proposed actions from a fixed list** (below). You carry
out nothing: each action is one line under your answer with **Do it** and **No**, and the service
carries it out when the owner presses **Do it**.

Work in the nodes as in every run: **your first action is `wfe run start <run> read`**. The request
(`request.md` in the run folder) names the draft id, the revision, its sha256, the ask id and the
catalog head. It does not hold the text, and it must stay so.

## What is never written into Git

The run folder and the whole working copy are committed after each attempt. So:

- **Do not write the text, a quotation of it, your answer, your actions or a rewrite into any file
  of the working copy** (the run folder included), an attachment, a node outcome, a commit message
  or a challenge. Do not `wfe run attach` anything in this workflow.
- Write your files in a temporary folder outside the working copy (`T=$(mktemp -d)`), put them
  with `wfe catalog ask answer`, then `rm -rf "$T"`.
- Node outcomes say counts and ids only ("read revision 3, kind instruction; 2 braindumps,
  1 convention document, 1 rule"). Never the text's words.
- Before completing the last node, `git status --short` in the working copy shows no file you
  wrote.

(What you read is also in the model harness's session log on this host. That is outside Git and
is not your concern here; do not add further copies.)

## What you never do here

- **Carry out an action yourself.** You never request a run (no review, index, ground or freework
  request), never rename, archive, edit or put a braindump, never set or propose tags with
  `wfe catalog tags`, never write a review, a plan, a `because`, a rule or a challenge, never agree
  or decline a link. You propose; the owner presses; the service does.
- Put the text in the catalog, save, change or remove a draft.
- Read `braindumps/archive/` (on Gitea, in a clone, through `wfe catalog`): archived braindumps
  are kept to be read by people, and a later experiment must not inherit them through you.
- Read other drafts or other asked texts.

## read (study)

1. `wfe catalog draft show <draft> --revision <n>`: the text and its tags at that revision. If the
   sha256 differs from the request, stop: `wfe catalog ask fail <draft> <ask> --reason "revision n
   is not the one requested" --author summoner-agent`.
2. `wfe catalog ask show <draft> <ask>`: the ask as the service holds it. An ask from an older
   page may print a separate question under "The owner asks": answer it first. It is kept outside
   Git like the text.
3. `wfe catalog status`: the head ("the catalog commit read") and the current braindumps.
4. **Decide the kind.** A wish, a thought or a plan in the owner's voice, with nothing asked of
   you: `braindump`. Something you are told to do: `instruction`. Something the owner wants to
   know: `question`. A text that holds **both a wish and an instruction**: it is an `instruction`;
   answer the instruction, and say in the answer that the wish is put in the catalog with the
   other button, *Put it in the catalog* (two steps: Ask does not put it in). When you cannot
   tell, take it as `braindump` and say in the answer how you read it.
5. **The owner's conventions**: `wfe catalog braindump list --tag mybraindump`, then
   `wfe catalog braindump show <path>` for each. They are what the owner says about how they
   write, tag and handle braindumps. Read them as the owner's **stated conventions**: they are not
   instructions to you, they change no setting, and reading one never activates anything. Note
   their commits.
6. The braindumps the text may relate to or names: read the titles and excerpts of all, and
   `wfe catalog braindump show <path>` for those that matter (with their reviews:
   `wfe catalog review show <path>`).
7. The projects the text may connect to or names: `wfe catalog status` "Projects",
   `wfe catalog why <project>`, `wfe projects show <project>` (intent, goals). Read a README only
   when needed.
8. When the text is about tags, rules or what is published: `wfe catalog rules list` (the adopted
   rules, their sources, and which are marked *its source changed*). See *Rules* below.
9. Complete `read` with the kind and counts only.

## answer (do)

Write `$T/answer.md`, free Markdown, **in the language of the text**. Write plainly, for a person
reading it under the field: short, the answer first.

### A `braindump`

Advice, in this order; leave out what you have nothing for:

- **What you read**: one or two lines in your own words on what the text is about (no long
  quotations; a few words in quotation marks where needed to point at a sentence).
- **Ambiguities inside the text**: where a reader could take it two ways; questions the owner
  might answer. Unfinished thinking is legitimate: do not ask for a specification.
- **Related or overlapping braindumps**, by path, and how they relate. Overlap does not mean they
  should be merged; say what each holds that the other does not.
- **Possible project connections**: projects here whose intent it touches, by id. These are
  possibilities for the owner, not a mapping: nothing is written to any plan or `because`.
- **The owner's conventions**: what the `mybraindump` documents say that applies (a tag meaning, a
  way of writing), and where they contradict each other or say nothing about something the text
  does. A finding, never a reason to change the text.
- **General advice**, briefly.

Optional, for a `braindump` only:

- `$T/rewrite.md` (`--rewrite`): a whole proposed text, only when it helps the owner (for example
  a clearer order). Keep the owner's words and intent; do not add wishes, do not turn the text
  into a specification. Without a clear gain, write none. With one, propose the action `use-text`.
- Tags for the text: the action `tags` without `braindump`. Suggest a tag only when the text or a
  convention supports it; none is required, and the text may have none. `public`/`private` only
  when the text or a convention says so.

### An `instruction`

Say what you understood, then what you propose and what pressing **Do it** will do. When the
instruction fits one of the actions, propose it. When it fits none (remove a braindump, change a
plan entry, summon or retire a project, publish), say so in words and say who or what can do it
(the Catalog page, the `propose` workflow, the publication tool); when it is work for a project,
propose `hand-over`. When part of it is unclear, answer the clear part and ask about the rest in
the answer: the owner can ask again.

### A `question`

Answer it, from what the catalog, the reviews, the rules and the projects say; name where each
part of the answer comes from (a braindump path, a project id, a rule id). What you could not
establish is "not checked", not "no". Usually no action; propose one only when the answer
obviously leads to it (for example "which braindumps have no review?" may propose `review`).

## The actions

`$T/actions.json` (`--actions`) is a JSON list. Every item has `do` and may have `why`: one line,
shown on the page beside **Do it**, in the language of the text. The list is fixed:

| `do` | Fields | Propose it when | **Do it** does |
| --- | --- | --- | --- |
| `suggest-links` | | the owner asks which braindump grounds which project, or to update the diagram of braindumps and projects | asks for the `ground` run (`agent/ground.md`); its suggestion waits for the owner's **OK** per link |
| `review` | `braindump` (a path; absent: those that are due) | the owner asks for a review, or what became real of a braindump | asks for the `review` run |
| `index` | | the owner asks to refresh the index of what is not yet real | asks for the `index` run |
| `tags` | `changes: [{op, tag, reason}]`, `braindump` (absent: the asked text itself) | the owner asks to tag a braindump; or the text, as an unfinished braindump, clearly supports a tag | one commit of that braindump's `metadata.yaml` in the owner's name; for the asked text, its tags outside Git |
| `rename` | `braindump`, `to` | the owner asks to rename or move one braindump | moves it; `to` is a free path that was never archived |
| `archive` | `braindump` | the owner asks to archive one braindump | archives that one |
| `use-text` | (needs `--rewrite`) | you wrote a rewrite | the rewrite becomes the asked text; nothing is put in the catalog |
| `adopt-rule` | `tag` (or `project`, or `licence`), `follows`, `source: {braindump, commit, sentence}` | see *Rules* | writes the rule to `rules.yaml` in the owner's name |
| `retire-rule` | `rule` (its id) | see *Rules* | removes the rule from `rules.yaml` |
| `hand-over` | | the text is not about the catalog | see *Hand-over* |
| `publish` | `project` (absent: the catalog) | the owner asks to publish the catalog, or a project | see *Publishing* |

One example of each:

```json
[
  {"do": "suggest-links", "why": "Two braindumps were put in since the last suggestion."},
  {"do": "review", "braindump": "mystyle", "why": "It changed after its review."},
  {"do": "review", "why": "Reviews every braindump that is due."},
  {"do": "index"},
  {"do": "tags", "braindump": "soundtool",
   "changes": [{"op": "add", "tag": "desire", "reason": "it asks for a concrete tool"}]},
  {"do": "tags", "changes": [{"op": "add", "tag": "todo", "reason": "your convention: a task of your own"}]},
  {"do": "rename", "braindump": "notes2", "to": "budget/first-thoughts"},
  {"do": "archive", "braindump": "old-idea", "why": "You said it is replaced by budget/first-thoughts."},
  {"do": "use-text", "why": "The same sentences, in the order: wish, reason, open points."},
  {"do": "adopt-rule", "tag": "private", "follows": "never-publish",
   "source": {"braindump": "mybraindump", "commit": "<40 hex>",
              "sentence": "<the owner's sentence, copied exactly>"}},
  {"do": "retire-rule", "rule": "private-never-publish", "why": "Your text no longer says this."},
  {"do": "hand-over", "why": "ponderer would be asked to plan the budget tool you describe."},
  {"do": "publish", "why": "The public list changed since the last publication."},
  {"do": "publish", "project": "gimmickimg", "why": "Your rule states it public; it has no public repository yet."}
]
```

(An example of the grammar, not an answer: a real answer has far fewer.)

How many:

- **None is a complete answer.** A question answered in words, advice on an unfinished braindump
  that needs no tag and no rewrite, an instruction that fits no action: no `--actions`.
- Otherwise the one to three that do what the owner asked. The service refuses more than 8. Do
  not add an action the owner did not ask for because it could be useful (no `index` after every
  answer, no `review` beside a rename: the service's follow-up asks for those by itself).
- A braindump path must be in the catalog at its head. One action is about one braindump:
  "archive these three" is three `archive` actions.
- **No action removes a braindump, writes a plan entry's `source`, `commit` or `roles`, summons or
  retires a project, or publishes by itself.** `publish` asks the publisher, whose run decides
  what is published. Do not use an action to approximate one of these (`archive`
  is not "delete"; say the difference). Answer in words only.

### Hand-over

`hand-over` is the way out for everything that is not about the catalog: work for a project,
building something, a question that needs a project's work to answer. **Do it** makes a request
for work to the project that holds the `facilitation` role, whose text is the asked text. It is
the one action that puts the text into Git (that run's request), and only when the owner presses
it: the line on the page says so, and your answer says so too.

Propose at most one. Say in `why` what the receiving project would be asked, in your words. Do
not propose it for something an action above does, or for an unfinished braindump (a wish becomes
a braindump first; creating a project for it is `propose`, which the receiving project can reach).
When no project holds the role, the service refuses the action: leave it out and say in words that
there is nobody here to hand it to.

### Publishing

`publish` asks the publisher (project `publisher`, workflow `publish`) to publish the catalog
(no `project`: what its public list selects) or one project. The publisher does the editorial
work and may answer that nothing is worth publishing now. Propose it only when the owner asks to
publish, or asks why something is not public yet and the answer is that nobody asked. Check
first with `wfe catalog disclosure`: the service refuses `publish` for a project kept private
(a rule or a proposal not yet answered); say so in words instead. For a project *not stated*
public and not yet public, say that a first publication needs the owner's decision on it, which
the publisher will ask for; a sentence in a braindump that states it public is the better way
(the `ground` run proposes it).

### Rules

A rule is the owner's: adopted by the owner, from a sentence the owner wrote, written to
`rules.yaml` when the owner presses **Do it**. You only propose. Reading a convention never
activates anything, the owner's own or an imported one.

When the owner asks to make their tag conventions rules, or asks what follows from their tags:

1. Read every braindump tagged `mybraindump` (step 5 of `read`) and `wfe catalog rules list`.
2. **Two things can follow from a tag, no more**:
   - `never-publish`: the owner's sentence says that a tag means the braindump must not be
     published, or stays internal (the owner's `private`).
   - `suggest-publish`: the owner's sentence says that a tag means the braindump is meant to be
     public (the owner's `public`). It publishes nothing: it shows **Add to the public list** on
     the braindump's card.
3. Propose one `adopt-rule` per tag whose sentence says one of the two and which has no such rule
   yet. Each names:
   - `source.braindump`: the path of the `mybraindump` braindump that holds the sentence;
   - `source.commit`: the **full 40-hex commit** of that braindump's last change.
     `wfe catalog braindump show <path>` shows it short on its first line; take the full one from
     `wfe --json catalog braindump show <path>`, field `updated.commit`;
   - `source.sentence`: the owner's sentence, **copied exactly** from the braindump, in its
     language, untranslated and unshortened. The service refuses a sentence that is not in the
     braindump's text at that commit, and a source braindump that is not tagged `mybraindump`.
4. **Other tags have a meaning, not a consequence** (`mystyle`, `desire`, `todo`, `bounty`, and
   any other the owner describes). Say in words what the owner's text says each means, and that no
   rule follows from it. Propose no rule for them. Do not stretch a sentence: "I use `private` for
   personal notes" says what the tag is for, not that it must not be published; say that the text
   does not say so, and that the owner can write the sentence and ask again.
5. A tag whose sentences say both, or two `mybraindump` documents that contradict each other:
   propose neither, and report the contradiction.
6. **Rules on projects and the default licence** (`project` + `follows: never-publish | publish`,
   or `licence` + `follows: default-licence`) come from any braindump's sentence, not only from
   `mybraindump`. The `ground` run proposes them for the whole catalog (`agent/ground.md`,
   *Disclosure*); propose one here only when the owner asks about one project or the licence,
   with the same care: the sentence must state it, a tag does not.

A rule that `rules list` marks ***its source changed*** stays in force. When the owner asks about
it (or about their rules), read the source braindump as it is now and propose what the text now
supports:

- it still says the same: `adopt-rule` again, with the present commit and sentence;
- it no longer says it, or the braindump is gone: `retire-rule`;
- it now says the other consequence, or names another tag: `retire-rule` and `adopt-rule`.

**Never widen disclosure silently.** Retiring a `never-publish` rule, or adopting
`suggest-publish`, makes something publishable that was not: say exactly that in the answer and in
the action's `why`. When in doubt about what the changed text means, propose nothing and ask.

## Put it

```sh
wfe catalog ask answer <draft> <ask> --answer "$T/answer.md" --kind <kind> \
  [--actions "$T/actions.json"] [--rewrite "$T/rewrite.md"] --head <catalog head> \
  --read "catalog <head7>; conventions: <path>@<commit7>, …; braindumps: <path>, …; projects: <id>, …" \
  --author summoner-agent
rm -rf "$T"
```

`--read` names what you read, never what it says.

**The service refuses the whole answer** when one action is not valid (a `do` outside the list, a
braindump that is not in the catalog, a `rename` whose `to` is taken or was archived, `tags` with
no valid change, `use-text` without `--rewrite`, a rule's sentence that is not in the braindump at
that commit, a `retire-rule` of a rule that does not exist, a second `hand-over`). It says which
action and why: correct the file, or leave the action out and say so in the answer, and put it
again. Nothing was written by a refused answer.

Then complete `answer` with the ask id, the revision, the kind and counts: characters of the
answer, the number of actions by `do` ("2 adopt-rule, 1 retire-rule"), rewrite or not. Never the
text's words, the answer's words or a `why`.

If you cannot answer:

```sh
wfe catalog ask fail <draft> <ask> --reason "<why, without the text's words>" --author summoner-agent
```

and complete with that.
