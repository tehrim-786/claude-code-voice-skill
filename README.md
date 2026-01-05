# Claude Code Voice

> **Voice conversations with Claude Opus 4.5 about your code.**

Talk through problems, brainstorm ideas, or get a code review - all over the phone.

## Quick Start

```bash
# Install
pip install claude-code-voice

# One-time setup (need Vapi account: https://dashboard.vapi.ai)
claude-code-voice setup

# Register a project
cd your-project
claude-code-voice register

# Start the service
claude-code-voice start
```

Now you can:
- **Have Claude call you**: `claude-code-voice call "debug the auth flow"`
- **Call Claude**: Dial your Vapi number and Claude answers with your project loaded

## Features

| Feature | Description |
|---------|-------------|
| **Opus 4.5** | Best-in-class reasoning for technical discussions |
| **Project context** | Git status, recent files, todos loaded automatically |
| **Live tools** | Claude can read files and search code during calls |
| **Auto-transcripts** | Every call saved as markdown |
| **Personalized** | Claude greets you by name |

## Commands

```bash
claude-code-voice setup              # Configure API key, phone, name
claude-code-voice register           # Register current project
claude-code-voice start              # Start server + tunnel (easy mode)
claude-code-voice call [topic]       # Have Claude call you
claude-code-voice status             # Check configuration
claude-code-voice config name <name> # Update your name
```

## Requirements

- Python 3.8+
- [Vapi](https://vapi.ai) account with API key
- Vapi phone number (~$2/month)
- Node.js (for localtunnel)

## How It Works

```
You ──call──▶ Vapi Phone ──webhook──▶ Your Server ──context──▶ Claude Opus 4.5
                                            │
                                      reads your code
```

1. **Setup** stores your Vapi credentials
2. **Register** snapshots project context (git, files, todos)
3. **Start** runs server + tunnel, configures Vapi
4. **Call** - Claude has full context about your project

## As a Claude Code Skill

For `/call` in Claude Code:

```bash
git clone https://github.com/abracadabra50/claude-code-voice-skill.git
ln -s /path/to/claude-code-voice-skill ~/.claude/skills/call
```

Then use `/call` directly in conversations.

## License

MIT
