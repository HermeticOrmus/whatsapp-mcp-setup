# WhatsApp MCP for Claude Code

Send and receive WhatsApp messages directly from Claude Code using the [Periskope](https://periskope.app) WhatsApp MCP server.

This guide walks you through setting up WhatsApp as an MCP (Model Context Protocol) tool in Claude Code, plus optional skills that make WhatsApp messaging a natural part of your workflow.

---

## What You Get

Once set up, Claude Code can:

- **Send messages** to any WhatsApp contact or group
- **Read messages** from chats
- **Search contacts** by name
- **Search messages** by content
- **List and manage chats**
- **React to messages**
- **Create groups**, add/remove participants

All from within your Claude Code session. No switching apps.

---

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- A WhatsApp account (personal or business)
- Node.js 18+ installed (`node --version` to check)

---

## Step 1: Get a Periskope Account

[Periskope](https://periskope.app) is a cloud-based WhatsApp API that provides the MCP server.

1. Go to [periskope.app](https://periskope.app) and create an account
2. Connect your WhatsApp number by scanning the QR code (similar to WhatsApp Web)
3. Once connected, go to **Settings > API Keys**
4. Create a new API key -- name it something like "Claude Code"
5. Copy the API key (a long JWT token) -- you'll need it in the next step
6. Note your **phone number** in international format without the `+` (e.g., `16264296854` for a US number `+1 626-429-6854`)

---

## Step 2: Configure the MCP Server

Claude Code uses a file called `mcp.json` to know which MCP servers to connect to.

### Find your config location

The file lives at:

```
~/.claude/mcp.json
```

If it doesn't exist yet, create it. If it already exists, you'll add to it.

### Add the Periskope server

Open `~/.claude/mcp.json` and add (or merge into existing config):

```json
{
  "mcpServers": {
    "periskope-whatsapp": {
      "command": "npx",
      "args": ["-y", "@periskope/whatsapp-mcp"],
      "env": {
        "PERISKOPE_API_KEY": "YOUR_API_KEY_HERE",
        "PERISKOPE_PHONE_ID": "YOUR_PHONE_NUMBER_HERE"
      }
    }
  }
}
```

Replace:
- `YOUR_API_KEY_HERE` with the JWT token from Step 1
- `YOUR_PHONE_NUMBER_HERE` with your phone number (digits only, no `+`)

### If you already have other MCP servers

Just add the `periskope-whatsapp` block inside the existing `mcpServers` object. Example with an existing GitHub server:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your-token"
      }
    },
    "periskope-whatsapp": {
      "command": "npx",
      "args": ["-y", "@periskope/whatsapp-mcp"],
      "env": {
        "PERISKOPE_API_KEY": "YOUR_API_KEY_HERE",
        "PERISKOPE_PHONE_ID": "YOUR_PHONE_NUMBER_HERE"
      }
    }
  }
}
```

---

## Step 3: Restart Claude Code

After saving `mcp.json`, restart Claude Code so it picks up the new server:

```bash
# If using the CLI
claude

# Or just close and reopen the app/terminal
```

The first time it runs, `npx` will download the `@periskope/whatsapp-mcp` package automatically.

---

## Step 4: Test It

In a Claude Code session, try:

```
Search my WhatsApp contacts for "Mom"
```

or:

```
Send a WhatsApp message to 16264296854@c.us saying "Hello from Claude Code!"
```

**Contact format**: WhatsApp uses JIDs (Jabber IDs):
- Personal chats: `phonenumber@c.us` (e.g., `16264296854@c.us`)
- Groups: `groupid@g.us` (e.g., `120363422067085557@g.us`)

You can always search for contacts by name first, then use the returned JID.

---

## Step 5 (Optional): Install Skills

Skills are reusable prompt templates that teach Claude Code how to handle specific tasks. They live in `~/.claude/skills/`.

This repo includes two WhatsApp skills you can install:

### Send WhatsApp Skill

Formats and sends rich content (conversations, research, analysis) to WhatsApp with proper formatting.

```bash
# Create the skill directory
mkdir -p ~/.claude/skills/send-whatsapp

