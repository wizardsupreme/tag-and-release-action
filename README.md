# tag-and-release-action

Automated version tagging and release based on commit messages

## Overview

This GitHub Action automatically creates tags and releases based on semantic commit messages. It analyzes commit messages to determine the appropriate version bump (major, minor, or patch) following semantic versioning principles.

## Usage

Add this action to your workflow file:

```yaml
name: Release

on:
  push:
    branches: [ main ]

permissions:
  contents: write  # Needed for creating tags and releases

jobs:
  release:
    runs-on: ubuntu-minimal
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      
      - name: Create Tag and Release
        uses: wizardsupreme/tag-and-release-action@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `github-token` | GitHub token with permissions to create tags and releases | Yes | |
| `commit-message` | Override commit message for version bump detection | No | Last commit message |
| `base-ref` | Base reference to compare against | No | Latest tag |

## How It Works

The action determines the version bump type based on commit message prefixes:

- **Major version bump**: Commits containing `BREAKING CHANGE:` or `MAJOR:`
- **Minor version bump**: Commits containing `FEAT:`, `FEATURE:`, or `MINOR:`
- **Patch version bump**: All other commits

## Examples

### Basic Usage

```yaml
- name: Create Tag and Release
  uses: wizardsupreme/tag-and-release-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

### With Custom Commit Message

```yaml
- name: Create Tag and Release
  uses: wizardsupreme/tag-and-release-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    commit-message: "FEAT: Add new feature"
```

## Self-Releasing

This repository uses its own action to manage releases. See the workflow configuration in `.github/workflows/release.yml`.

## License

Apache License 2.0
