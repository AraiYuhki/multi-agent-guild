# multi-agent-guild

<div align="center">

**Multi-Agent Orchestration System for Claude Code**

*One request. Eight AI agents working in parallel.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude-Code-blueviolet)](https://claude.ai)
[![tmux](https://img.shields.io/badge/tmux-required-green)](https://github.com/tmux/tmux)

[English](README.md) | [Japanese / 日本語](README_ja.md)

</div>

---

## What is this?

**multi-agent-guild** is a system that runs multiple Claude Code instances simultaneously, organized as an adventurers' guild.

**Why use this?**
- Submit one request, get 8 adventurers executing in parallel
- No waiting - you can keep submitting requests while quests run in the background
- AI remembers your preferences across sessions (Memory MCP)
- Real-time progress tracking via the quest board (`dashboard.md`)

**Guild flow:** Request → Quest → Adventure → Report

```
        You (Client / Patron)
             │
             ▼ Submit request
      ┌─────────────┐
      │  GUILDMASTER │  ← Interprets the request and delegates immediately
      └──────┬──────┘
             │ YAML files + tmux
      ┌──────▼──────┐
      │  RECEPTIONIST  │  ← Breaks quests down and assigns missions
      └──────┬──────┘
             │
    ┌─┬─┬─┬─┴─┬─┬─┬─┐
    │1│2│3│4│5│6│7│8│  ← 8 adventurers execute in parallel
    └─┴─┴─┴─┴─┴─┴─┴─┘
       ADVENTURERS
```

---

## 🚀 Quick Start

### 🪟 Windows Users (Most Common)

<table>
<tr>
<td width="60">

**Step 1**

</td>
<td>

📥 **Download this repository**

[Download ZIP](https://github.com/yohey-w/multi-agent-guild/archive/refs/heads/main.zip) and extract to `C:\tools\multi-agent-guild`

*Or use git:* `git clone https://github.com/yohey-w/multi-agent-guild.git C:\tools\multi-agent-guild`

</td>
</tr>
<tr>
<td>

**Step 2**

</td>
<td>

🖱️ **Run `install.bat`**

Right-click and select **"Run as administrator"** (required if WSL2 is not yet installed). The installer will guide you through each step — you may need to restart your PC or set up Ubuntu before re-running.

</td>
</tr>
<tr>
<td>

**Step 3**

</td>
<td>

🐧 **Open Ubuntu and run** (first time only)

```bash
cd /mnt/c/tools/multi-agent-guild
./first_setup.sh
```

</td>
</tr>
<tr>
<td>

**Step 4**

</td>
<td>

✅ **Deploy!**

```bash
./shutsujin_departure.sh
```

</td>
</tr>
</table>

#### 📅 Daily Startup (After First Install)

Open **Ubuntu terminal** (WSL) and run:

```bash
cd /mnt/c/tools/multi-agent-guild
./shutsujin_departure.sh
```

---

<details>
<summary>🐧 <b>Linux / Mac Users</b> (Click to expand)</summary>

### First-Time Setup

```bash
# 1. Clone the repository
git clone https://github.com/yohey-w/multi-agent-guild.git ~/multi-agent-guild
cd ~/multi-agent-guild

# 2. Make scripts executable
chmod +x *.sh

# 3. Run first-time setup
./first_setup.sh
```

### Daily Startup

```bash
cd ~/multi-agent-guild
./shutsujin_departure.sh
```

</details>

---

<details>
<summary>❓ <b>What is WSL2? Why do I need it?</b> (Click to expand)</summary>

### About WSL2

**WSL2 (Windows Subsystem for Linux)** lets you run Linux inside Windows. This system uses `tmux` (a Linux tool) to manage multiple AI agents, so WSL2 is required on Windows.

### Don't have WSL2 yet?

No problem! When you run `install.bat`, it will:
1. Check if WSL2 is installed (auto-install if missing)
2. Check if Ubuntu is installed (auto-install if missing)
3. Guide you to the next steps (`first_setup.sh`)

**Quick install command** (run in PowerShell as Administrator):
```powershell
wsl --install
```

Then restart your computer and run `install.bat` again.

</details>

---

<details>
<summary>📋 <b>Script Reference</b> (Click to expand)</summary>

| Script | Purpose | When to Run |
|--------|---------|-------------|
| `install.bat` | Windows: WSL2 + Ubuntu setup | First time only |
| `first_setup.sh` | Installs tmux, Node.js, Claude Code CLI + configures Memory MCP | First time only |
| `shutsujin_departure.sh` | Creates tmux sessions + starts Claude Code + loads instructions | Every day |

### What `install.bat` does automatically:
- ✅ Checks if WSL2 is installed (auto-install if missing)
- ✅ Checks if Ubuntu is installed (auto-install if missing)
- ✅ Guides you to the next steps (`first_setup.sh`)

### What `shutsujin_departure.sh` does:
- ✅ Creates tmux sessions (guildmaster + multiagent)
- ✅ Launches Claude Code on all agents
- ✅ Automatically loads instruction files for each agent
- ✅ Resets queue files for a fresh start

**After running, all agents are ready to receive requests immediately!**

</details>

---

<details>
<summary>🔧 <b>Prerequisites (for manual setup)</b> (Click to expand)</summary>

If you prefer to install dependencies manually:

| Requirement | How to install | Notes |
|-------------|----------------|-------|
| WSL2 + Ubuntu | `wsl --install` in PowerShell | Windows only |
| Set Ubuntu as default | `wsl --set-default Ubuntu` | Required for scripts to work |
| tmux | `sudo apt install tmux` | Terminal multiplexer |
| Node.js v20+ | `nvm install 20` | Required for Claude Code CLI |
| Claude Code CLI | `npm install -g @anthropic-ai/claude-code` | Anthropic's official CLI |

</details>

---

### ✅ What Happens After Setup

After running either option, **AI agents** will start automatically:

| Agent | Role | Quantity |
|-------|------|----------|
| 🛡️ Guildmaster | Master - receives your requests | 1 |
| 📋 Receptionist | Senior adventurer - splits quests and assigns missions | 1 |
| ⚔️ Adventurer | Adventurers - execute quests in parallel | 8 |

You'll see tmux sessions created:
- `guildmaster` - Connect here to submit requests
- `multiagent` - Workers running in background

---

## 📖 Basic Usage

### Step 1: Connect to the Guildmaster

After running `shutsujin_departure.sh`, all agents automatically load their instructions and are ready to work.

Open a new terminal and connect to the Guildmaster:

```bash
tmux attach-session -t guildmaster
```

### Step 2: Submit Your First Request

The Guildmaster is already initialized! Just submit your request:

```
Investigate the top 5 JavaScript frameworks and create a comparison table.
```

The Guildmaster will:
1. Write the quest to a YAML file
2. Notify the Receptionist (quest coordinator)
3. Return control to you immediately (you don't have to wait!)

Meanwhile, the Receptionist distributes the work to Adventurer workers who execute in parallel.

### Step 3: Check Progress

Open the quest board (`dashboard.md`) in your editor to see real-time progress:

```markdown
## In Progress
| Adventurer | Quest | Progress |
|--------|------|--------|
| Adventurer 1 | React research | Running |
| Adventurer 2 | Vue research | Running |
| Adventurer 3 | Angular research | Done |
```

---

## ✨ Key Features

### ⚡ 1. Parallel Execution

One request can spawn up to 8 parallel quests:

```
You: "Research 5 MCP servers"
→ 5 Adventurer start researching simultaneously
→ Reports ready in minutes, not hours
```

### 🔄 2. Non-Blocking Workflow

The Guildmaster delegates immediately and returns control to you:

```
You: Submit request → Guildmaster: Delegates → You: Can submit the next request immediately
                                           ↓
                         Adventurers: Execute in background
                                           ↓
                         Dashboard: Shows reports
```

You never have to wait for long quests to complete.

---

## 🎮 Unity Guild Specialization

This guild is optimized for Unity game development. Standard Unity skills live in:

- `skills/unity-adventurer-skills/`

Adventurers pick specialized classes (Unity engineer, AI designer, UI/UX, optimization, QA) to match each quest. Skill definitions include purpose, expected input/output, and recommended model profile.

### Model Selection (Unity)

- **Design/Architecture/Root-cause analysis** → reasoning-focused model
- **C# implementation/Unity API work** → code-focused model
- **Summaries/checklists** → fast model

### 🧠 3. Memory Across Sessions (Memory MCP)

The AI remembers your preferences:

```
Session 1: You say "I prefer simple solutions"
           → Saved to Memory MCP

Session 2: AI reads memory at startup
           → Won't suggest over-engineered solutions
```

### 📡 4. Event-Driven (No Polling)

Agents communicate via YAML files and wake each other with tmux send-keys.
**No API calls are wasted on polling loops.**

### 📸 5. Screenshot Support

VSCode's Claude Code extension lets you paste screenshots to explain issues. This CLI system brings the same capability:

```
# Configure your screenshot folder in config/settings.yaml
screenshot:
  path: "/mnt/c/Users/YourName/Pictures/Screenshots"

# Then just tell the Guildmaster:
You: "Check the latest screenshot"
You: "Look at the last 2 screenshots"
→ AI reads and analyzes your screenshots instantly
```

**💡 Windows Tip:** Press `Win + Shift + S` to take a screenshot. Configure the save location to match your `settings.yaml` path for seamless integration.

Perfect for:
- Explaining UI bugs visually
- Showing error messages
- Comparing before/after states

### 🧠 Model Configuration

| Agent | Model | Thinking | Reason |
|-------|-------|----------|--------|
| Guildmaster | Opus | Disabled | Delegation & quest board updates don't need deep reasoning |
| Receptionist | Default | Enabled | Quest distribution requires careful judgment |
| Adventurer | Default | Enabled | Actual implementation needs full capabilities |

The Guildmaster uses `MAX_THINKING_TOKENS=0` to disable extended thinking, reducing latency and cost while maintaining Opus-level judgment for high-level decisions.

### 📁 Context Management

The system uses a three-layer context structure for efficient knowledge sharing:

| Layer | Location | Purpose |
|-------|----------|---------|
| Memory MCP | `memory/guildmaster_memory.jsonl` | Persistent memory across sessions (preferences, decisions) |
| Global | `memory/global_context.md` | System-wide settings, client preferences |
| Project | `context/{project}.md` | Project-specific knowledge and state |

This design allows:
- Any Adventurer to pick up work on any project
- Consistent context across agent switches
- Clear separation of concerns
- Knowledge persistence across sessions

### Universal Context Template

All projects use the same 7-section template:

| Section | Purpose |
|---------|---------|
| What | Brief description of the project |
| Why | Goals and success criteria |
| Who | Stakeholders and responsibilities |
| Constraints | Deadlines, budget, limitations |
| Current State | Progress, next actions, blockers |
| Decisions | Decision log with rationale |
| Notes | Free-form notes and insights |

This standardized structure ensures:
- Quick onboarding for any agent
- Consistent information across all projects
- Easy handoffs between Adventurer workers

### 🛠️ Skills

Skills are not included in this repository by default.
As you use the system, skill candidates will appear on the quest board (`dashboard.md`).
Review and approve them to grow your personal skill library.

Skills can be invoked with `/skill-name`. Just tell the Guildmaster: "run `/skill-name`".

---

## 🏛️ Design Philosophy

### Why Hierarchical Structure?

The Guildmaster → Receptionist → Adventurer hierarchy exists for:

1. **Immediate Response**: Guildmaster delegates instantly and returns control to you
2. **Parallel Execution**: Receptionist distributes to multiple Adventurer simultaneously
3. **Separation of Concerns**: Each role is clearly defined — Guildmaster decides "what", Receptionist decides "who"
4. **Scalability**: Adding more Adventurer doesn't break the structure
5. **Fault Isolation**: One Adventurer failing doesn't affect others
6. **Centralized Reporting**: Only Guildmaster communicates with you, keeping information organized

### Why YAML + send-keys?

- **YAML files**: Structured communication that survives agent restarts and is human-readable for debugging
- **send-keys**: Event-driven wakeups (no polling = no wasted API calls)
- **No direct calls**: Agents can't interrupt each other or your input
- **Conflict avoidance**: Each Adventurer has dedicated files, preventing race conditions

### Why Only Receptionist Updates the Quest Board?

- **Single responsibility**: One writer = no conflicts
- **Information hub**: Receptionist receives all reports, knows the full picture
- **Consistency**: All updates go through one quality gate
- **No interruptions**: Prevents disrupting your input when Guildmaster would otherwise update the quest board

### How Skills Work

Skills (`.claude/commands/`) are **not committed to this repository** by design.

**Why?**
- Each client's workflow is different
- Skills should grow organically based on your needs
- No one-size-fits-all solution

**How to create new skills:**

```
Adventurer notices a repeatable pattern during work
    ↓
Candidate appears on the quest board (`dashboard.md`) under "Skill Candidates"
    ↓
You (the client/patron) review the candidate
    ↓
If approved, Receptionist creates the skill
```

Skills are **client-driven** — they only grow when you decide they're useful. Automatic growth would make them unmanageable, so only what you explicitly approve gets added.

---

## 🔌 MCP Setup Guide

MCP (Model Context Protocol) servers extend Claude's capabilities. Here's how to set them up:

### What is MCP?

MCP servers give Claude access to external tools:
- **Notion MCP** → Read/write Notion pages
- **GitHub MCP** → Create PRs, manage issues
- **Memory MCP** → Remember things across sessions

### Installing MCP Servers

Run these commands to add MCP servers:

```bash
# 1. Notion - Connect to your Notion workspace
claude mcp add notion -e NOTION_TOKEN=your_token_here -- npx -y @notionhq/notion-mcp-server

# 2. Playwright - Browser automation
claude mcp add playwright -- npx @playwright/mcp@latest
# Note: Run `npx playwright install chromium` first

# 3. GitHub - Repository operations
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=your_pat_here -- npx -y @modelcontextprotocol/server-github

# 4. Sequential Thinking - Step-by-step reasoning for complex problems
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking

# 5. Memory - Long-term memory across sessions (Recommended!)
# ✅ Automatically configured by first_setup.sh
# To reconfigure manually:
claude mcp add memory -e MEMORY_FILE_PATH="$PWD/memory/guildmaster_memory.jsonl" -- npx -y @modelcontextprotocol/server-memory
```

### Verify Installation

```bash
claude mcp list
```

You should see all servers listed as "Connected".

---

## 🌍 Real-World Use Cases

### Example 1: Research Quest

```
You: "Research the top 5 AI coding assistants and compare them"

What happens:
1. Guildmaster delegates to Receptionist
2. Receptionist assigns:
   - Adventurer 1: Research GitHub Copilot
   - Adventurer 2: Research Cursor
   - Adventurer 3: Research Claude Code
   - Adventurer 4: Research Codeium
   - Adventurer 5: Research Amazon CodeWhisperer
3. All 5 research simultaneously
4. Reports compiled on the quest board (`dashboard.md`)
```

### Example 2: PoC Preparation

```
You: "Prepare a PoC for the project in this Notion page: [URL]"

What happens:
1. Receptionist fetches Notion content via MCP
2. Adventurer 2: Lists items to clarify
3. Adventurer 3: Researches technical feasibility
4. Adventurer 4: Creates PoC plan document
5. All reports on the quest board (`dashboard.md`), ready for your meeting
```

---

## ⚙️ Configuration

### Language Setting

Edit `config/settings.yaml`:

```yaml
language: ja   # Japanese only
language: en   # Japanese + English translation
```

---

## 🛠️ Advanced Usage

<details>
<summary><b>Script Architecture</b> (Click to expand)</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                      FIRST-TIME SETUP (Run Once)                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  install.bat (Windows)                                              │
│      │                                                              │
│      ├── Check/Install WSL2                                         │
│      └── Check/Install Ubuntu                                       │
│                                                                     │
│  first_setup.sh (run manually in Ubuntu/WSL)                        │
│      │                                                              │
│      ├── Check/Install tmux                                         │
│      ├── Check/Install Node.js v20+ (via nvm)                      │
│      ├── Check/Install Claude Code CLI                              │
│      └── Configure Memory MCP server                                │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                      DAILY STARTUP (Run Every Day)                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  shutsujin_departure.sh                                             │
│      │                                                              │
│      ├──▶ Create tmux sessions                                      │
│      │         • "guildmaster" session (1 pane)                          │
│      │         • "multiagent" session (9 panes, 3x3 grid)           │
│      │                                                              │
│      ├──▶ Reset queue files and quest board                          │
│      │                                                              │
│      └──▶ Launch Claude Code on all agents                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

</details>

<details>
<summary><b>shutsujin_departure.sh Options</b> (Click to expand)</summary>

```bash
# Default: Full startup (tmux sessions + Claude Code launch)
./shutsujin_departure.sh

# Session setup only (without launching Claude Code)
./shutsujin_departure.sh -s
./shutsujin_departure.sh --setup-only

# Full startup + open Windows Terminal tabs
./shutsujin_departure.sh -t
./shutsujin_departure.sh --terminal

# Show help
./shutsujin_departure.sh -h
./shutsujin_departure.sh --help
```

</details>

<details>
<summary><b>Common Workflows</b> (Click to expand)</summary>

**Normal Daily Usage:**
```bash
./shutsujin_departure.sh          # Start everything
tmux attach-session -t guildmaster     # Connect to submit requests
```

**Debug Mode (manual control):**
```bash
./shutsujin_departure.sh -s       # Create sessions only

# Manually start Claude Code on specific agents
tmux send-keys -t guildmaster:0 'claude --dangerously-skip-permissions' Enter
tmux send-keys -t multiagent:0.0 'claude --dangerously-skip-permissions' Enter
```

**Restart After Crash:**
```bash
# Kill existing sessions
tmux kill-session -t guildmaster
tmux kill-session -t multiagent

# Start fresh
./shutsujin_departure.sh
```

</details>

<details>
<summary><b>Convenient Aliases</b> (Click to expand)</summary>

Running `first_setup.sh` automatically adds these aliases to `~/.bashrc`:

```bash
alias css='cd /mnt/c/tools/multi-agent-guild && ./shutsujin_departure.sh'  # Setup + deploy
alias csm='cd /mnt/c/tools/multi-agent-guild'                              # Navigate to directory only
```

*To apply aliases, run `source ~/.bashrc` or restart your terminal. On WSL, run `wsl --shutdown` in PowerShell first — simply closing the window does not terminate WSL.*

</details>

---

## 📁 File Structure

<details>
<summary><b>Click to expand file structure</b></summary>

```
multi-agent-guild/
│
│  ┌─────────────────── SETUP SCRIPTS ───────────────────┐
├── install.bat               # Windows: First-time setup
├── first_setup.sh            # Ubuntu/Mac: First-time setup
├── shutsujin_departure.sh    # Daily startup (auto-loads instructions)
│  └────────────────────────────────────────────────────┘
│
├── instructions/             # Agent instruction files
│   ├── guildmaster.md             # Guildmaster instructions
│   ├── receptionist.md               # Receptionist instructions
│   └── adventurer.md           # Adventurer instructions
│
├── config/
│   └── settings.yaml         # Language and other settings
│
├── projects/                # Project details (git-ignored, contains client data)
│   └── <project_id>.yaml   # Full project info (client, quests, Notion links, etc.)
│
├── queue/                    # Communication files
│   ├── guildmaster_to_receptionist.yaml   # Requests from Guildmaster to Receptionist
│   ├── tasks/                # Individual quest files
│   └── reports/              # Adventurer reports
│
├── memory/                   # Memory MCP storage
├── dashboard.md              # Quest board & real-time progress overview
└── CLAUDE.md                 # Project context for Claude
```

</details>

---

## 📂 Project Management

This system manages **all white-collar quests**, not just its own development. Projects can live anywhere on your filesystem — they don't need to be inside this repository.

### How It Works

```
config/projects.yaml          # Project registry (ID, name, path, progress)
projects/<project_id>.yaml    # Full project details (client info, quests, Notion links, etc.)
```

- **`config/projects.yaml`**: Lists all projects with basic metadata (ID, name, path, progress)
- **`projects/<id>.yaml`**: Contains full details for each project (client info, contract, quests, related files, Notion pages, etc.)
- **Project files** (source code, docs, etc.) live at the `path` specified in the project entry — anywhere on the filesystem
- **`projects/` is git-ignored** because it may contain confidential client information

### Example

```yaml
# config/projects.yaml
projects:
  - id: my_client
    name: "Client X Consulting"
    path: "/mnt/c/Consulting/client_x"
    progress: active

# projects/my_client.yaml
id: my_client
client:
  name: "Client X"
  company: "X Corp"
contract:
  fee: "monthly"
current_quests:
  - id: quest_001
    name: "System architecture review"
    progress: in_progress
```

This separation allows the Guildmaster system to orchestrate quests across multiple external projects while keeping project details private and out of version control.

---

## 🔧 Troubleshooting

<details>
<summary><b>MCP tools not working?</b></summary>

MCP tools are "deferred" and need to be loaded first:

```
# Wrong - tool not loaded
mcp__memory__read_graph()  ← Error!

# Correct - load first
ToolSearch("select:mcp__memory__read_graph")
mcp__memory__read_graph()  ← Works!
```

</details>

<details>
<summary><b>Agents asking for permissions?</b></summary>

Make sure to start with `--dangerously-skip-permissions`:

```bash
claude --dangerously-skip-permissions --system-prompt "..."
```

</details>

<details>
<summary><b>Workers stuck?</b></summary>

Check the worker's pane:
```bash
tmux attach-session -t multiagent
# Use Ctrl+B then number to switch panes
```

</details>

---

## 📚 tmux Quick Reference

| Command | Description |
|---------|-------------|
| `tmux attach -t guildmaster` | Connect to Guildmaster |
| `tmux attach -t multiagent` | Connect to workers |
| `Ctrl+B` then `0-8` | Switch between panes |
| `Ctrl+B` then `d` | Detach (leave running) |
| `tmux kill-session -t guildmaster` | Stop Guildmaster session |
| `tmux kill-session -t multiagent` | Stop worker sessions |

### 🖱️ Mouse Support

`first_setup.sh` automatically configures tmux mouse support (`set -g mouse on` in `~/.tmux.conf`). This enables the following mouse operations:

| Action | Description |
|--------|-------------|
| Scroll wheel | Scroll within a pane |
| Click on a pane | Switch focus between panes |
| Drag pane border | Resize panes |

> **Note:** If you set up tmux manually (without `first_setup.sh`), add `set -g mouse on` to your `~/.tmux.conf` to enable mouse support.

---

## 🙏 Credits

Based on [Claude-Code-Communication](https://github.com/Akira-Papa/Claude-Code-Communication) by Akira-Papa.

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details.

---

<div align="center">

**Command your AI army. Build faster.**

</div>
