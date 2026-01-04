# Claude Code Voice

Voice conversations with Claude about your code. Have Claude call you to brainstorm, debug, or discuss your projects.

## Overview

Claude Code Voice is a CLI tool and Claude Code skill that enables phone conversations about your codebase. When you're deep in a coding session and want to talk through a problem, just run `/call` and Claude will call your phone with full context about your project.

**Key Features:**

- **Outbound calls** — Claude calls you, not the other way around
- **Rich context** — Automatically includes git status, recent files, todos, and session context
- **Live tools** — Claude can read files and search code during the call
- **Transcripts** — Every call is saved as markdown for future reference

## Quick Start

```bash
# Install
pip install claude-code-voice

# Configure (requires Vapi account)
claude-code-voice setup

# Register your project
cd your-project
claude-code-voice register

# Have Claude call you
claude-code-voice call "let's debug the auth flow"
```

## Requirements

- Python 3.8+
- [Vapi](https://vapi.ai) account (handles the voice AI infrastructure)
- A phone number configured in Vapi

## Installation

### From PyPI

```bash
pip install claude-code-voice
```

### From Source

```bash
git clone <repo-url>
cd claude-code-voice
pip install -e .
```

### As a Claude Code Skill

```bash
# Symlink to your skills directory
ln -s /path/to/claude-code-voice ~/.claude/skills/call
```

Then use `/call` directly in Claude Code conversations.

## Commands

| Command | Description |
|---------|-------------|
| `setup` | Configure Vapi API key and phone numbers |
| `register` | Register the current project for voice calls |
| `call [topic]` | Have Claude call you (optionally with a specific topic) |
| `sync` | Download transcripts from completed calls |
| `history` | Show recent call history |
| `list` | List all registered projects |
| `status` | Show current configuration |
| `server` | Start the context server for live file access |

## Usage

### Initial Setup

```bash
claude-code-voice setup
```

You'll need:
1. Your Vapi API key from [dashboard.vapi.ai](https://dashboard.vapi.ai)
2. A phone number purchased or imported in Vapi
3. Your personal phone number (where Claude will call you)

### Register a Project

Navigate to any project directory and register it:

```bash
cd ~/projects/my-app
claude-code-voice register
```

This captures a snapshot of:
- Project type (Node.js, Python, Rust, Go, Swift, etc.)
- Description from README
- Git status and recent commits
- Recently modified files
- Current todos

### Make a Call

```bash
# General check-in
claude-code-voice call

# Specific topic
claude-code-voice call "brainstorm the new caching layer"
claude-code-voice call "help me understand the auth middleware"
claude-code-voice call "the tests are failing and I don't know why"
```

Or use the shorthand (first argument treated as topic):

```bash
claude-code-voice "let's discuss the API design"
```

### Using with Claude Code

When installed as a skill, you can invoke directly:

```
/call                        # Claude calls you
/call "debug the login bug"  # Call with specific topic
/call register               # Register current project
/call sync                   # Pull transcripts
/call status                 # Check configuration
```

### Sync Transcripts

After calls end, sync the transcripts:

```bash
claude-code-voice sync
```

Transcripts are saved as markdown files in `~/.claude-code-voice/transcripts/`.

## Context Server (Optional)

For live file reading and code search during calls, run the context server:

```bash
# Terminal 1: Start the server
claude-code-voice server --port 8765

# Terminal 2: Expose via tunnel
npx localtunnel --port 8765
# or
ngrok http 8765
```

Then update your config with the tunnel URL. This enables Claude to:
- Read specific files on demand
- Search code patterns across your project
- Get fresh context during long calls

## How It Works

```
┌─────────────────┐     ┌─────────────┐     ┌──────────────┐
│  Claude Code    │────▶│    Vapi     │────▶│  Your Phone  │
│   (context)     │     │  (voice AI) │     │              │
└─────────────────┘     └──────┬──────┘     └──────────────┘
                               │
                               ▼
                      ┌─────────────────┐
                      │ Context Server  │
                      │  (live tools)   │
                      └─────────────────┘
```

1. **Registration** captures project context (git status, files, todos)
2. **Call** creates a transient Vapi assistant with:
   - Full project context baked into the system prompt (fast, no latency)
   - Live tools for deeper lookups when needed
3. **Claude** converses naturally about your code
4. **Transcript** is saved after the call ends

### Session Context

When used within Claude Code, session context is automatically captured:
- What you've been working on
- Recent files touched in the session
- Current problem or question
- Any notes from the conversation

This means Claude on the phone knows exactly where you left off.

## Configuration

Config is stored in `~/.claude-code-voice/config.json`:

```json
{
  "vapi_api_key": "your-api-key",
  "vapi_phone_number_id": "phone-number-id",
  "vapi_phone_number": "+1234567890",
  "user_phone": "+0987654321",
  "server_url": "https://your-tunnel.loca.lt",
  "tool_ids": {}
}
```

When used as a Claude Code skill, data is stored in `~/.claude/skills/call/data/` instead.

## Troubleshooting

**"No Vapi API key configured"**
→ Run `claude-code-voice setup`

**"No phone number configured"**
→ Create or import a phone number in the [Vapi dashboard](https://dashboard.vapi.ai/phone-numbers)

**"Project not registered"**
→ Run `claude-code-voice register` in your project directory

**Call not connecting**
→ Check your phone number format includes country code (+1234567890)
→ Verify your Vapi account has available credits
→ For international calls, you may need to import a Twilio number

**Context server not working**
→ Ensure the tunnel URL is accessible
→ Run `claude-code-voice setup` again to update the server URL

## License

MIT
