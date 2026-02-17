# Droid Receipts

Factory plugin that generates visual receipts for Droid sessions.

## Structure

```
droid-receipts/
├── .factory-plugin/
│   └── plugin.json       # Plugin manifest
├── hooks/
│   ├── hooks.json        # Hook configuration
│   └── generate-receipt.py
└── README.md
```

## How It Works

1. SessionEnd hook triggers when a Droid session ends
2. Hook reads session data from:
   - stdin (session_id, transcript_path, cwd)
   - `{session-id}.settings.json` (token counts, model)
   - transcript JSONL (timestamps)
3. Generates HTML/SVG receipt
4. Saves to `~/.factory/receipts/{session-id}.html`
5. Opens in browser (macOS)

## Output Format

Set via `DROID_RECEIPT_FORMAT` env var:
- `html` (default)
- `svg`
- `both`

## Testing

```bash
echo '{"session_id": "test", "transcript_path": "~/.factory/sessions/...", "cwd": "/path/to/project"}' | python3 hooks/generate-receipt.py
```

## Development

- Edit `hooks/generate-receipt.py` for receipt logic
- Edit `.factory-plugin/plugin.json` for metadata
- Edit `hooks/hooks.json` for hook configuration
- Cost calculation: `$1 per 1M tokens`
