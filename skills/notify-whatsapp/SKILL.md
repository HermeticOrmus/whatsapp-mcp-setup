---
name: notify-whatsapp
description: Send WhatsApp notification about completed work, errors, or important updates
version: 1.0.0
auto_trigger:
  keywords:
    - "send me a whatsapp"
    - "notify me on whatsapp"
    - "whatsapp me when done"
    - "message me on whatsapp"
  contexts:
    - task_completion
    - significant_updates
    - errors
---

# Notify via WhatsApp

**Purpose:** Send WhatsApp notifications about completed work, significant updates, or issues requiring attention.

---

## When to Use

- User explicitly requests "send me a WhatsApp"
- Major task completion (research, refactoring, deployment)
- Errors requiring attention
- Significant project updates
- User says "notify me when done"

---

## Destination

**CUSTOMIZE**: Replace with your own JID.

```
Default: YOUR_PHONE@c.us
Group:   YOUR_GROUP_ID@g.us  (if you have a dedicated notifications group)
```

---

## Message Templates

### Task Completion

```
✅ {Task Name} complete

{2-3 sentence summary}

📊 Stats:
• Files modified: {count}
• Lines changed: {count}
• Commit: {hash}

🎯 Next steps:
• {Next action 1}
• {Next action 2}
```

### Research Complete

```
🔍 Research complete: {Topic}

{Key insight or finding}

📚 Deliverables:
• {Document 1}
• {Document 2}

💡 Top insights:
• {Insight 1}
• {Insight 2}

🎯 Recommended next action:
{Description}
```

### Deployment

```
🚀 Deployed: {Project/Feature}

Commit: {hash}

Changes:
• {Change 1}
• {Change 2}

Status: ✅ Live
URL: {url}
```

### Error / Attention Needed

```
⚠️ Attention needed: {Error type}

{Brief description}

Impact: {What's affected}

🔧 Suggested fix:
{Recommendation}
```

---

## Procedure

### Step 1: Determine Notification Type

Based on context, identify: Task Completion, Research, Deployment, Error, or Custom.

### Step 2: Gather Key Info

- What was completed (1-2 sentences)
- Key results (3-5 outcomes)
- Stats (files, lines, commits)
- Next steps
- File locations

### Step 3: Format Message

- Keep under 4096 characters
- Direct, functional tone
- Include purposeful emoji (✅ 🔍 🚀 ⚠️ 📊 🎯)
- Clear sections
- Actionable next steps

### Step 4: Send via MCP

```
mcp__periskope-whatsapp__periskope_send_message({
  phone: "YOUR_PHONE@c.us",
  message: formattedMessage
})
```

### Step 5: Confirm Delivery

Verify send success and report to user.

---

## Important Notes

1. Keep messages under 4096 chars (WhatsApp limit)
2. Include actionable next steps
3. Reference file locations where applicable
4. Use functional, direct tone

---

**Status:** Active
**Auto-trigger:** Yes (on completion keywords)
