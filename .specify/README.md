# Spec-Kit Framework

This directory contains the Spec-Kit framework files for specification-driven development.

## Source

Based on [GitHub Spec-Kit](https://github.com/github/spec-kit) - A toolkit to help you get started with Spec-Driven Development.

## What's Included

- `scripts/` - Automation scripts for creating features, plans, and tasks
- `templates/` - Document templates for specs, plans, and tasks
- `memory/` - Project constitution and principles

## Usage

These files work with the custom Cursor IDE commands in `.cursor/commands/`:

- `/specify` - Create new feature specification
- `/plan` - Generate implementation plan
- `/implement` - Execute implementation

## Updating the Framework

To pull the latest version of Spec-Kit:

```bash
# Clone spec-kit
git clone https://github.com/github/spec-kit.git /tmp/spec-kit

# Update framework files
cp -r /tmp/spec-kit/scripts/* .specify/scripts/
cp -r /tmp/spec-kit/templates/* .specify/templates/
cp -r /tmp/spec-kit/memory/* .specify/memory/
cp -r /tmp/spec-kit/.cursor/* .cursor/

# Clean up
rm -rf /tmp/spec-kit

# Commit updates
git add .specify/ .cursor/
git commit -m "chore: update Spec-Kit framework to latest version"
```

## Learn More

- Documentation: https://github.com/github/spec-kit
- Issues: https://github.com/github/spec-kit/issues
- Contributing: https://github.com/github/spec-kit/blob/main/CONTRIBUTING.md
