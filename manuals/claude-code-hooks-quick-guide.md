# Claude Code Hooks & Context Engineering: Quick Guide
*5-minute read for immediate productivity gains*

## What Are Claude Code Hooks?

**Hooks = Automatic triggers that execute your commands when Claude does specific things.**

Think of them as "if this, then that" rules for your AI coding assistant.

## Quick Setup (1 minute)

1. Create `.claude/settings.toml` in your project:
```toml
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.py", "*.ts", "*.js"]
command = "echo '✅ Auto-formatting...' && prettier --write $CLAUDE_FILE_PATHS"
```

2. That's it! Claude will now auto-format every file it edits.

## The 4 Hook Types

| Event | When It Fires | Best For |
|-------|---------------|----------|
| **PreToolUse** | Before Claude acts | Validation, blocking dangerous operations |
| **PostToolUse** | After Claude acts | Formatting, testing, committing |
| **Notification** | When Claude needs attention | Alerts, logging |
| **Stop** | When Claude finishes | Cleanup, final checks |

## Context Engineering Power Patterns

### 1. The Auto-Quality Pattern
```toml
# After any code edit: format → lint → test
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
command = "npm run format && npm run lint && npm test"
```

### 2. The Context Preservation Pattern
```toml
# Save important context before it's lost
[[hooks]]
event = "Stop"
command = "echo '$CLAUDE_RESPONSE' >> .claude/context-history.log"
```

### 3. The Safety Gate Pattern
```toml
# Block production changes
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["**/production/**"]
command = "echo '🚫 BLOCKED: No production edits!' && exit 2"
```

### 4. The Smart Context Pattern
```toml
# Automatically update CLAUDE.md with new patterns
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.py"]
command = "python scripts/extract_patterns.py $CLAUDE_FILE_PATHS >> CLAUDE.md"
```

## Environment Variables You Get

- `$CLAUDE_FILE_PATHS` - Files being modified
- `$CLAUDE_TOOL_NAME` - What tool is being used
- `$CLAUDE_NOTIFICATION` - Notification content
- `$CLAUDE_RESPONSE` - Claude's output

## Real-World Examples

### TypeScript Project
```toml
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.ts", "*.tsx"]
command = "npx tsc --noEmit && npx eslint --fix $CLAUDE_FILE_PATHS"
```

### Python Project
```toml
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
file_paths = ["*.py"]
command = "black $CLAUDE_FILE_PATHS && mypy $CLAUDE_FILE_PATHS && pytest"
```

### Documentation Auto-Update
```toml
[[hooks]]
event = "PostToolUse"
[hooks.matcher]
tool_name = "edit_file"
command = "python scripts/update_docs.py && git add -A && git commit -m 'Auto-update docs'"
```

## Context Engineering Best Practices

1. **Layer Your Hooks**: Start simple, add complexity gradually
   ```toml
   # Level 1: Just format
   command = "prettier --write $CLAUDE_FILE_PATHS"
   
   # Level 2: Format + lint
   command = "prettier --write $CLAUDE_FILE_PATHS && eslint $CLAUDE_FILE_PATHS"
   
   # Level 3: Full pipeline
   command = "npm run format && npm run lint && npm test && npm run build"
   ```

2. **Use Exit Codes**: 
   - Exit 0 = Success
   - Exit 2 = Block Claude and show error
   - Other = Show error but continue

3. **Background Tasks**: Add `run_in_background = true` for non-blocking operations

4. **Combine with CLAUDE.md**: Hooks handle automation, CLAUDE.md handles knowledge

## Advanced: JSON Control
```toml
[[hooks]]
event = "PreToolUse"
command = '''
if [[ "$CLAUDE_FILE_PATHS" == *"critical"* ]]; then
  echo '{"continue": false, "stopReason": "Critical file protection enabled"}'
fi
'''
```

## Security Quick Tips

- Always quote variables: `"$VAR"` not `$VAR`
- Use absolute paths: `/usr/bin/prettier` not `prettier`
- Validate inputs: Check for `..` in paths
- Test hooks in safe environment first

## Common Hook Recipes

### Auto-commit After Changes
```toml
[[hooks]]
event = "Stop"
command = "git add -A && git commit -m 'Claude: $CLAUDE_LAST_MESSAGE' || true"
```

### Notify When Long Task Done
```toml
[[hooks]]
event = "Notification"
command = "osascript -e 'display notification \"$CLAUDE_NOTIFICATION\" with title \"Claude Code\"'"
```

### Enforce Code Standards
```toml
[[hooks]]
event = "PreToolUse"
[hooks.matcher]
tool_name = "edit_file"
command = "scripts/check-standards.sh $CLAUDE_FILE_PATHS || exit 2"
```

## The Context Engineering Mindset

**Don't tell Claude what to do repeatedly. Set up hooks once, and let automation handle it.**

Before hooks: "Please format this file, run tests, and check types"
With hooks: "Update the user service" (everything else happens automatically)

## Start Now

1. Copy any example above
2. Create `.claude/settings.toml`
3. Paste and modify for your needs
4. Save 10+ minutes per hour

---

*Remember: Hooks turn Claude from a coding assistant into a coding system. Use them.*