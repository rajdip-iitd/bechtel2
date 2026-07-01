# AI Instruction Files: A Practical Guide to Rules, Skills, and Workflows

*How to use small Markdown files to give AI assistants reliable project context—without creating a confusing pile of instructions.*

**June 30, 2026**

---

AI assistants can write code, review documents, analyse data, and prepare reports. But they work much better when they are given the context that a careful new team member would need:

- what the project is trying to achieve;
- what must not be changed;
- which commands to run;
- how quality is checked;
- which standards, units, naming conventions, and review steps matter.

A short Markdown file can hold this context in a form that people can read, edit, review in Git, and reuse across tasks.

The purpose is not to “control how the model thinks.” It is to give the model clear, relevant constraints and working conventions before it begins work.

A prompt is a request for one task.

An instruction file is reusable context for many related tasks.

For an engineering team, that can mean fewer repeated explanations and more consistent outcomes. For example, instead of repeating:

> “Use SI units, do not overwrite raw sensor data, document assumptions, and run tests before changing a validated calculation.”

you can record those expectations once in a project instruction file.

## The first principle: use the smallest useful instruction set

More instructions are not automatically better.

A long file full of generic advice can distract an agent, conflict with other instructions, and increase the time it spends interpreting context rather than completing the task. Keep instruction files focused on rules that are:

1. **specific to your team or project;**
2. **important enough that an AI may otherwise make a costly mistake;**
3. **not already enforced by a formatter, linter, test suite, access control, or CI pipeline.**

Avoid vague statements such as:

- Write clean code.
- Follow best practices.
- Be careful.
- Produce high-quality work.

Prefer specific, verifiable rules:

- Run `pytest` and `ruff check .` before marking a Python change complete.
- Use SI units internally; convert units only at input and reporting boundaries.
- Do not overwrite files in `raw_data/`.
- Do not modify validated baseline models without explicit approval.
- Add a unit test when changing a calculation utility.
- State assumptions and operating-range limitations in every generated analysis report.

Instruction files improve guidance. They are **not** a security boundary or a substitute for permissions, code review, tests, protected branches, or data-access controls.

---

# 1. A simple instruction stack

The useful idea is **layering**. Broad rules belong near the top of a project; narrow rules belong close to the work that needs them.

```text
Organisation or programme policy
        ↓
Repository-wide project rules
        ↓
Tool-specific instructions
        ↓
Folder- or component-specific rules
        ↓
Reusable task workflow or skill
        ↓
Current task prompt
```

The closer an instruction is to the task, the more specific it should be.

A local rule should refine the broader project rule, not contradict it.

For example:

- A repository-wide rule may say: “Use SI units internally.”
- A `fatigue/` folder rule may add: “Use stress range in MPa only at report output; retain Pa internally.”
- A fatigue-reporting skill may add the exact verification checklist for a fatigue analysis.

---

# 2. Which files are real conventions—and which are team choices?

Not every Markdown filename has the same status.

Some are recognised by specific AI tools. Others are useful names that your team can adopt, but an AI assistant will only use them if the relevant tool is configured to read them or you explicitly ask the assistant to do so.

## AI instruction files at a glance

| File or pattern | What it does | Use it when your project needs... | Important note |
|---|---|---|---|
| `AGENTS.md` | Repository-level guidance for AI agents | Shared rules for build, test, architecture, documentation, safety, or review | An open cross-tool convention; check whether your chosen agent discovers it automatically. |
| `CLAUDE.md` | Persistent Claude Code project instructions | Claude Code-specific project context, commands, conventions, and boundaries | A Claude Code convention. |
| `.claude/rules/` | Scoped Claude Code rules | Different instructions for selected paths or file types | Use this rather than inventing a generic local filename when working specifically in Claude Code. |
| `SKILL.md` | A reusable task playbook | A repeated workflow such as code review, report generation, data validation, or document conversion | Usually belongs inside a skill folder and may include scripts, templates, and references. |
| `DESIGN.md` | Team-defined design-system guidance | Consistent UI terminology, components, typography, layouts, accessibility, and interaction patterns | Useful convention; not automatically universal. |
| `WORKFLOW.md` | Team-defined process guidance | A task must follow a recognised process from intake to review and handoff | Useful for human and AI contributors alike. |
| `decisions.md` or `ADR/` | Durable technical decisions | The team needs an auditable record of why a technical choice was made | Prefer this to expecting an AI “memory” file to preserve shared history. |
| `.prompt.md` | A reusable task starting point | You repeatedly launch a structured task with the same inputs and outputs | A team convention unless your tool has explicit prompt-file support. |
| `style-guide.md` | Writing or communication guardrail | Reports, emails, specifications, presentations, or documentation require a consistent voice | Keep it separate from code rules so it is loaded only for writing tasks. |
| `.agent.md` | Role brief for a specialist assistant | You want a defined review role, such as test engineer, cybersecurity reviewer, or data-quality analyst | Treat it as a team convention unless supported by your chosen platform. |

