# GitHub Actions Workflows

## deploy.yml - GitHub Pages Deployment

Deploys the site to GitHub Pages only when a new release is published.

### Triggers

```yaml
on:
  release:
    types: [published]  # Runs when you create a release
  workflow_dispatch:     # Allows manual trigger from Actions tab
```

### Permissions

```yaml
permissions:
  contents: read    # Read repository files
  pages: write      # Write to GitHub Pages
  id-token: write   # Required for Pages deployment authentication
```

### Steps Explained

| Step | Action | Purpose |
|------|--------|---------|
| **Checkout** | `actions/checkout@v4` | Downloads repo code to the runner |
| **Setup Pages** | `actions/configure-pages@v4` | Configures Pages URL and settings |
| **Upload artifact** | `actions/upload-pages-artifact@v3` | Packages files into deployable archive |
| **Deploy** | `actions/deploy-pages@v4` | Publishes to GitHub Pages servers |

### How to Deploy

**Option 1: Create a release (recommended)**
```bash
gh release create v1.2.0 --title "v1.2.0 - New features" --notes "What changed..."
```

**Option 2: Manual deploy**
1. Go to Actions tab on GitHub
2. Select "Deploy to GitHub Pages"
3. Click "Run workflow"

### Live URL

After deployment: https://oleksandr-kazimirov.github.io/kids-games/multiplication/multiplication-game.html
