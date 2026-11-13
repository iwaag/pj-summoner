# summoner

summoner is a project for an [agdev](https://github.com/iwaag/agdev) installation. It answers one
question: **"I want to do this; which projects should my installation have?"**

You write what you want as a **braindump**. summoner reads it with the installation's other
braindumps and its current **summonplan**, looks at the **catalogs** of projects that people share,
and proposes a plan: which projects would arrive, the sentences of yours that call for each, whose
code they are, which of the core's roles they fill, and what no catalog covers. You agree, ask for
changes, or decline. Only after you agree does it add your braindump to the mind, write the plan and
run `wfe summon apply`.

This repository is the project as an agdev installation takes it in: a `project.yaml`, the agent's
guides and the workflow definitions. It is not a program you run by itself. It needs an installation
of the agdev core ([`install/`](https://github.com/iwaag/agdev/tree/main/install)); the
core is what runs the workflows and holds the mind. The formats and commands are the core's:
[`mind.md`](https://github.com/iwaag/agdev/blob/main/workflow_editor/docs/mind.md) (braindumps,
summonplans, catalogs, roles, `wfe mind`) and
[`summon.md`](https://github.com/iwaag/agdev/blob/main/workflow_editor/docs/summon.md)
(`wfe summon apply`).

## Getting it

1. **A new installation receives summoner first.** After the core is up, `agdevctl bootstrap-projects`
   without `--plan`, on an installation whose mind names no plan yet, applies the core's reference summonplan,
   [`install/bootstrap/summonplan.yaml`](https://github.com/iwaag/agdev/blob/main/install/bootstrap/summonplan.yaml)
   (written as the plan of the installation, named after its slug). That plan names this repository at
   one commit; summoner fills no role. The core's
   [`docs/install.md`](https://github.com/iwaag/agdev/blob/main/docs/install.md) (Setup) gives the
   order: `bootstrap-projects`, write a braindump, request `summoner/propose`.
2. **Any installation can also put summoner in its own plan**, with this repository as its `source`
   and a commit of this repository as its `commit`, like any project. Use `wfe mind plan put` and
   `wfe summon apply`, or `agdevctl bootstrap-projects --plan <file>` (see
   [`summon.md`](https://github.com/iwaag/agdev/blob/main/workflow_editor/docs/summon.md),
   "Bootstrap").

## Asking for a proposal

Write your wish in your own words, one file, for example "reproduce iwaag's development style and
start developing". Request a run of the workflow **`propose`** with it as the braindump:

- the WFE page → project **summoner** → *Request a run* → `propose`; or
- `wfe request propose --workspace summoner --author <you> --braindump <file>`.

## What happens, and how to answer

1. **Gather.** It reads your request, the mind (every braindump, the plan in use), what the
   installation already has, and the catalogs your wish points at, each pinned to the commit it
   read. A catalog is the file `catalog.yaml` of a repository `<user>/agdev-mind`. A request that
   names a person ("iwaag's ...") points at that person's catalog; a request that names none and
   asks for projects uses the one catalog summoner knows,
   [`iwaag/agdev-mind`](https://github.com/iwaag/agdev-mind) (iwaag, who develops agdev), and says so.
2. **Propose.** It writes `proposal.md` (what you asked, what you have, each addition with your
   quoted sentences, whose code, roles filled and still empty, what is uncovered) and
   `proposal.yaml` (the complete summonplan) in the run folder.
3. **Ask.** When the proposal would write anything, the run asks you on its question. Answer
   **agree**, **change ...** (say what; you get a revised proposal) or **decline** (nothing is
   written).
4. **Apply**, only after you agree to the exact proposal you were shown: it adds your braindump if one
   was offered, puts the plan in the mind and runs `wfe summon apply`. `outcome.md` says what the
   installation has afterwards.

A proposal that adds nothing is a valid result; it says why.

## What it does not do

- It never removes a project from the plan; it may suggest it.
- It does not create projects that no catalog has: it says the wish is uncovered.
- It changes the installation only with `wfe mind` and `wfe summon`, and only after you agree.
- It adds nothing that no sentence of yours calls for.
- It does not restart services. When apply says the observer must be restarted (the recovery role
  changed), run `agdevctl services-config --restart`.
- It trusts the catalogs it reads only as far as you do: an added project is someone else's code that
  your installation will run. The proposal says whose.

## Files

- [`project.yaml`](project.yaml): the project definition.
- [`agent/guide.md`](agent/guide.md): the agent's rules and the catalog convention.
- [`agent/propose.md`](agent/propose.md): the procedure of `propose`.
- [`devdocs/workflows/propose.yaml`](devdocs/workflows/propose.yaml): the workflow you request;
  `freework.yaml` is a general conversation workflow.
- [`PROVENANCE.md`](PROVENANCE.md): how this repository relates to its development history.

License: CC0 1.0 Universal ([`LICENSE`](LICENSE)).