# Copy the skill file
cp skills/send-whatsapp/SKILL.md ~/.claude/skills/send-whatsapp/SKILL.md
```

**Then customize** `SKILL.md`:
- Replace the phone number (`YOUR_PHONE@c.us`) with your own
- Adjust the group JIDs to your own WhatsApp groups
- Modify the style/formatting to match your preferences

**Usage**: Just say "send this to WhatsApp" or "WhatsApp me this" in a Claude Code session.

### Notify WhatsApp Skill

Sends operational notifications (task completions, errors, deployments) to a WhatsApp group or contact.

```bash
mkdir -p ~/.claude/skills/notify-whatsapp
cp skills/notify-whatsapp/SKILL.md ~/.claude/skills/notify-whatsapp/SKILL.md
```

**Customize** the destination JID and message templates in the SKILL.md file.

**Usage**: Claude Code will auto-notify after major task completions, or you can say "notify me on WhatsApp when done".

---

## Available MCP Tools

Once connected, these tools become available to Claude Code:

| Tool | What it does |
|------|-------------|
| `periskope_send_message` | Send a message to a contact or group |
| `periskope_list_chats` | List all your WhatsApp chats |
| `periskope_list_messages_in_a_chat` | Read messages from a specific chat |
| `periskope_search_contact` | Find a contact by name |
| `periskope_search_message` | Search messages by content |
| `periskope_search_chat` | Search chats by name |
| `periskope_get_chat` | Get details about a specific chat |
| `periskope_get_contact_by_id` | Get contact details by JID |
| `periskope_get_message_by_id` | Get a specific message |
| `periskope_react_to_message` | React to a message with an emoji |
| `periskope_create_chat` | Create a new group chat |
| `periskope_forward_message` | Forward a message to another chat |
| `periskope_edit_message` | Edit a sent message |
| `periskope_delete_message` | Delete a message |
| `periskope_add_participants_to_group` | Add people to a group |
| `periskope_remove_participants_from_group` | Remove people from a group |
| `periskope_promote_participants_to_admins` | Make someone a group admin |
| `periskope_demote_participants_from_admins` | Remove admin status |
| `periskope_update_chat` | Update chat metadata |
| `periskope_update_chat_labels` | Manage chat labels |
| `periskope_update_chat_settings` | Update chat settings |
| `periskope_update_contact` | Update contact information |
| `periskope_update_contact_labels` | Manage contact labels |

---

## WhatsApp Formatting Reference

WhatsApp supports basic rich text:

| Format | Syntax | Example |
|--------|--------|---------|
| **Bold** | `*text*` | `*important*` |
| _Italic_ | `_text_` | `_emphasis_` |
| ~~Strikethrough~~ | `~text~` | `~old info~` |
| `Monospace` | `` `text` `` | `` `code` `` |

Good structural elements for messages:
- Use `*BOLD HEADERS*` for section titles
- Bullet points with `\u2022` (bullet character)
- Numbered lists with emoji numbers: 1️⃣ 2️⃣ 3️⃣
- Section dividers: `\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501\u2501`

---

## REST API Fallback

If the MCP server isn't running, you can also use the Periskope REST API directly:

```
POST https://api.periskope.app/v1/message/send

Headers:
  Authorization: Bearer YOUR_API_KEY
  x-phone: YOUR_PHONE_NUMBER
  Content-Type: application/json

