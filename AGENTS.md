# Droid Marketplace - Agent Guide

This guide explains how to use Droid to work with the droid-plugins marketplace repository.

## Working with This Repository

Droid can help you with:

**Creating new plugins:**
- Generate plugin structure with proper files (.factory-plugin, hooks.json, plugin.json)
- Create hook scripts in Python or other languages
- Write skills documentation in Markdown format
- Test plugin functionality

**Managing the marketplace:**
- Add new plugins to marketplace.json
- Update plugin versions and descriptions
- Validate plugin manifests
- Test plugin installation

**Troubleshooting plugins:**
- Debug hook execution failures
- Check marketplace.json syntax
- Verify plugin structure matches specification
- Test hooks with sample data

**Documentation tasks:**
- Write plugin README files
- Create usage examples
- Document hook interfaces
- Maintain developer guides

## Repository Structure

```
droid-plugins/
├── .factory-plugin/
│   └── marketplace.json    # Marketplace manifest
├── plugins/
│   ├── droid-receipts/      # Example plugin
│   │   ├── .factory-plugin/
│   │   │   └── plugin.json  # Plugin metadata
│   │   ├── hooks/            # Hook scripts
│   │   │   └── generate-receipt.py
│   │   ├── hooks.json        # Hook configuration
│   │   ├── README.md         # Plugin docs
│   │   └── AGENTS.md         # Plugin dev guide
└── README.md                 # Marketplace docs
```

## Available Tasks

Ask Droid to:

1. **"Add a plugin called X that does Y"** - Creates new plugin structure
2. **"Update the marketplace.json for my new plugin"** - Registers plugin
3. **"Debug why my SessionEnd hook isn't firing"** - Troubleshoots issues
4. **"Create a README for the droid-receipts plugin"** - Writes documentation
5. **"Check if my plugin.json is valid"** - Validates manifest syntax
6. **"Write test data for my hook"** - Generates sample JSON for testing
7. **"Refactor generate-receipt.py to use functions"** - Code improvements

## Plugin Types

1. **Hooks** - Executed on events (SessionEnd, PreCommit, etc.)
   - Defined in `hooks.json`
   - Scripts in `hooks/` directory

2. **Skills** - On-demand capabilities invoked via slash commands
   - Defined in `SKILL.md` files
   - Can be simple text-based or complex AI-driven

3. **Droids** - Custom autonomous agents for specific use cases
   - Reusable configurations with specialized tools and behaviors

## Quick Start

To create a new plugin:

```
Use Droid to:
1. Create the directory structure under plugins/
2. Set up .factory-plugin/plugin.json with name, version, description
3. Add hooks.json or skills directory as needed
4. Create hook scripts or skill definitions
5. Add plugin to marketplace.json
6. Test by installing locally
7. Commit and push
```

## Testing Plugins

Locally test plugin before pushing:

```bash
# Add as local marketplace
droid plugin marketplace add /path/to/this/repo

# Install plugin
droid plugin install your-plugin@droid-plugins

# Test functionality
# Verify hooks fire
# Check logs in ~/.factory/logs/
```

## Common Issues

**Hook not executing:**
- Check file permissions (chmod +x)
- Verify hooks.json path references
- Check Factory logs for errors
- Test with sample JSON input

**Plugin not appearing:**
- Verify marketplace.json syntax
- Check source path is correct
- Reload marketplace list with `/plugins`

**Marketplace install fails:**
- Verify git history contains plugin.json
- Check commit hash is correct
- Ensure README.md exists in plugin directory