> **Important:** A file being present in a repository does not guarantee that every AI tool will read it. Before depending on a convention, verify how your tool discovers and applies instruction files.

---

# 3. What belongs in each layer?

## `AGENTS.md`: rules that should apply to most work in the repository

Use `AGENTS.md` for compact repository-wide guidance. It is the right place for information that a coding assistant should know on most tasks:

- project purpose and important directories;
- setup, test, build, and lint commands;
- coding and documentation conventions;
- boundaries around sensitive files or data;
- completion criteria;
- links to more detailed instructions.

Do **not** turn `AGENTS.md` into an encyclopedia. If a section grows into a multi-step procedure, move that procedure into a reusable skill or workflow file.

### Example: engineering analytics project

```markdown
# AGENTS.md

## Project purpose
This repository supports model-based engineering analytics and reporting.

## Non-negotiable rules
- Use SI units internally.
- Never overwrite files under `raw_data/`.
- Do not modify files under `validated_models/` without explicit approval.
- Do not expose credentials, client identifiers, or confidential data in code, logs, or generated reports.
- Record assumptions and limitations in every analysis report.

## Required checks
- Run `pytest` before finalising a code change.
- Run `ruff check .` and the type checker for Python changes.
- Add or update a unit test when calculation logic changes.

## Key locations
- `src/`: application and analysis code
- `tests/`: automated tests
- `docs/`: user-facing documentation
- `raw_data/`: immutable input data
- `validated_models/`: controlled baseline models

## More specific guidance
- For fatigue analysis, use `skills/fatigue-review/SKILL.md`.
- For report writing, use `docs/report-style-guide.md`.
```

This file tells the assistant what matters without explaining every possible engineering method.

---

## `CLAUDE.md`: instructions for Claude Code

Use `CLAUDE.md` when Claude Code is part of your workflow. It is a place for instructions you want Claude Code to retain while operating in the project.

It can include:

- how to initialise the project;
- common commands;
- local architecture conventions;
- paths that should be treated cautiously;
- a concise definition of “done.”

Do not duplicate `AGENTS.md` word-for-word. A practical approach is:

- keep broadly useful repository rules in `AGENTS.md`;
- place Claude Code-specific guidance in `CLAUDE.md`;

### Example

```markdown
# CLAUDE.md

Before editing code, read AGENTS.md.

## Working approach
1. Inspect the relevant code and tests before proposing a change.
2. Make the smallest safe change that satisfies the request.
3. Run the checks specified in AGENTS.md.
4. Summarise modified files, validation performed, assumptions, and remaining risks.

## Do not
- rewrite validated numerical methods unless the task explicitly requests it;
- change public API behaviour without updating tests and documentation;
- treat an instruction file as permission to access data that you otherwise cannot access.
```

---

## Scoped rules: use them only when local work genuinely differs

A project may contain very different kinds of work:

- a Python calculation engine;
- API services;
- infrastructure configuration;
- confidential-data processing;
- report templates.

Do not force all their rules into one global file.

Instead, place narrow instructions close to the relevant area. In Claude Code, scoped rules can be placed under `.claude/rules/` for path-specific or file-type-specific guidance. Other tools may use different mechanisms.

### Example: calculation-engine rule

