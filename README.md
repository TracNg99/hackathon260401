# Project Name

Replace this with your project description.

## Quick Start

```bash
# Install dependencies
npm install

# Start development server (if applicable)
npm run dev
```

## Project Structure

```
project-root/
├── .claude/              # Claude Code configuration
│   ├── CLAUDE.md        # Project rules and guidelines
│   ├── MEMORY.md        # Memory index for cross-session context
│   ├── settings.json    # Claude Code harness configuration
│   ├── launch.json      # Dev server startup configuration
│   ├── rules/           # Shared team/project rules
│   ├── memories/        # Persistent session memory files
│   ├── agents/          # Agent definitions and configs
│   ├── skills/          # Project-specific skills
│   └── templates/       # Reusable templates
├── src/                 # Source code
│   ├── components/      # Reusable components
│   ├── utils/          # Utility functions
│   └── types/          # TypeScript types
├── public/             # Static files (HTML, images)
├── data/               # Data files and documentation
├── assets/             # Design files and images
├── tests/              # Test files
└── docs/               # Additional documentation
```

## Development

See `.claude/CLAUDE.md` for detailed project guidelines and rules.

## Memory System

This project uses Claude's persistent memory system. See `.claude/MEMORY.md` for the memory index.

## Skills

Custom skills for this project are located in `.claude/skills/`. See individual skill files for details.

## Configuration

- **Claude Code Settings**: `.claude/settings.json`
- **Dev Server**: `.claude/launch.json`
- **Project Rules**: `.claude/CLAUDE.md`

## Next Steps

1. Update this README with your actual project details
2. Configure `.claude/settings.json` for your workflow
3. Update `.claude/CLAUDE.md` with project-specific guidelines
4. Add skills in `.claude/skills/` as needed
