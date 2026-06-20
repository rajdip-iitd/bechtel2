# Getting Started with Claude Code

> Build websites, data tools, and automations by describing what you want in plain English. No coding experience required.

---

## What is Claude Code?

Claude Code is a command-line tool (CLI) that lets you build real projects by having a conversation. You describe a problem, and it writes all the code, creates all the files, and delivers a working solution.

You say: *"Organize my camera photos by date and event"*  
You get: A complete system that renames files, creates folders, and builds a browsing webpage.

No programming languages. No technical jargon. No memorizing commands.

---

## Pricing

Claude Code runs on Anthropic's platform. You have two options:

| Plan | Cost | Best For |
|------|------|----------|
| Pro | $20/month | Beginners and casual builders |
| Max 5x | $100/month | Daily builders |
| Max 20x | $200/month | Heavy, all-day usage |
| API Credits | Pay-as-you-go | If you prefer not to subscribe |

**Typical project costs on pay-as-you-go:**
- Simple website: $2-10
- Data analysis: $10-25
- Complex project: $25+

Use `/cost` inside Claude Code at any time to check your spending.

---

## Installation

### Prerequisites

Nothing extra required. The installer handles everything automatically.

### Mac

Open Terminal (`Cmd + Space`, type "Terminal", press Enter), then run:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

### Windows

Open PowerShell (`Windows + R`, type "powershell", press Enter), then run:

```powershell
irm https://claude.ai/install.ps1 | iex
```

### Prefer a desktop app?

Claude Code also has a Desktop app for Mac and Windows. Download it and skip the terminal entirely. You can always switch to the terminal version later.

### Verify your installation

```bash
claude --version
```

If you see a version number, you are all set.

---

## Your First Session (5 Minutes)

**Step 1: Create a workspace**

```bash
cd Documents
mkdir my-projects
cd my-projects
```

**Step 2: Start Claude Code**

```bash
claude
```

**Step 3: Start the conversation**

Instead of a boring "hello", try this:

> *"I'm in a new project folder. Can you help me understand what we could build together and show me what you can do?"*

Claude will explain its capabilities, suggest project ideas based on your folder, and ask what problems you're trying to solve.

**When you're done:** type `/exit`

---

## Essential Terminal Commands

You only need these to get started. Don't memorize them — just refer back here.

```bash
pwd              # Where am I right now?
ls               # What's in this folder? (Mac/Linux)
dir              # What's in this folder? (Windows)
cd foldername    # Go into a folder
cd ..            # Go back up one level
mkdir newname    # Create a new folder
```

**Smart naming rules:**
- Use lowercase: `my-project` not `My Project`
- Use hyphens: `photo-organizer` not `photo organizer`
- No spaces: `My Cool Project` breaks commands
- No special characters like `project@#$%` causes errors

---

## Four Projects to Build Right Now

Pick the one that solves a real problem in your life.

### Project 1: Personal Website

```bash
cd Documents/my-projects
mkdir portfolio-site
cd portfolio-site
claude
```

**Prompt to use:**
```
I need a simple, clean website for myself. Include sections for: my name and what I do, 
a short about me paragraph, my contact information, and 2-3 sample photos. Keep it 
professional but not boring. Make it work well on phones too. Ask me clarifying 
questions until you are 90% confident about the task before you start.
```

**What happens next:**

Claude asks clarifying questions like "What's your profession?" and "What style do you prefer?" Answer naturally, be comfortable to say “I don't know” or “not sure”, don't overthink it.

**When Claude shows you the result:**

Open the HTML file in your browser. If you don't love something, just tell Claude: "Make the header bigger" or "Change the colors to something more blue" or "This feels too fancy, make it simpler."

---

### Project 2: Expense Tracker

```bash
cd Documents/my-projects
mkdir expense-tracker
cd expense-tracker
claude
```

**Prompt to use:**
```
I have bank statements in this folder. Create something where I can see spending by 
month, day, and categories like food, transport, entertainment. Include a weekly 
summary of patterns. Anything you are not sure of midway, just ask me.
```
**What Claude builds:**

