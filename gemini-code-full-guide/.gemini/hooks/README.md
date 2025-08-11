# Gemini Hooks Examples

This directory contains example hooks for Gemini that demonstrate how to add deterministic behavior to your AI coding workflow.

## What are Hooks?

Hooks are user-defined shell commands that execute at specific points in Gemini's lifecycle. They provide control over Gemini's behavior, ensuring certain actions always happen rather than relying on the AI to choose to run them.

## Files in this Directory

1. **format-after-edit.sh** - A PostToolUse hook that automatically formats code after file edits
2. **example-hook-config.json** - Example configuration showing how to set up various hooks

## How to Use These Hooks

### Option 1: Copy to Your Settings File

Copy the hooks configuration from `example-hook-config.json` to your Gemini settings:

**Project-specific** (`.gemini/settings.json`):
```bash
# Create settings file if it doesn't exist
touch .gemini/settings.json

# Add hooks configuration from example-hook-config.json
```

**User-wide** (`~/.gemini/settings.json`):
```bash
# Apply hooks to all Gemini sessions
cp example-hook-config.json ~/.gemini/settings.json
```

### Option 2: Use Individual Hooks

1. Copy the hook script to your project:
```bash
cp format-after-edit.sh /your/project/.gemini/hooks/
chmod +x /your/project/.gemini/hooks/format-after-edit.sh
```

2. Add to your settings.json:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": ".gemini/hooks/format-after-edit.sh"
          }
        ]
      }
    ]
  }
}
```

## Available Hook Events

- **PreToolUse**: Before tool execution (can block tools)
- **PostToolUse**: After successful tool completion
- **UserPromptSubmit**: When user submits a prompt
- **SubagentStop**: When a subagent completes
- **Stop**: When main agent finishes responding
- **Notification**: During system notifications
- **PreCompact**: Before context compaction
- **SessionStart**: At session initialization

## Creating Your Own Hooks

1. Write a shell script that:
   - Reads JSON input from stdin
   - Processes the input
   - Returns JSON output (empty `{}` for success)
   - Can return `{"action": "block", "message": "reason"}` to block operations

2. Make it executable:
```bash
chmod +x your-hook.sh
```

3. Add to settings.json with appropriate matcher and event

## Security Considerations

- Hooks execute arbitrary shell commands
- Always validate and sanitize inputs
- Use full paths to avoid PATH manipulation
- Be careful with file operations
- Test hooks thoroughly before deployment

## Debugging Hooks

Run Gemini with debug flag to see hook execution:
```bash
gemini --debug
```

This will show:
- Which hooks are triggered
- Input/output for each hook
- Any errors or issues

## Integration with Subagents

The example configuration includes a hook that integrates with the validation-gates subagent, demonstrating how hooks and subagents can work together for a more robust development workflow.