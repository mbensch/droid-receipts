# Droid Receipts

A Factory plugin that generates visual receipts (HTML/SVG) for Droid sessions when they end.

Inspired by [claude-receipts](https://github.com/chrishutchinson/claude-receipts).

## Installation

### From Marketplace (Recommended)

```bash
# Add the marketplace
droid plugin marketplace add https://github.com/mbensch/mb-ai-tools

# Install the plugin
droid plugin install droid-receipts@mb-ai-tools
```

Or use the interactive UI: `/plugins` → Marketplaces → Add marketplace → enter URL

### From Local Directory (Development)

```bash
droid plugin marketplace add /path/to/mb-ai-tools
droid plugin install droid-receipts@mb-ai-tools
```

## How It Works

1. **SessionEnd Hook**: When a Droid session ends, the plugin is triggered
2. **Data Collection**: Reads session settings and transcript for token counts, model info, and timestamps
3. **Receipt Generation**: Creates an HTML receipt with modern styling
4. **Output**: Saves to `~/.factory/receipts/{session-id}.html` and opens in browser (macOS)

## Receipt Contents

- Factory branding
- Model badge prominently displayed
- Session ID
- Project location
- Date/time
- Duration
- Token breakdown:
  - Input tokens
  - Output tokens
  - Cache write tokens
  - Cache read tokens
- Total cost ($1 per 1M tokens)
- Star Wars-style droid cashier name (e.g., "R2-A3F", "GNK-7B2")

## Configuration

Set output format via environment variable `DROID_RECEIPT_FORMAT`:
- `html` (default) - Generate HTML receipt
- `svg` - Generate SVG receipt
- `both` - Generate both formats

## Example Output

```
================================
         FACTORY
      DROID RECEIPT
================================
[ Droid Core (GLM-5) ]

Location....................my-project
Session.....................abc12345
Date........................2026-02-16 22:31:45
Duration....................2m 15s

================================
ITEM                    QTY    PRICE
----------------------------------------
Input tokens          113,575      $0.11
Output tokens           2,287      $0.00
Cache write            96,562      $0.10
Cache read            578,832      $0.58
----------------------------------------
TOTAL                           $0.79
================================

SERVED BY: R2-A3F

Thank you for building!

factory.ai
================================
```

## Plugin Structure

```
droid-receipts/
├── .factory-plugin/
│   └── plugin.json       # Plugin manifest
├── hooks/
│   ├── hooks.json        # Hook configuration
│   └── generate-receipt.py
└── README.md
```

## License

MIT
