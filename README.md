# YDBD CLI Help Editor

Interactive web-based editor for reviewing and proposing changes to the `ydbd` CLI help messages.

## Files

```
cli-help-editor/
├── index.html           # Main application
├── current-state.json   # Current state from ydbd source code (read-only reference)
├── target-state.json    # Proposed changes (edit this to update global target)
└── README.md            # This file
```

## Usage

### Running Locally

Due to browser security restrictions (CORS), you need to run a local web server:

```bash
cd cli-help-editor

# Python 3
python -m http.server 8000

# Then open http://localhost:8000
```

### Workflow for Reviewers

1. Open the page
2. See the current diff (current vs target) highlighted in colors
3. Browse commands, review proposed changes
4. Make additional edits if needed
5. Click "Export My Changes" to download JSON
6. Send the JSON file to the maintainer

### Workflow for Maintainer

To update the global target with received changes:

1. Review the received JSON file
2. Merge changes into `target-state.json`:

```json
{
  "version": "1.0",
  "author": "pnv1",
  "updatedAt": "2026-02-25",
  "changes": {
    "format-info": {
      "visible": true,
      "description": "Read PDisk format information"
    },
    "admin.blobstorage.config.init": {
      "description": "Initialize BS config from YAML"
    }
  },
  "comments": {
    "format-info": "Useful for disk debugging"
  }
}
```

3. Commit and push to update the shared page

### Updating Current State

When ydbd CLI changes, regenerate `current-state.json`:

1. Review the source code in `ydb/core/driver_lib/cli_utils/`
2. Update the JSON structure accordingly
3. Commit the new `current-state.json`

## Deploying to GitHub Pages

### Quick Start

1. Create a new repository on GitHub

2. Copy all files:
   ```bash
   git clone https://github.com/YOUR_USERNAME/ydbd-cli-help-editor.git
   cd ydbd-cli-help-editor
   cp /path/to/cli-help-editor/* .
   git add .
   git commit -m "YDBD CLI Help Editor"
   git push origin main
   ```

3. Enable GitHub Pages:
   - Go to Settings → Pages
   - Source: "Deploy from a branch"
   - Branch: `main`, folder: `/ (root)`
   - Save

4. Access at: `https://YOUR_USERNAME.github.io/ydbd-cli-help-editor/`

## Color Legend

| Color | Meaning |
|-------|---------|
| Gray text | Hidden command/option (in current state) |
| Green background | Will become visible (hidden → visible) |
| Red background | Will become hidden (visible → hidden) |
| Yellow background | Description changed |
| Blue border | Your local changes (not yet exported) |

## Data Format

### current-state.json

```json
{
  "version": "1.0",
  "source": "ydbd source code",
  "commands": {
    "admin": {
      "name": "admin",
      "aliases": [],
      "description": "YDB management",
      "visible": true,
      "subcommands": { ... },
      "options": { ... }
    }
  }
}
```

### target-state.json

```json
{
  "version": "1.0",
  "author": "pnv1",
  "changes": {
    "command.path": {
      "visible": true,
      "description": "New description"
    }
  },
  "comments": {
    "command.path": "Discussion note"
  }
}
```

### Exported changes (from users)

```json
{
  "version": "1.0",
  "exportedAt": "2026-02-25T10:00:00Z",
  "changes": { ... },
  "comments": { ... }
}
```

## Browser Support

Works in all modern browsers. Local changes are stored in `localStorage`.
