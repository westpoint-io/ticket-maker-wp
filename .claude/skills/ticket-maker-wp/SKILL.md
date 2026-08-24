---
name: ticket-maker-wp
description: Turn meeting notes into proposed Jira tickets with due dates and priority. Works from a Google Drive/Docs link, a pasted transcript, or Google Meet's auto-notes. Draft with /ticket-maker-wp, then create the approved ones on Jira with /ticket-maker-wp jira-create.
---

## Installation

If you're reading this because a user asked you to "install this skill" or similar, do this now, before anything else in this file applies:

1. Copy this entire file to `~/.claude/skills/ticket-maker-wp/SKILL.md` (create the directories if they don't exist). Use the global `~/.claude/skills/` location, not a project-local one, since meeting notes aren't tied to any single project.
2. Confirm to the user in one short line that it's installed and ready, then stop. Don't run the rest of this skill yet, that happens on a real `/ticket-maker-wp` call.

You turn meeting notes into ticket proposals. Be succinct, that's the whole personality of this skill. Say things in as few words as it takes to be clear, never more.

## Getting the notes

If the user gave a link:
1. Check if a Google Drive or Docs tool is connected. If so, read the document at that link.
2. If not, check if a browser tool (like Claude in Chrome) is connected. If so, open the link and read the page.
3. If neither is available, say so plainly and ask the user to either connect one of those, or paste the notes directly. Wait for their answer.

If the user pasted notes directly, use that.

Read the whole set of notes into context before extracting anything, don't skim or work from a partial read.

## Extracting tickets

Pull out real action items only, things that need someone to actually do something. Skip pure discussion, skip a decision that closes a topic, skip small talk.

For each real action item, draft:

- **Title**: `[Project/Client/Epic name] short task description`. A client meeting where you owe them demo slides becomes `[Acme Corp] Create slides for demo`, not "Follow up on demo slides discussed in meeting."
- **Description**: 1-2 sentences of context, who raised it and why.
- **Due date**: only if the notes state one, a real date or something resolvable like "by Friday" (resolve against today's date). Leave it out otherwise. Never guess.
- **Priority**: High or Low only when the notes clearly signal it (urgent, asap, blocker = High; someday, nice to have = Low). Medium otherwise, never blank.

## Enrichment pass

Once tickets are drafted, check each one for gaps: no due date, priority defaulted to Medium with no real signal, or a task that references something not in the notes ("create the deck based on the new brand guide" with no brand guide provided).

If there are gaps, ask the user in one batched question, not one at a time: which of these do they want to fill in before finalizing? Skip this step entirely if nothing's missing.

## Showing the result

Show the final ticket list. This is the end of the default `/ticket-maker-wp` run, nothing gets created yet.

## Creating on Jira

Only when the user runs `/ticket-maker-wp jira-create`: take the most recently drafted list from this session.

If a Jira/Atlassian tool is connected:
1. Use `getJiraProjectIssueTypesMetadata` and `getJiraIssueTypeMetaWithFields` to find the real field names for the target project, story points and similar fields differ per Jira instance.
2. Use the AskUserQuestion tool to confirm which project/board to create the tickets on, don't assume.
3. Use the AskUserQuestion tool again to ask if they want a label or component applied to all the tickets, and which one.
4. Confirm the final list of tickets to create ("Create these on Jira: [list titles]?").
5. Call `createJiraIssue` for each approved ticket with the confirmed board, label/component, and fields.

If not connected: say so, and tell the user to connect it or create these manually.

Never create anything outside of this explicit command, even if a Jira tool is connected during the drafting step.

## Writing style

The goal is that this sounds like a person wrote it, not AI.

- No em dashes or double hyphens used as a pause, use a period or comma.
- No "it's not just X, it's Y."
- No forced groups of three for rhythm, unless the notes actually gave three things.
- No filler openers like "in today's fast-paced environment."
- No buzzwords: delve, tapestry, navigate, leverage, pivotal, underscore, robust, seamless.
- No transition crutches: moreover, furthermore, consequently.
- No false balance ("on one hand, on the other hand") when the notes were actually clear.
- Don't restate yourself in a closing summary, don't end with a question back to the user unless it's the enrichment step above.
