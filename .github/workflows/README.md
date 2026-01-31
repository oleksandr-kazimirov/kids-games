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

---

## How Git Releases Work

### What is a Release?

A **release** is a snapshot of your code at a specific point, marked with a **tag** (like `v1.0.0`). Think of it as:

```
Commits:  A → B → C → D → E → F → G  (continuous development)
                      ↑           ↑
Tags:                v1.0.0     v1.1.0  (stable milestones)
```

### Tags vs Releases

| Concept | What it is |
|---------|------------|
| **Tag** | A label pointing to a specific commit (Git feature) |
| **Release** | A GitHub feature that wraps a tag with title, notes, and downloadable files |

### Creating a Release

**Using GitHub CLI:**
```bash
# Basic release
gh release create v1.0.0

# With title and notes
gh release create v1.1.0 --title "v1.1.0 - Bug fixes" --notes "Fixed voice button issue"

# From a specific commit
gh release create v1.2.0 --target abc123
```

**On GitHub website:**
1. Go to repository → Releases → "Create a new release"
2. Enter tag (e.g., `v1.0.0`)
3. Add title and description
4. Click "Publish release"

### Version Numbering (Semantic Versioning)

Format: `vMAJOR.MINOR.PATCH`

| Change type | Example | When to use |
|-------------|---------|-------------|
| **MAJOR** | v1.0.0 → v2.0.0 | Breaking changes |
| **MINOR** | v1.0.0 → v1.1.0 | New features (backward compatible) |
| **PATCH** | v1.0.0 → v1.0.1 | Bug fixes |

### Useful Commands

```bash
# List all releases
gh release list

# View a specific release
gh release view v1.0.0

# Delete a release
gh release delete v1.0.0

# List all tags
git tag

# See which commit a tag points to
git show v1.0.0
```

### Why Use Releases?

1. **Stable deployments** - Only deploy tested, approved versions
2. **Rollback** - Easy to redeploy a previous version
3. **Changelog** - Document what changed in each version
4. **Distribution** - Users can download specific versions