Body:
{
  "chat_id": "phonenumber@c.us",
  "message": "Your message here"
}
```

This can be useful for scripts, cron jobs, or other automation outside of Claude Code.

---

## Troubleshooting

### "MCP server not found" or tools don't appear

- Make sure `mcp.json` is at `~/.claude/mcp.json`
- Restart Claude Code after editing the file
- Check that Node.js 18+ is installed

### Messages not sending

- Verify your Periskope account is connected (check the Periskope dashboard)
- Make sure the API key hasn't expired
- Check the phone number format (digits only, with country code, no `+`)

### Multiple MCP servers failing

If you have multiple MCP servers that use `mcp-remote`, they can conflict on the default callback port (22227). Assign unique ports:

```json
{
  "mcpServers": {
    "service-a": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://some-service.com/mcp", "22230"]
    },
    "service-b": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://other-service.com/mcp", "22231"]
    }
  }
}
```

The second positional argument after the URL sets the callback port.

---

## Security Notes

- Your `mcp.json` contains API keys. **Never commit it to a public repo.**
- Add `mcp.json` to your `.gitignore` if it's in a synced directory.
- Periskope API keys are JWTs with long expiration. Rotate them periodically.
- The MCP server runs locally on your machine. Messages go through Periskope's cloud to WhatsApp.

---

## Links

- [Periskope](https://periskope.app) -- WhatsApp API provider
- [Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code) -- Claude Code documentation
- [MCP Specification](https://modelcontextprotocol.io) -- Model Context Protocol spec
- [@periskope/whatsapp-mcp](https://www.npmjs.com/package/@periskope/whatsapp-mcp) -- The npm package

---

*Built by [Ormus](https://ormus.solutions) with Claude Code.*

---

## Part of the Libre Open-Source Stack for Claude Code

This repository is part of a growing family of open-source toolkits for Claude Code.

### Libre suite — comprehensive plugin bundles

- [LibreUIUX-Claude-Code](https://github.com/HermeticOrmus/LibreUIUX-Claude-Code) — UI/UX development (152 agents, 70 plugins, 76 commands, 74 skills)
- [LibreArch-Claude-Code](https://github.com/HermeticOrmus/LibreArch-Claude-Code) — Software architecture and system design
- [LibreCopy-Claude-Code](https://github.com/HermeticOrmus/LibreCopy-Claude-Code) — Technical writing and documentation engineering
- [LibreDevOps-Claude-Code](https://github.com/HermeticOrmus/LibreDevOps-Claude-Code) — DevOps engineering and infrastructure automation
- [LibreEmbed-Claude-Code](https://github.com/HermeticOrmus/LibreEmbed-Claude-Code) — Embedded systems, firmware, and IoT development
- [LibreFinTech-Claude-Code](https://github.com/HermeticOrmus/LibreFinTech-Claude-Code) — Financial technology development
- [LibreGEO-Claude-Code](https://github.com/HermeticOrmus/LibreGEO-Claude-Code) — AI-search optimization (ChatGPT, Perplexity, Gemini, Google AI Overviews)
- [LibreGameDev-Claude-Code](https://github.com/HermeticOrmus/LibreGameDev-Claude-Code) — Game development across Godot, Unity, Unreal
- [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) — ML engineering and AI operations
- [LibreMobileDev-Claude-Code](https://github.com/HermeticOrmus/LibreMobileDev-Claude-Code) — Mobile app development (Flutter, React Native, native iOS, native Android)
- [LibreSecOps-Claude-Code](https://github.com/HermeticOrmus/LibreSecOps-Claude-Code) — Security operations

### Skills mini-repos — single CLAUDE.md drop-ins

- [vibe-engineer-skills](https://github.com/HermeticOrmus/vibe-engineer-skills) — Direct AI codegen well (hypothesis → scope → validate → reject working-but-wrong)
- [markdown-discipline-skills](https://github.com/HermeticOrmus/markdown-discipline-skills) — Strip AI-slop from markdown (no em dashes, no marketing fluff)
- [shell-safety-skills](https://github.com/HermeticOrmus/shell-safety-skills) — `set -euo pipefail` discipline + 15 failure-mode examples
- [commit-standard-skills](https://github.com/HermeticOrmus/commit-standard-skills) — Ormus Commit Standard v1.0 + commit-msg hook + commitlint
- [unwoke-skills](https://github.com/HermeticOrmus/unwoke-skills) — Strip AI theater (ten sins to eliminate, symmetric engagement)
- [python-conventions-skills](https://github.com/HermeticOrmus/python-conventions-skills) — Modern Python 3.11+ (types, pathlib, async, ruff, mypy, uv)
- [typescript-conventions-skills](https://github.com/HermeticOrmus/typescript-conventions-skills) — TypeScript strict mode, discriminated unions, Result types
- [hermetic-laws-skills](https://github.com/HermeticOrmus/hermetic-laws-skills) — Seven Hermetic Principles applied to engineering
- [riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills) — Research / Innovate / Plan / Execute / Review systematic dev
- [six-day-cycle-skills](https://github.com/HermeticOrmus/six-day-cycle-skills) — Sustainable shipping cadence with mandatory rest
- [token-optimization-skills](https://github.com/HermeticOrmus/token-optimization-skills) — Claude Code token + context optimization
- [osint-skills](https://github.com/HermeticOrmus/osint-skills) — OSINT research methodology (multi-wave investigative spiral)
- [calcinate-skills](https://github.com/HermeticOrmus/calcinate-skills) — Stage 1 of the Magnum Opus (burn project bloat)
- [claude-md-overhaul-skills](https://github.com/HermeticOrmus/claude-md-overhaul-skills) — Audit CLAUDE.md and MEMORY.md against caps
- [session-handoff-skills](https://github.com/HermeticOrmus/session-handoff-skills) — Session handoff + pickup discipline
- [naming-skills](https://github.com/HermeticOrmus/naming-skills) — Product naming methodology (mine the brand's vocabulary)
- [magnum-opus-skills](https://github.com/HermeticOrmus/magnum-opus-skills) — Seven-stage alchemy applied to project transformation

### Template source

- [andrej-karpathy-skills](https://github.com/HermeticOrmus/andrej-karpathy-skills) — the canonical single-file CLAUDE.md pattern (fork of jiayuan_jy's original)

Star the family, not just one — that's how the suite stays coherent.
