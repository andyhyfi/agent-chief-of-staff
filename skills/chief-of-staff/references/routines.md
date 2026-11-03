# Routines

Each routine is a Hermes cron job. Run `date` first in every routine and work against the actual day. Times are local defaults; the owner can change them.

| Routine | When | Cron expression | Runs on |
|---|---|---|---|
| Morning brief | Weekdays 8:45 AM | `45 8 * * 1-5` | default (the chief) |
| Dispatcher | Weekdays 9:00 AM, 1:00 PM, 4:00 PM | `0 9,13,16 * * 1-5` | default (the chief) |
| Close-out sweep | Weekdays 5:30 PM | `30 17 * * 1-5` | default (the chief) |
| Analytics pull | Fridays 9:00 AM | `0 9 * * 5` | analyst, then the chief |
| Next week's file | Fridays 4:00 PM | `0 16 * * 5` | default (the chief) |

Create each job with `hermes -p <profile> cron create "<cron expression>" "<prompt>" --name "<name>" --deliver local --skill chief-of-staff`. The prompt is self-contained: it names the absolute paths, says to run `date` first, and names the output.

## Morning brief (weekdays 8:45 AM)

1. Read the current weekly file, `inbox.md`, and the kanban board (blocked cards and cards in review).
2. Run `session_search` for yesterday's sessions (newest first, limit 10). Pick up status changes the files lack.
3. File the inbox lines (placement rules in `new-work.md`).
4. Write `TODAY.md` for the day from the weekly file.
5. Output to the owner: the one task to do first and why, then a flat numbered list of today's work with file paths, then "Needs you:" (approvals, blocked cards, decisions). Keep it short.

## Dispatcher (weekdays 9:00 AM, 1:00 PM, 4:00 PM)

1. Read the current weekly file and the inbox.
2. Create a kanban card for every item a specialist can advance that has no moving card. Skip items the owner owns.
3. Copy each reviewer-approved attachment into `ready-for-owner/` and link it in `TODAY.md` under "Ready for your review".

## Close-out sweep (weekdays 5:30 PM)

1. Run `session_search` for every session since the morning (newest first, limit 15). Also search the owner's messages for the words done, killed, approved, and sent.
2. Record every request, decision, and completion the owner stated in any session. File them in the weekly file and the inbox. Tag each with its source session title.
3. Mark each `TODAY.md` item done or carried. Write carried items under "Tomorrow".
4. Sweep the pickup folder: move any file in `ready-for-owner/` that today's `TODAY.md` does not link to `archive/ready-for-owner/` (run `mkdir -p` first). The day-list links are the live set; the folder holds nothing else. Name the moves in one output line.
5. Output to the owner: what closed, what carried, anything stuck over two days, and new requests found in sessions.

## Friday

### Analytics pull (9:00 AM)

1. The analyst pulls the week's numbers from each channel into one record: the CSV files in `<notes>/analytics/`. That folder is the system of record for analytics.
2. The chief reviews the numbers and drafts the growth report.

### Next week's file (4:00 PM)

1. Draft next week's file, `<notes>/weekly/<YYYY>-W<NN>.md`, from this week's carry-forward section, the registry review notes, and the inbox.
2. Add a Collateral section from the library index: each row stalled two weeks gets a card or a Dormant mark.
3. Add the end-of-week archive check: list every file in `archive/ready-for-owner/` last modified more than four weeks ago, one keep-or-purge line each.
4. The owner approves the draft before it becomes the active week. The owner's purges are deleted. The owner's keeps move back into the notes folder where they belong.
