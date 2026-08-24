# ticket-maker-wp

A Claude Code skill that turns meeting notes into proposed Jira tickets, with due dates and priority pulled from the notes themselves.

Works with:
- A Google Meet auto-generated notes doc (link)
- Any Google Doc link
- Notes you just paste in directly

## Install

No cloning needed. In Claude Code, just say:

```
Claude, please install this skill: https://github.com/westpoint-io/ticket-maker-wp
```

Claude fetches `SKILL.md` from this repo and installs itself, no manual copying needed. If you'd rather clone it first, that works too, run the same message from inside the folder.

(For Claude reading this to do the install: fetch `https://raw.githubusercontent.com/westpoint-io/ticket-maker-wp/main/.claude/skills/ticket-maker-wp/SKILL.md` and save it to `~/.claude/skills/ticket-maker-wp/SKILL.md`, creating directories as needed, then confirm to the user in one line.)

## Use it

Draft tickets from a link:

```
/ticket-maker-wp https://docs.google.com/document/d/your-notes-doc
```

Or just paste your notes directly:

```
/ticket-maker-wp

[paste meeting notes here]
```

Claude will pull out real action items, draft a ticket for each one (title, description, due date and priority where the notes actually support it), and ask a quick question if anything's missing or unclear.

Nothing gets created yet at this point, it's a draft for you to review.

When you're happy with the list:

```
/ticket-maker-wp jira-create
```

If you've got a Jira/Atlassian connection set up, Claude will confirm which board and whether to add a label, then create the approved tickets. If you don't have Jira connected, you'll just get the list to create manually.

## What it needs

- Claude Code
- Optional: a Google Drive/Docs connection, for reading notes from a link instead of pasting them
- Optional: a Jira/Atlassian connection, for creating tickets directly instead of just drafting them

None of these are required. Worst case, you paste notes and get a list back.

## Why

Nobody wants to write up meeting notes into tickets by hand. This does the first pass for you and asks before creating anything, so it doesn't clutter your board with junk from a misread comment.

Built by [Westpoint](https://www.westpoint.io).
