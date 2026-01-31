# AGENTS.md - Guidelines for AI Coding Agents

This document provides instructions for AI agents working in this repository.

## Repository Overview

This is a **GitHub Profile Repository** (`taslabs-net/taslabs-net`). It's a special
repository that displays content on the user's GitHub profile page. The repository
contains:

- `README.md` - The main profile page content displayed on GitHub
- `.github/workflows/profile-summary.yml` - GitHub Actions automation
- `profile-summary-card-output/` - Auto-generated statistics cards (65 themes)

**Important:** This is a static content repository with no source code to build or
test. There are no package managers, dependencies, or test suites.

## Build/Lint/Test Commands

### There Are No Local Build Commands

This repository contains only Markdown, YAML, and auto-generated SVGs. There are no:
- Build commands
- Lint commands  
- Test commands
- Package dependencies

### GitHub Actions Workflow

The only automation is the profile summary cards generation:

```bash
# Manually trigger the workflow via GitHub CLI
gh workflow run "GitHub Profile Summary Cards"

# Check workflow status
gh run list --workflow=profile-summary.yml

# View workflow logs
gh run view --log
```

**Workflow Configuration:** `.github/workflows/profile-summary.yml`
- **Schedule:** Runs daily at midnight UTC (cron: `0 0 * * *`)
- **Manual trigger:** Supports `workflow_dispatch`
- **Action:** Uses `vn7n24fzkq/github-profile-summary-cards@v0.7.0`
- **Timezone offset:** UTC-5 (US Eastern)
- **Required secret:** `SUMMARY_GITHUB_TOKEN` (Personal Access Token)

## File Structure

```
.
├── README.md                          # Profile content (edit this)
├── AGENTS.md                          # This file
├── .github/
│   └── workflows/
│       └── profile-summary.yml        # Card generation automation
└── profile-summary-card-output/       # Auto-generated (DO NOT EDIT)
    ├── README.md                      # Theme preview index
    └── <theme>/                       # 65 theme directories
        ├── 0-profile-details.svg
        ├── 1-repos-per-language.svg
        ├── 2-most-commit-language.svg
        ├── 3-stats.svg
        ├── 4-productive-time.svg
        └── README.md
```

## Code Style Guidelines

### Markdown (README.md)

**Structure:**
- Use `##` for main sections
- Keep content concise and scannable
- Group related badges together with blank lines between groups

**Badges:**
- Use shields.io badge format: `![Name](https://img.shields.io/badge/...)`
- Use `style=flat` for consistent appearance
- Include appropriate logo from Simple Icons
- Format: `![Label](https://img.shields.io/badge/Label-Color?style=flat&logo=logo-name&logoColor=white)`

**Images:**
- Reference images via raw GitHub URLs
- Format: `https://raw.githubusercontent.com/taslabs-net/taslabs-net/main/path/to/image`
- Current theme: `gruvbox` (referenced in Stats section)

**Example badge:**
```markdown
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
```

### YAML (GitHub Actions)

**Workflow files:**
- Use 2-space indentation
- Include descriptive comments for non-obvious settings
- Pin action versions with full semver (e.g., `@v0.7.0`, `@v6`)
- Use GitHub secrets for sensitive values, never hardcode tokens

**Example:**
```yaml
name: Workflow Name

on:
  schedule:
    - cron: "0 0 * * *"  # Comment explaining schedule
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v6
      - uses: some/action@v1.0.0
        with:
          KEY: ${{ secrets.SECRET_NAME }}
```

## Working With This Repository

### Changing Profile Theme

The current theme is `gruvbox`. To change it:

1. Browse available themes at `profile-summary-card-output/README.md`
2. Edit `README.md` and replace `gruvbox` with the desired theme name
3. Update all 5 card URLs in the Stats section

Available themes include: 2077, algolia, apprentice, aura, aura_dark, ayu_mirage,
bear, blue_green, blueberry, buefy, calm, chartreuse_dark, city_lights, cobalt,
cobalt2, codeSTACKr, darcula, dark, date_night, default, discord_old_blurple,
dracula, github, github_dark, gotham, graywhite, great_gatsby, gruvbox,
highcontrast, jolly, material_palenight, monokai, moonlight, nightowl, nord_bright,
nord_dark, ocean_dark, omni, onedark, outrun, panda, radical, react, rose_pine,
slateorange, solarized, solarized_dark, tokyonight, transparent, vue, and more.

### Adding Technology Badges

1. Find the logo name at https://simpleicons.org/
2. Pick a hex color (often matches the technology's brand color)
3. Add badge to appropriate section in README.md

```markdown
![TechName](https://img.shields.io/badge/TechName-HEXCOLOR?style=flat&logo=logo-name&logoColor=white)
```

### Modifying the Workflow

If editing `.github/workflows/profile-summary.yml`:

- **USERNAME:** Uses `${{ github.repository_owner }}` (do not hardcode)
- **BRANCH_NAME:** Set to `main`
- **UTC_OFFSET:** Currently `-5` (US Eastern), adjust for local timezone
- **GITHUB_TOKEN:** Must use `secrets.SUMMARY_GITHUB_TOKEN` (PAT with repo scope)

## Do Not Edit

The following are auto-generated and should not be manually edited:

- `profile-summary-card-output/**/*.svg` - Generated by GitHub Action
- `profile-summary-card-output/**/README.md` - Generated theme previews
- `profile-summary-card-output/README.md` - Generated theme index

These files are regenerated daily and any manual changes will be overwritten.

## Version Control

This repository uses **jj** (Jujutsu) for version control, not git directly.

```bash
# Common jj commands
jj status          # Show working copy status
jj diff            # Show changes
jj new -m "msg"    # Create new commit with message
jj describe -m ""  # Update current commit message
jj git push        # Push to GitHub
```

## Commit Guidelines

- Keep commits atomic and focused
- Use present tense ("Add badge" not "Added badge")
- Reference specific changes ("Update theme to dracula", "Add Kubernetes badge")

## Common Tasks

| Task | Action |
|------|--------|
| Add new technology badge | Edit `README.md`, add badge line |
| Change card theme | Edit `README.md`, update 5 image URLs |
| Regenerate cards | Trigger workflow manually via GitHub |
| Change timezone | Edit `UTC_OFFSET` in workflow file |
| Update workflow action | Change version tag in workflow file |

## Secrets Required

- `SUMMARY_GITHUB_TOKEN`: Personal Access Token with `repo` scope
  - Required for the profile-summary-cards action
  - Set in repository Settings > Secrets > Actions

## Resources

- [GitHub Profile Summary Cards](https://github.com/vn7n24fzkq/github-profile-summary-cards)
- [Shields.io](https://shields.io/)
- [Simple Icons](https://simpleicons.org/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