Usually a simple web page where it has processed your existing data, categorized expenses automatically, and created charts showing spending patterns you never noticed.

**How to improve it:**

It's simple enough that you'll definitely get something working, but useful enough to actually solve a problem. After it is working, try: "Can you analyze my spending patterns and suggest where I could be more mindful?" or "Give me your honest review on my spending."

---

### Project 3: Purchase Decision Tool

```bash
cd Documents/my-projects
mkdir car-purchase
cd car-purchase
claude
```

**Prompt to use:**
```
I want to buy a reliable, affordable family car within my budget of $X. My location 
is [your area]. Help me research the details, compare advantages and disadvantages, 
and create a decision framework based on my budget and preferences.
```

**What happens:**
Claude creates a comprehensive research tool that pulls together reliability ratings, pricing data, dealer reviews, and financing options. It builds comparison charts and helps you identify the best deals in your
area. 

**The Result:**
What would take days of scattered research across multiple websites becomes an organized, actionable analysis.

Works for any big purchase: car, laptop, apartment.

---

### Project 4: Data Dashboard

```bash
cd Documents/my-projects
mkdir data-dashboard
cd data-dashboard
claude
```

**Prompt to use:**
```
I have a CSV file called [filename.csv] in this folder. Build a simple web dashboard 
that shows: the total and average of [main column], a bar chart breaking it down by 
[category column], and a filter to view by [date or category]. Use plain HTML, CSS, 
and JavaScript — no frameworks. The entire output should be a single file I can open 
directly in my browser.
```
If you don’t have a dataset handy, create a test file first:

*Create a sample CSV file called expenses.csv with 20 rows of fictional monthly expenses — include columns for date, category (food/transport/entertainment), and amount. Then build the dashboard from it*

---

## Your 30-Minute Action Plan

| Time | What to Do |
|------|-----------|
| Minutes 1-10 | Pick the project that solves your biggest current problem |
| Minutes 11-15 | Follow the setup steps exactly as written above |
| Minutes 16-25 | Have the conversation with Claude, ask for changes you want |
| Minutes 26-30 | Test your result, make one small improvement |

**Important:** Do not try to build all four projects in one day. Pick one, make it work, and feel proud. The goal is proving to yourself that you can build useful things without knowing how to code.

---

## Tips That Actually Matter

**Be specific.** Instead of "make a website," say "make a website for my import/export business with services, pricing, and a contact form."

**Ask questions without hesitation.** If the result doesn't feel right, say so. "This looks like it was built in 2005 — completely redesign it." Claude genuinely learns your preferences through questions.

**Use CLAUDE.md.** Create a file called `CLAUDE.md` in your project folder with a few lines about what you're building. Claude reads it automatically every session, so it remembers your context.

**It's okay not to understand everything.** Focus on results first, understanding second. Claude will explain
technical concepts when you ask, but you don't need to understand every line of code to successfully use what it creates

---

## Common Issues and How to Handle Them

**Unexpected costs** - Run `/cost` regularly. Set a budget before you start and stick to it.

**Claude says "done" but things are missing** - Always test what it builds. Ask "show me the actual implementation" if something seems incomplete.

**Files were changed unexpectedly** - Claude modifies files directly without asking. Keep backups of anything important. Say "undo that change" if something goes wrong.

**First attempt looks off** - That's normal. Be more specific in your follow-up. "The colors feel too corporate, switch to something warmer" is enough to get a full redesign.

---

## What You're Actually Learning

You are not learning programming. You're learning to collaborate with an AI that happens to be excellent at programming. This is a fundamentally different and much more accessible skill.

As you build, you'll naturally start seeing patterns in how Claude thinks through problems. Without trying, you'll absorb how pieces of a project fit together. The understanding follows the building — not the other way around.

---

## Using Claude Code with DeepSeek v4 (Cost-Saving Alternative)
 
