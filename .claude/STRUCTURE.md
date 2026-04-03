# Standard Claude Code Directory Structure

## Overview

This project follows the standard Claude Code structure for maximum compatibility and team collaboration.

## Directory Breakdown

### `.claude/` — Claude Code Configuration
- **CLAUDE.md**: Project rules, guidelines, and coding standards
- **MEMORY.md**: Index of persistent session memories
- **SETUP.md**: Initial setup and configuration checklist
- **STRUCTURE.md**: This file — directory structure reference
- **settings.json**: Claude Code harness configuration
- **launch.json**: Development server startup configuration
- **rules/**: Shared team/organization rules
- **memories/**: Persistent memory files (user, feedback, project, reference)
- **agents/**: Agent definitions and configurations
- **skills/**: Project-specific skills and workflows
- **templates/**: Reusable templates and boilerplates

### `src/` — Source Code
- **components/**: Reusable UI components
- **utils/**: Utility functions and helpers
- **types/**: TypeScript type definitions

### `public/` — Static Assets
- HTML pages
- Images
- Stylesheets
- Static resources

### `data/` — Data & Documentation
- JSON data files
- Markdown documentation
- Configuration data
- Project documentation

### `assets/` — Design Files
- Images
- Icons
- Design files (Figma exports, etc.)

### `tests/` — Test Files
- Unit tests
- Integration tests
- Test utilities

### Root Files
- **.gitignore**: Git ignore rules
- **README.md**: Project overview and getting started
- **package.json**: Project metadata and dependencies

## Best Practices

1. **Keep `.claude/` organized**: Each subdirectory has a specific purpose
2. **Use MEMORY.md**: Link all persistent memories for cross-session context
3. **Document in CLAUDE.md**: Project-specific rules go here, not scattered
4. **Create agents for complex workflows**: Use `.claude/agents/` for agent definitions
5. **Share skills**: Custom skills in `.claude/skills/` are project-specific
6. **Maintain README**: Keep it updated with current project status

## Adding New Files

When adding new files or directories:
1. Determine the category (code, config, data, docs)
2. Place in the appropriate directory
3. Update `.claude/STRUCTURE.md` if needed
4. Add to `.gitignore` if sensitive

## Git Integration

- Commit `.claude/` directory regularly for team sync
- Exclude `node_modules/`, `.env`, and build outputs in `.gitignore`
- Use `.claude/skills/` and `.claude/agents/` for version control

---

Last updated: 2026-04-03
