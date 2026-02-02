# Publishing to NPM

This guide explains how to publish the PAIRL Claude Code skill to npm.

## Pre-Publishing Checklist

- [x] No sensitive data (API keys, secrets, tokens) in code
- [x] `.gitignore` and `.npmignore` configured
- [x] `package.json` has correct metadata
- [x] `README.md` is complete with examples
- [x] Examples in `examples/` directory
- [x] License file (Apache 2.0) present
- [x] Git repository initialized and pushed to GitHub

## Step-by-Step Publishing

### 1. Test Package Locally

Before publishing, test the package contents:

```bash
# See what will be published
npm pack --dry-run

# Create a tarball to inspect
npm pack

# Extract and inspect
tar -xzf pairl-claude-skill-1.1.0.tgz
ls -R package/
```

### 2. Login to NPM

```bash
# Login to npm (creates ~/.npmrc with auth token)
npm login

# Verify login
npm whoami
```

### 3. Verify Package Version

Check that version in `package.json` is correct:

```json
{
  "name": "pairl-claude-skill",
  "version": "1.1.0",
  ...
}
```

### 4. Publish to NPM

```bash
# Publish publicly
npm publish --access public

# Expected output:
# + pairl-claude-skill@1.1.0
```

### 5. Verify Publication

```bash
# Check package page
npm view pairl-claude-skill

# Test installation
npm install pairl-claude-skill
```

### 6. Update Repository

```bash
# Tag the release
git tag -a v1.1.0 -m "Release v1.1.0 - PAIRL Claude Code Skill"

# Push with tags
git push origin main --tags
```

### 7. Create GitHub Release

Go to: `https://github.com/dwehrmann/PAIRL-claude-skill/releases/new`

- **Tag**: `v1.1.0`
- **Title**: `PAIRL Claude Code Skill v1.1.0`
- **Description**:

```markdown
# PAIRL Claude Code Skill v1.1.0

Claude Code skill for working with PAIRL v1.1 (Protocol for Agent Intermediate Representation Lite).

## Features

- Generate PAIRL messages from natural language
- Validate PAIRL syntax and compliance
- Convert conversations to PAIRL format
- Explain PAIRL messages in human-readable form
- 70-90% token reduction vs natural language

## Installation

```bash
claude-code skill install pairl-claude-skill
```

Or via npm:

```bash
npm install pairl-claude-skill
```

## Quick Start

```
/pairl generate: Hi this is Bob. Message Jonas and ask for the report on Dynamic Mondays. I need it by tomorrow, Feb. 3rd, 2026
```

See [README.md](https://github.com/dwehrmann/PAIRL-claude-skill#readme) for full documentation.

## Resources

- [PAIRL Specification](https://github.com/dwehrmann/PAIRL)
- [Examples](./examples/)
- [Issues](https://github.com/dwehrmann/PAIRL-claude-skill/issues)
```

## Updating the Package

When making updates:

1. **Update version** in `package.json`:
   ```json
   "version": "1.1.1"  // or 1.2.0, or 2.0.0
   ```

2. **Update CHANGELOG.md**:
   ```markdown
   ## [1.1.1] - 2026-02-02
   ### Fixed
   - Bug in validation logic
   ```

3. **Commit changes**:
   ```bash
   git add .
   git commit -m "chore: bump to v1.1.1"
   ```

4. **Publish**:
   ```bash
   npm publish --access public
   ```

5. **Tag release**:
   ```bash
   git tag -a v1.1.1 -m "Release v1.1.1"
   git push origin main --tags
   ```

## Versioning

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR** (2.0.0): Breaking changes
- **MINOR** (1.2.0): New features, backwards-compatible
- **PATCH** (1.1.1): Bug fixes, backwards-compatible

## NPM Package Links

After publishing:

- **Package page**: `https://www.npmjs.com/package/pairl-claude-skill`
- **Install stats**: `https://npm-stat.com/charts.html?package=pairl-claude-skill`

## Troubleshooting

### "You do not have permission to publish"

Make sure you're logged in and the package name is available:

```bash
npm whoami
npm search pairl-claude-skill
```

### "Package name too similar to existing package"

If `pairl-claude-skill` is taken, try:
- `@yourusername/pairl-claude-skill` (scoped package)
- `claude-skill-pairl`
- `pairl-skill`

Update `package.json` name accordingly.

### Files missing from published package

Check `.npmignore` - it should NOT exclude necessary files.

Files automatically included by npm:
- `package.json`
- `README.md`
- `LICENSE`
- Files listed in `"files"` array

## Support

For issues with publishing:
- [NPM Support](https://www.npmjs.com/support)
- [NPM Documentation](https://docs.npmjs.com/)

For skill issues:
- [GitHub Issues](https://github.com/dwehrmann/PAIRL-claude-skill/issues)