```markdown
# Calculation-engine rules

Applies to: `src/calculations/**`

- Preserve numerical precision until final presentation.
- Use SI units at all internal interfaces.
- Add a regression test for each corrected numerical defect.
- Do not replace a validated physical model with a black-box approximation
  unless the task explicitly requests a model-form change.
- Record the valid operating range in function documentation.
```

Use local rules when they reduce a real risk. Do not create a separate instruction file merely because a folder exists.

---

# 4. Skills: use a `SKILL.md` file for repeated workflows

A skill is a reusable bundle for a specific kind of work. The bundle usually contains a `SKILL.md` playbook plus optional templates, examples, scripts, or reference files.

A skill is useful when you repeatedly ask for a task that has a stable procedure.

Examples:

- reviewing a certain request;
- checking a calculation notebook;
- generating a model-validation report;
- preparing a slide deck having a specific structure;
- extracting risks from site reports;
- converting test data into a standard engineering report.

A skill is **not** the same as a permanent repository rule.

Use:

- `AGENTS.md` for “always do this in this repository”;
- `SKILL.md` for “follow this workflow whenever this kind of task is requested.”

## Example skill layout

```text
skills/
└── engineering-review/
    ├── SKILL.md
    ├── checklist.md
    └── templates/
        └── review-summary.md
```

### Example `SKILL.md`

```markdown
---
name: engineering-review
description: Review engineering-analysis code, notebooks, and reports for
units, assumptions, validation evidence, reproducibility, and communication
of limitations. Use when asked to review an analysis, model, calculation,
notebook, or engineering report.
---

# Engineering Review Skill

## Objective
Identify technical, reproducibility, and communication risks before a result is shared.

## Review procedure
1. Identify the stated problem, inputs, outputs, and intended operating range.
2. Check units, assumptions, boundary conditions, and sign conventions.
3. Check whether raw data are preserved and transformations are traceable.
4. Review tests, validation evidence, and uncertainty treatment.
5. Separate confirmed defects from questions requiring domain-owner confirmation.
6. Produce a concise review using `templates/review-summary.md`.

## Required output sections
- Scope reviewed
- Confirmed issues
- Assumptions requiring confirmation
- Validation evidence
- Risks and limitations
- Recommended next actions
```

The `description` is important because it helps an agent recognise when the skill is relevant.

---

# 5. Workflow files: define the process, not every sentence

Use a `WORKFLOW.md` file when a task must proceed through a known sequence.

Examples include:

- data intake → validation → analysis → review → approval → delivery;
- feature request → implementation → test → peer review → release;
- incident report → triage → containment → root-cause analysis → closure.

A workflow file is especially valuable where auditability, or approval stages matter.

### Example: analysis delivery workflow

```markdown
# WORKFLOW.md

## Analysis delivery workflow

1. Confirm the question, data source, intended decision, and required deadline.
2. Record assumptions, operating range, and exclusions.
3. Preserve raw inputs; create derived data in a separate location.
4. Perform the analysis and retain reproducible scripts or notebooks.
5. Run required validation checks.
6. Prepare a report that states results, uncertainty, limitations, and next steps.
7. Obtain technical review before external release.
8. Archive the final report, source data reference, and review record.
```

The workflow should be short enough to use. Detailed process documents can sit elsewhere and be linked from this file.

---

# 6. Decision records are better than generic “memory” files

AI tools may have their own private memory features. Those features are tool-specific and may be local to one user, one machine, or one workspace.

Do not rely on an AI assistant’s memory as the shared record of your team’s decisions.

For decisions that should outlive a chat or be visible to the whole team, use a version-controlled document such as:

```text
docs/
├── decisions.md
└── adr/
    ├── 001-use-si-units-internal.md
    ├── 002-raw-data-are-immutable.md
    └── 003-validation-thresholds.md
```

### Example architecture decision record

```markdown
# ADR-002: Raw data are immutable

## Status
Accepted

## Decision
Files under `raw_data/` are treated as immutable. Derived datasets must be
written to `processed_data/` with a documented transformation script.

## Why
This preserves traceability, prevents accidental data corruption, and supports
reproducibility during review.

## Consequences
- Analysis code must not write to `raw_data/`.
- Each derived dataset must record its source and transformation version.
```

That is useful to humans, future team members, and AI assistants alike.

---

# 7. Design and writing guidance: load only when relevant

A `DESIGN.md` or `writing-style-guide.md` or `writng-standards.md` can be helpful, but it should not be forced into every coding task.

Use design guidance for:

- UI components;
- accessibility;
- visual hierarchy;
- typography;
- terminology;
- layout;
- approved component libraries;
- content design.

Use a writing guide for:

- reports;
- emails;
- specifications;
- proposals;
- slide text;
- executive summaries.

### Example writing-style guide

```markdown
# Report Writing Style Guide

- Use direct, plain language.
- State the engineering conclusion before detailed explanation.
- Distinguish evidence, assumptions, and recommendations.
- Avoid unsupported certainty.
- Prefer short paragraphs and informative headings.
- Define acronyms on first use.
- State limitations and operating range explicitly.
```

---

# 8. A practical starter setup

Do not create ten files on day one.

Start with the smallest setup that solves a real problem.

## For a software or engineering repository

```text
my-project/
├── AGENTS.md
├── CLAUDE.md                     # Only if your team uses Claude Code
├── docs/
│   ├── decisions.md
│   └── report-style-guide.md
├── skills/
│   └── engineering-review/
│       └── SKILL.md
└── tests/
```

## For a writing or knowledge-work project

```text
my-writing-project/
├── AGENTS.md
├── CLAUDE.md                     # Only if using Claude Code
├── docs/
│   └── writing-standards.md
└── prompts/
    └── write-article.prompt.md
```

Add further files only when you can name the recurring problem they solve.

---

# 9. How to adopt this 

## Step 1: Identify repeated time-taking process

Ask:

- What do we explain to AI assistants repeatedly?
- What mistakes would be expensive or embarrassing?
- What rules differ from standard practice?
- Which quality checks are regularly missed?
- Which repeated tasks need a fixed review process?

## Step 2: Put only stable rules in project instructions

Start with a 20–50 line `AGENTS.md`.

Include only the most important commands, boundaries, and completion criteria. You can use 

## Step 3: Move procedures into skills

When you find yourself pasting the same checklist into a chat, turn it into a skill.

## Step 4: Put decisions in version-controlled records

Do not write durable project decisions in chats but keep them in a record file.

## Step 5: Test the setup

Ask the assistant:

```text
Read the project instructions that apply to this directory.
Summarise:
1. the rules you will follow;
2. the commands you will run before completion;
3. the files or directories you must treat cautiously.
Do not modify anything yet.
```

If it gives the wrong summary, fix the instruction architecture before assigning a large task.

## Step 6: Review instruction files like code

Changes to a repository instruction file can change how every future AI session behaves.

Review them. Keep them concise. Remove obsolete rules. Resolve contradictions.

---

# 10. Common mistakes

## Mistake 1: One giant instruction file

**Why it fails:** Important rules become hard to find; unrelated instructions increase context load.

**Better approach:** Keep repository-wide rules short and move specialised procedures into skills or local rules.

## Mistake 2: Repeating the same rule in many files

**Why it fails:** Files drift out of sync.

**Better approach:** Define the rule once in the broadest appropriate layer, then reference it from narrower files.

## Mistake 3: Treating Markdown as enforcement

**Why it fails:** An instruction may be ignored or misunderstood. It cannot replace access control, testing, or review.

**Better approach:** Use Markdown instruction files for guidance and use technical controls like permissions, tests, review gates, for enforcement.

## Mistake 4: Storing private or sensitive information in broad context files

**Why it fails:** Project instructions are often read frequently and may be committed to source control.

**Better approach:** Keep secrets, personal data, credentials, and restricted client information out of instruction files. Reference approved secure systems instead.

## Mistake 5: Conflicting instructions

**Why it fails:** “Never alter the report” and “redesign the report” cannot both guide an agent safely.

**Better approach:** State precedence clearly and update old rules when a decision changes.

---

# 11. A compact decision rule

Use this decision rule when deciding where a new instruction belongs:

| Question | Best home |
|---|---|
| Does it apply to almost every task in this repository? | `AGENTS.md` |
| Is it specific to Claude Code? | `CLAUDE.md` or Claude Code scoped rules |
| Is it a repeatable multi-step task? | A skill folder with `SKILL.md` |
| Does it apply only to one folder, service, or file type? | Tool-supported scoped rules or a nearby local instruction file |
| Is it a durable engineering or architecture decision? | `docs/decisions.md` or an ADR |
| Does it define a full workflow process? | `WORKFLOW.md` |
| Does it define how reports or UI should look and sound? | `style-guide.md` or `DESIGN.md` |
| Is it just the starting request for a repeated task? | A prompt template such as `.prompt.md` |

---

# Final takeaway

Instruction files are most useful when they remove repeated ambiguity.

They help an AI assistant understand:

- how your project is organised;
- what quality means in your team;
- what must not be changed;
- which process to follow;
- where specialised guidance lives.

Start small. Keep instructions concrete. Separate always-on project rules from task-specific workflows. Store important decisions where the whole team can review them. And verify that your chosen AI tool actually reads the files you create.

A better prompt may improve one answer.

A well-maintained instruction system can improve many future tasks.

---

## Further reading

- AGENTS.md open format: https://agents.md/
- Claude Code memory and `CLAUDE.md`: https://docs.anthropic.com/en/docs/claude-code/memory
- Claude Code scoped rules and skills: https://docs.anthropic.com/en/docs/claude-code/skills
- OpenAI Skills: https://openai.com/academy/skills/
- OpenAI Agent Skills documentation: https://developers.openai.com/api/docs/guides/tools-skills
