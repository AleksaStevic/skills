---
name: writing-tickets
description: Write or update the content of an issue/ticket in Linear, Jira, GitHub Issues or any other tracker. Use when asked to create a new ticket, draft a ticket body, or update an existing one (e.g. "napravi Linear tiket za ovo", "make a ticket for this bug", "update WFA-123 with what we found").
---

Governs **what goes inside a ticket**, not how it gets posted. Tracker-agnostic: create the
issue with whatever tool is at hand (Linear MCP, `gh issue`, or printing the markdown for the
user to paste) — the body always follows this standard.

Two variants: **Bug** and **Task**. One per ticket, never merged.

## Steps

1. Pick the variant.
2. Gather facts from the code, git, logs, or the conversation.
3. Draft title + body from the template below. Delete every `<!-- -->` hint.
4. Show the draft to the user before posting or saving anything.

## Bug template

```markdown
## Context
<!-- One or two concise paragraphs: what breaks, where, for whom, and in one sentence why. -->

## STR
<!-- Numbered steps. Only steps actually confirmed; otherwise TODO: repro unknown. -->

## ER
<!-- Expected result. -->

## AR
<!-- Actual result. -->

## More info
<!-- Optional: cause, impact, links, screenshots. Delete the whole section if empty. -->
```

## Task template

```markdown
## Context
<!-- One or two concise paragraphs: what is missing or weak today, and what this task is. -->

## AC
<!-- Acceptance criteria as a checkbox bullet list. One checkable deliverable per line. -->

## More info
<!-- Optional: out of scope, links, pointers. Delete the whole section if empty. -->
```

## Style

- Context is one or two concise paragraphs of plain prose. Every other section is bullets.
- One fact per bullet. Short full sentence, one line.
- No bold labels, no arrow chains (`56s → 0.3s`), no stacked parentheticals.
- Almost no metrics. At most one approximate figure when it carries the point.
- No commit SHAs in prose — links get their own bullet: `[Commit](url)`.
- Why in one sentence: causality for the reader, not evidence.
- No trade-offs, alternatives, or decision framing. Facts and the fix.
- Concepts over implementation minutiae.
- English always, whatever language the conversation is in.
- Body fits on one screen. Audits, measurement dumps, file inventories go in a ticket comment
  or a linked doc, referenced from one bullet.
- Delete any section that does not apply. Never pad it with filler.

## Title

Sentence fragment stating the change or the symptom. No type prefix (`[BUG]`, `fix:`), no
ticket key, no component brackets — the tracker's labels carry that. ≤ 80 characters.

- Yes: `Daily report shows yesterday's totals for users in UTC+13`
- No: `[BUG] Report bug`, `WFA-123: report`, `Fix the thing in reports`

## Missing facts

Ask the user once for the gaps that block the ticket (repro steps, environment, who is
affected). Leave `TODO:` markers for the rest. Never invent a repro, a metric, a cause, or an
affected version — a visible `TODO:` beats a plausible fabrication.

## Updating an existing ticket

Surgical by default: touch only the sections the user asked about, leave the rest of the body
byte-identical. Someone else (PM, QA, support) may have written it.

If the body does not follow this template, make the requested edit, then offer in one line:
"body doesn't follow the template — want me to reshape it?"

## Example (Bug)

Title: `Daily report shows yesterday's totals for users in UTC+13`

```markdown
## Context
Employees in New Zealand see the previous day's numbers in the daily report all morning. It hits
every employee more than a few hours ahead of UTC, every day.

The date range is built from the server clock, not the employee's timezone. At UTC+13 the server
is still on the previous calendar day for most of the local morning.

## STR
1. Set an employee's timezone to Pacific/Auckland.
2. Open the daily report before noon local time.

## ER
- The report shows the employee's local day, matching the date in the header.

## AR
- The header says today's date, the totals belong to yesterday.

## More info
- The range helper takes no timezone argument, so every caller gets the server day.
- [Report screenshot](https://example.com/screenshot)
```