If you want to reduce API costs, you can point Claude Code at DeepSeek's API instead of Anthropic's. DeepSeek offers compatible endpoints that Claude Code can talk to with a small configuration change.
 
### Step 1: Get a DeepSeek API Key
 
Sign up at [platform.deepseek.com](https://platform.deepseek.com) and generate an API key from your dashboard. It will look like `sk-xxxxxxxxxxxxxxxx`. Keep it somewhere safe.
 
### Step 2: Install Claude Code (Windows)
 
For Windows users, a community-maintained installer bundles Node.js, Git, and Claude Code together:
 
1. Visit the releases page: `https://github.com/Redster1/claude-code-windows-installer-withprerequisites/releases`
2. Download `ClaudeCodeInstaller.exe`
3. Right-click it and select **Run as administrator**
> **Note:** If the installer gets stuck at the shortcut creation step, close it and continue to the verification step below anyway. The core installation usually completes fine.
 
### Step 3: Fix Common Windows Issues
 
**PowerShell execution policy error**
 
If you see an error like `running scripts is disabled on this system`, open PowerShell as Administrator and run:
 
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
 
**"claude" not recognised error**
 
If PowerShell cannot find the `claude` command, run this in an Administrator PowerShell to add it to your PATH:
 
```powershell
$p = [System.Environment]::GetEnvironmentVariable("PATH","User")
[System.Environment]::SetEnvironmentVariable("PATH","$p;C:\Users\$env:USERNAME\AppData\Roaming\npm","User")
```
 
Then open a fresh PowerShell window and verify with:
 
```bash
claude --version
```
 
If it still fails, restart your PC and try again.
 
### Step 4: Set the DeepSeek Environment Variables
 
Before starting Claude Code each session, run these in PowerShell to redirect traffic to DeepSeek's API. Replace `your-api-key-here` with your actual DeepSeek key:
 
```powershell
$env:ANTHROPIC_BASE_URL="https://api.deepseek.com/anthropic"
$env:ANTHROPIC_AUTH_TOKEN="your-api-key-here"
$env:ANTHROPIC_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek-v4-pro"
$env:ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek-v4-flash"
$env:CLAUDE_CODE_SUBAGENT_MODEL="deepseek-v4-pro"
$env:CLAUDE_CODE_EFFORT_LEVEL="max"
```
 
These variables only last for your current PowerShell session. See the tip below to make them permanent.
 
> **Tip: Make the variables permanent.** Instead of pasting these every session, add them to your PowerShell profile. Run `notepad $PROFILE` and paste the block above (with your real key) into that file. Save and close. From now on every new PowerShell window will have DeepSeek configured automatically.
 
### Step 5: Navigate to Your Project and Start
 
Open your project folder in File Explorer, right-click inside it, and select **Open in Terminal**. Then run:
 
```bash
claude
```
 
You should see the Claude Code welcome screen. You are now running through DeepSeek's API.
 
### Step 6: Final Check
 
Send a test prompt to confirm everything is working, then ask Claude to install Python and any packages you need for your project:
 
```
Install Python and the packages needed to read and edit Word and Excel files.
```
 
Once that completes without errors, your environment is ready for subsequent projects.
 
---
 
### DeepSeek vs Anthropic API: Quick Comparison
 
| | Anthropic (default) | DeepSeek v4 |
|---|---|---|
| Setup | Works out of the box | Requires env variable config |
| Pricing | Per token, higher rates | Significantly cheaper |
| Model quality | Claude Sonnet/Opus | DeepSeek v4 Pro |
| Best for | Production, reliability | Cost-conscious experimentation |
 
---
 
## Resources
 
- [Claude Code Documentation](https://docs.claude.ai) — official docs and reference
- [Anthropic Support](https://support.claude.ai) — billing, plans, and account questions
- [DeepSeek Platform](https://platform.deepseek.com) — API keys and usage dashboard
- [Windows Installer (community)](https://github.com/Redster1/claude-code-windows-installer-withprerequisites/releases) — one-click Windows setup

---

*Start with the thing you've been putting off because it felt too technical.*
