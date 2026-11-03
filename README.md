# Agent chief of staff

The operator layer for a one-person function run with AI agents. One agent, the chief of staff (the chief), takes every request from the owner, routes the work to specialist agents on a kanban board, files the results, and brings the owner only the decisions that need them.

The system has these parts:

1. One pinned session. The owner talks to the chief there and nowhere else.
2. One inbox. Every request becomes one line.
3. A live day list (`TODAY.md`). The owner reads it to see what is done, what is ready, and what to do next.
4. Weekly files. They are the to-do list of record.
5. Kanban routing. The chief turns accepted work into cards for a writer, a reviewer, and an analyst.
6. Persistent memory. Durable facts go to agent memory; corrections to procedure go into the skill that governs the work.
7. A daily cadence. A morning brief, a dispatcher three times a day, a close-out sweep, and two Friday jobs.
8. A writing pipeline. The writer drafts, the reviewer checks, and the owner edits.
9. One pickup folder. Approved work lands there, one file per task.

The rule that holds it together: the agents never send, book, post, or contact anyone for the owner. The owner edits and sends everything.

## Companion repo

[agent-intake-system](https://github.com/andyhyfi/agent-intake-system) holds the procedure skills for one agent: intake and routing, files of record, the collateral library, and follow-through. This repo holds the orchestrator that routes work to specialist agents. Each repo works on its own. You can install this repo without the other one; the procedures the chief needs for intake and files of record are in `skills/chief-of-staff/references/new-work.md`.

## Contents

```
skills/chief-of-staff/
  SKILL.md                 the chief's rules
  references/new-work.md   how a request enters and gets filed
  references/routines.md   the scheduled step sequences
examples/
  today.md                 the live day list
  weekly-file.md           one week's to-do list of record
  inbox.md                 the inbox line format
  sent-log.md              the sent log format
  workflow-registry-entry.md
  record-index.md          the index for a file of record
```

## Requirements

1. Hermes Agent, with the gateway running as a service so scheduled jobs fire with no terminal open.
2. A kanban board (the Hermes kanban board).
3. Four agent profiles: `default` (the chief), `writer`, `reviewer`, `analyst`.
4. A notes folder the owner reads, for example a plain folder of markdown files.
5. A writing ruleset skill and an anti-slop skill for the writer and reviewer. Name them anything. The review checklist in `SKILL.md` is the minimum they must cover.

## Folder layout

Every path in the skill uses these placeholders. Replace them once at install.

- `<notes>`: the notes folder root. Only the chief writes here.
- `<hermes>`: the Hermes home, usually `~/.hermes`.

```
<notes>/
  chief-of-staff/
    inbox.md                 one line per request
    TODAY.md                 the live day list
    workflow-registry.md     one entry per workflow
    sent-log.md              one line per send the owner reports
    ready-for-owner/         the pickup folder: approved work, one file per task
    archive/ready-for-owner/ files the close-out sweep moved out of the pickup folder
  weekly/<YYYY>-W<NN>.md     one file per ISO week
  content-index.md           one row per approved piece the chief filed
  library/
    index.md                 one row per collateral asset
    assets/<name>/<YYYY-MM-DD>/   dated copies of each file of record
    <workstream>-package.md  the record index for one workstream
    <audience>-package.md    the audience package for one audience
  analytics/                 CSV files, the analytics record
```

## Setup

1. Install the skill. Copy `skills/chief-of-staff/` into `<hermes>/skills/productivity/chief-of-staff/`.
2. Replace `<notes>` in the skill files with your notes folder path.
3. Create the specialist profiles. Run these for `writer`, `reviewer`, and `analyst`:

```bash
hermes profile create <name> --no-skills --no-alias --description "<one-line role>"
hermes -p <name> config set model.default <model>
hermes profile describe <name> --text "<one-line role>"
```

4. Give each specialist the skill library. Symlink the skill folders it needs into `<hermes>/profiles/<name>/skills/`, then check with `hermes -p <name> skills list`. Setting `skills.external_dirs` alone did not deliver the skills in production.
5. Write `<hermes>/profiles/<name>/SOUL.md` for each specialist. It states the role, that the profile reports to the chief, that it never writes to `<notes>`, that nothing leaves the building, that it never drives the owner's own browser, and how it finishes a kanban card. The writer finishes with `kanban_request_review(reviewer="reviewer")`.
6. Smoke-test each profile: `hermes -p <name> chat -Q -q "Reply with only: OK"`.
7. Set the board so the chief routes work: `kanban.auto_decompose: false`, `kanban.orchestrator_profile: default`, `kanban.default_assignee: default`.
8. Start the gateway as a service: `hermes gateway install --start-now --start-on-login`, then `hermes gateway status`.
9. Create the scheduled jobs on the `default` profile with `hermes cron create`. Schedules are in `references/routines.md`. Each prompt is self-contained: it names the absolute paths, says to run `date` first, and names the output.
10. Copy the templates from `examples/` into `<notes>`.
11. Pin one chief session in your client. That session is the owner's entry point.

## Origin

This is the public, generalized version of a production system I have run daily since December 2025 behind IP and legal gates. Company data, partner names, and deal terms are removed. The structure is the one in production; the specifics are placeholders.

## License

MIT; see LICENSE.
