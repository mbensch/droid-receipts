# Manual Worktrees

A Factory plugin that provides on-demand git worktree creation via a slash command.

## Installation

### From Marketplace (Recommended)

```bash
# Add the marketplace
droid plugin marketplace add https://github.com/mbensch/mb-ai-tools

# Install the plugin
droid plugin install manual-worktrees@mb-ai-tools
```

Or use the interactive UI: `/plugins` → Marketplaces → Add marketplace → enter URL

### From Local Directory (Development)

```bash
droid plugin marketplace add /path/to/mb-ai-tools
droid plugin install manual-worktrees@mb-ai-tools
```

## Commands

### `/worktree`

Creates a git worktree for the current session:

1. Detects the repository name and current branch
2. Creates a worktree at `{project}/../droid-worktrees/{repo}-{session-id}/`
3. Creates a new branch `droid/{session-id}` based on the current branch
4. Reports the worktree location for Droid to work in

**Usage:**
```
/worktree
```

## Benefits

- **On-Demand Control**: Create worktrees only when you need them
- **Session Isolation**: Each worktree has its own branch for clean history
- **Flexible**: Use alongside or instead of automatic worktree creation

## Plugin Structure

```
manual-worktrees/
├── .factory-plugin/
│   └── plugin.json
├── commands/
│   └── worktree.md
└── README.md
```

## Requirements

- Git 2.5+ (for worktree support)
- Works on macOS and Linux

## See Also

For automatic worktree creation on session start, see the `auto-worktrees` plugin.

## License

MIT
