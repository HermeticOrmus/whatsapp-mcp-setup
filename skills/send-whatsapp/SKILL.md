---
name: send-whatsapp
description: Format and send conversational content via WhatsApp with rich formatting
version: 1.0.0
auto_trigger:
  keywords:
    - "send this to whatsapp"
    - "send me this on whatsapp"
    - "whatsapp me this"
    - "send to whatsapp"
    - "forward this to whatsapp"
---

# Send Content to WhatsApp

**Purpose:** Format conversational content with rich WhatsApp markup and send it as a well-structured message series.

---

## Destination

| Target | JID | When |
|--------|-----|------|
| **Personal** | `YOUR_PHONE@c.us` | Default |
| **Work Group** | `YOUR_GROUP_ID@g.us` | When specified |

**CUSTOMIZE**: Replace the JIDs above with your own phone number and group IDs.

---

## Formatting Rules

### WhatsApp Markup

| Format | Syntax | Use For |
|--------|--------|---------|
| **Bold** | `*text*` | Titles, key terms, emphasis |
| _Italic_ | `_text_` | Quotes, secondary emphasis |
| ~~Strike~~ | `~text~` | Corrections, contrasts |
| `Mono` | `` `text` `` | Code, commands, technical refs |

### Structure Elements

| Element | Format |
|---------|--------|
| Section dividers | `━━━━━━━━━━━━━━━━━━━━` |
| Numbered lists | 1️⃣ 2️⃣ 3️⃣ 4️⃣ 5️⃣ |
| Bullet points | `•` (not `-`) |
| Sub-bullets | `  ◦` or `  →` |
| Warnings | ⚠️ prefix |
| Key insight | 💡 prefix |
| Action item | 🎯 prefix |

---

## Message Chunking

WhatsApp has practical length limits. Split content into logical chunks:

1. Each major section/topic = one message
2. Keep individual messages under ~3000 characters
3. Use section headers at the top of each message
4. Send messages **sequentially** to preserve order (not in parallel)

### Chunk Structure

```
[Emoji] *SECTION TITLE* [Emoji]
[Optional subtitle in italic]

━━━━━━━━━━━━━━━━━━━━

[Content with formatting]

━━━━━━━━━━━━━━━━━━━━
```

---

## Signature

Every final message must end with:

```
━━━━━━━━━━━━━━━━━━━━

_Sent from Claude Code_ 🤖
```

Only on the LAST message of the series.

---

## Procedure

### Step 1: Identify Content Scope

Determine what to send:
- Full conversation? Specific section? Summary?
- If ambiguous, ask: "The whole conversation or a specific part?"

### Step 2: Plan Message Chunks

Break content into logical sections. Each chunk should be:
- Self-contained (makes sense alone)
- Topically focused (one theme per message)
- Scannable (headers, bullets, bold key terms)

### Step 3: Format Each Chunk

Apply WhatsApp markup:
- Bold all section headers and key terms
- Add contextual emojis
- Use numbered emoji lists for sequences
- Use bullet points (•) for unordered items
- Add section dividers between major blocks

### Step 4: Send Sequentially

```
mcp__periskope-whatsapp__periskope_send_message({
  phone: "YOUR_PHONE@c.us",
  message: chunk1
})
// Wait for success before sending next
mcp__periskope-whatsapp__periskope_send_message({
  phone: "YOUR_PHONE@c.us",
  message: chunk2
})
```

### Step 5: Confirm

Report how many messages were sent and a brief summary.

---

## Important Notes

1. **Always send sequentially** -- parallel sends arrive out of order
2. **Signature only on the final message**
3. **Default to personal number** unless told otherwise
4. **Max ~3000 chars per message** -- split if longer
5. **Don't over-summarize** -- the user asked to send the content, not a summary
6. **Preserve the depth** -- formatting should enhance, not flatten

---

**Status:** Active
**Auto-trigger:** Yes (on "send this to whatsapp" and variants)
