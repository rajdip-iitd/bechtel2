# Using Claude Effectively: A Field Guide for Bechtel Engineers

*IIT Delhi x Bechtel AI/ML Programme · June 2026*

---

## Why this guide exists

Claude is not a search engine. It does not give better answers just because you ask louder or more politely. It gives better answers when you give it **context, constraints, and a clear goal** before asking it to do anything.

This guide shows you three tools that do exactly that:

1. **CLAUDE.md** — a memory file that tells Claude who you are and how you work
2. **Skills** — saved instruction sets for tasks you repeat often
3. **MCP servers** — live connections to real tools (Gmail, Drive, Calendar)

Every example below is written for the kind of work Bechtel engineers actually do: project cost reviews, schedule tracking, document analysis, site safety reporting.

---

## Part 1: CLAUDE.md — Give Claude a briefing before every project

Think of CLAUDE.md as a one-page project brief you hand to a new engineer on their first day. Without it, they ask obvious questions and make obvious mistakes. With it, they hit the ground running.

You create this file once per project. Claude reads it automatically every time you open that project.

### How to create it

In Claude Code, run:

```
/init
```

This auto-generates a starting file. Then edit it to match your project.

### A template for Bechtel use

```markdown
# CLAUDE.md

## Who I am
I am a project engineer / cost manager / planner at Bechtel.
This project is about [e.g. reviewing tender documents for the Noida Airport terminal package].

## What I need help with
- Summarising long contract documents
- Drafting RFI (Request for Information) responses
- Flagging schedule risks in project status reports
- Writing clear emails to subcontractors

## My preferences
- Use plain English. No jargon unless I ask.
- Keep responses short. I will ask follow-up questions if I need more.
- If you are unsure, say so. Do not guess at cost figures or contract clauses.
- Always confirm before making changes to any document.

## What NOT to do
- Do not invent tender prices, contract dates, or project numbers.
- Do not rewrite documents I have not asked you to touch.
- Do not give legal or financial advice — flag that I need a specialist.

## Current focus
Right now I am working on: [describe the specific task]
Out of scope for now: [anything you want Claude to ignore]
```

**The rule:** specific instructions work far better than vague wishes.

| Weak (Claude guesses) | Strong (Claude acts) |
|---|---|
| "Be professional" | "Write at the level of a formal engineering letter" |
| "Keep it short" | "Maximum 5 sentences per response" |
| "Be careful with numbers" | "Do not state any cost figure without flagging it as an estimate" |

---

## Part 2: Skills — Save your most useful workflows

A skill is a saved set of instructions for a task you do over and over. Instead of typing the same long prompt every week, you write it once, save it as a skill file, and call it with one command.

### What Bechtel engineers repeat all the time

- Weekly progress report drafts
- Site safety incident summaries
- Subcontractor email follow-ups
- Risk register updates
- Meeting minutes from a raw transcript

These are exactly the right use cases for skills.

### How to create a skill

Create a folder and a file:

```
.claude/skills/progress-report/SKILL.md
```

Inside the file, write plain instructions (no coding required):

```markdown
---
name: progress-report
description: Draft a weekly project progress report from raw notes.
---

Read the notes I give you. Then write a structured progress report with these sections:

1. Work completed this week (bullet points, one per activity)
2. Work planned for next week
3. Issues and risks (flag anything that could affect cost or schedule)
4. Actions required (who, what, by when)

Rules:
- Use formal engineering report language.
- Do not add information I have not provided.
- If a section has no content, write "Nothing to report."
- Keep the whole report under one page.
```

To use it, just type:

```
/progress-report
```

Then paste in your raw notes. Claude produces a clean, consistent report every time, without you having to re-explain the format.

### More skill ideas for EPC work

| Skill name | What it does |
|---|---|
| `rfi-draft` | Drafts a formal RFI from a brief description of the issue |
| `risk-flag` | Reads a status update and pulls out schedule or cost risks |
| `meeting-minutes` | Turns a rough transcript into structured minutes with action items |
| `site-safety-summary` | Summarises an incident report into the standard Bechtel format |
| `email-subcontractor` | Writes a firm but professional follow-up email from bullet points |

---

## Part 3: MCP Servers — Connect Claude to your real tools

MCP (Model Context Protocol) is the standard that lets Claude connect to live external tools. Instead of you copying and pasting content into Claude, Claude reads the source directly.

For this programme, your Claude account is connected to three MCP servers:

| Tool | What Claude can do |
|---|---|
| **Gmail** | Read, search, and draft emails in your inbox |
| **Google Drive** | Read documents, spreadsheets, and files in your Drive |
| **Google Calendar** | Check your schedule, find meeting times, read event details |

### Practical examples

**"Summarise the last 5 emails about the concrete pour schedule"**
Claude searches your Gmail, reads the thread, and gives you a one-paragraph summary with any open action items flagged.

**"Find the latest risk register in my Drive and list all High risks"**
Claude searches your Google Drive, opens the file, and extracts every row where the risk level is marked High.

**"What does my week look like? I need a two-hour block for a site review call"**
Claude reads your Google Calendar and suggests open slots, accounting for existing meetings.

**"Draft a reply to the last email from [subcontractor name] about the delay claim"**
Claude finds the email, reads the context, and drafts a professional response for your review.

### Important: Claude does not act without your confirmation

When using MCP tools, Claude will show you what it found and what it plans to do. It will not send emails or delete files automatically. You always approve before anything is sent or changed.

---

## Part 4: The habit that matters most

Before asking Claude to do anything complex, take 30 seconds and answer these three questions in your prompt:

1. **What is the context?** (Which project? Which document? What stage are we at?)
2. **What do you want?** (A summary? A draft? A list of risks? A reply?)
3. **What are the constraints?** (Length, tone, what to include, what to avoid)

A prompt without context forces Claude to guess. A prompt with context produces work you can actually use.

**Without context:**
> "Summarise this document."

**With context:**
> "This is a 40-page subcontractor programme submitted for the civil works package on the Noida Airport project. Summarise it in bullet points. Focus on milestone dates, any float mentioned, and any risks to the concrete works. Keep it under 10 bullets."

The second prompt takes 20 extra seconds to write. The output saves you 20 minutes of reading.

---

## Quick reference card

| Tool | When to use it | How to start |
|---|---|---|
| **CLAUDE.md** | At the start of any project or recurring task | Run `/init` in Claude Code, then edit the file |
| **Skills** | For any task you do more than twice a week | Create `.claude/skills/your-skill/SKILL.md` |
| **MCP: Gmail** | Searching or drafting emails | Just describe the task in natural language |
| **MCP: Drive** | Reading or summarising documents | Just describe what file or topic you need |
| **MCP: Calendar** | Checking schedule or finding meeting slots | Just ask directly |

---

## Further reading

- Claude Code documentation: [docs.claude.ai](https://docs.claude.ai)
- Prompt engineering guide: [docs.claude.ai/en/docs/build-with-claude/prompt-engineering/overview](https://docs.claude.ai/en/docs/build-with-claude/prompt-engineering/overview)
- IIT Delhi x Bechtel programme materials: shared via Google Drive (see programme coordinator)

---

*Prepared for the IIT Delhi x Bechtel AI/ML Training Programme · Prof. Rajdip Nayek, Department of Applied Mechanics, IIT Delhi*
