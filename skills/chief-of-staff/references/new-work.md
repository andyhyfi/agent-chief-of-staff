# New work

How a request enters the system, where it goes, and how its files are kept. Paths use `<notes>` for the notes folder named in the README.

## How a request enters

1. The request becomes one line in `<notes>/chief-of-staff/inbox.md`. Format: `examples/inbox.md`.
2. The chief decides where the request belongs (placement rules below) before filing it.
3. Accepted work that a specialist can advance becomes a kanban card in the same pass. The card has a named owner, a full context brief, and a done-when. The brief attaches raw sources, never a pre-digested outline.
4. Work only the owner can do goes in the "Your own" section of `TODAY.md`, with its prep linked.
5. The inbox line closes once the work has a card, a weekly-file line, or a workflow.

## Placement rules

The workflow registry at `<notes>/chief-of-staff/workflow-registry.md` lists every repeating workflow. The registry is not locked. When the owner brings work, the chief places it in one of three ways:

1. **Fits an existing workflow.** File it there, and name the workflow in one line.
2. **Stretches one.** Propose the change to the owner in chat. On the owner's yes, update the registry and log the change in the registry change log.
3. **Fits nothing.** Propose a new workflow in chat as five lines: name, owner, trigger, files, done-when. On the owner's yes, add it to the registry as Pilot. Review it with the owner after two uses. Template: `examples/workflow-registry-entry.md`.

Rules:

1. Do not squeeze messy new work into an old workflow to avoid asking.
2. Do not build a workflow for a one-off. A one-off is a line in the current weekly file.
3. Move a workflow unused for four weeks to Dormant.
4. Move a workflow you keep bending to Needs redesign, and raise it at the next Friday routine.

## Files of record

When the owner names a file, deck, or doc as the record for a workstream ("the deck we pitch with"):

1. Each workstream has one file of record.
2. Copy the file into `<notes>/library/assets/<name>/<YYYY-MM-DD>/`. The library copy is the record. The owner's original stays where the owner put it.
3. Index it in the workstream's package file, `<notes>/library/<workstream>-package.md`: the path, what the file is, the spine (section order and key claims), every stat with its source and status, and the language that must not drift. Template: `examples/record-index.md`.
4. State precedence in that index: drafts defer to the record. Where a draft and the record disagree, the record wins.
5. Comment the record's location and the precedence rule on every in-flight card whose output overlaps it.
6. Save a one-line memory entry that names the record file and its index location.
7. Flag any stat in the record that is under fact-check, so new drafts do not repeat it.

### Versions

1. The owner edits their own file. When the owner says they updated it, or the Friday routine finds a difference, copy the file into a new dated folder beside the current one: `assets/<name>/<YYYY-MM-DD>/`. A second copy on the same day takes `-2`.
2. Rebuild the record index in the same pass, and mark the previous version's index line superseded.
3. Old versions stay. Supersede, never edit: nothing a recipient holds is edited in place.
4. Every derived asset's index row names the record version it was built from.

A wrong fact in a record follows the correction procedure in `SKILL.md`.

## The collateral flow

Conversations with the owner are where materials arrive. When the owner drops a material in chat (deck, PDF, design link, one-pager, screenshot), or decides anything about an outside-facing piece, the chief files it in the same pass. Do not wait for a package request.

1. **Materials.** File the material under `<notes>/library/assets/<name>/` and add a row to `<notes>/library/index.md` the same day.
2. **Package decisions.** When the owner decides what an audience package contains, update `<notes>/library/<audience>-package.md` in the same session: the audience, the pieces, their owners, and the claim limits.
3. **Approved output.** When the reviewer approves outside-facing work, the chief copies it into the library and adds its index row in the same pass. Specialists never write to the notes folder.
4. **Packages are built from the record.** Every piece in a package uses the record's stats and fixed language. Only the framing changes per audience.
5. **Monitoring.** The Friday routine reviews the library index. Any row not marked Parsed or Approved that has sat two weeks gets a card or a Dormant mark, listed in a Collateral section of next week's file.
