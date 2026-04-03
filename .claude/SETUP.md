# Claude Code Project Setup Guide

This is a standard Claude Code project structure. Follow these steps to customize it for your project.

## 1. Update Project Metadata

- [ ] Edit `package.json`:
  - Change `name` to your project name
  - Update `description`
  - Add dependencies as needed

- [ ] Edit `README.md`:
  - Replace "Project Name" with your actual project name
  - Update the Quick Start section
  - Add relevant documentation

## 2. Configure Claude Code

- [ ] Edit `.claude/settings.json`:
  - Add hooks for automated behaviors
  - Configure memory directories
  - Set up agent management

- [ ] Edit `.claude/launch.json`:
  - Update dev server configuration
  - Match your build tool (npm, yarn, pnpm, etc.)
  - Set correct port

- [ ] Create/Update `.claude/CLAUDE.md`:
  - Document project rules and guidelines
  - Define agent responsibilities
  - Specify coding standards

## 3. Set Up Project Structure

- [ ] Create source files in `src/`:
  - `components/` for reusable UI components
  - `utils/` for helper functions
  - `types/` for TypeScript types

- [ ] Add static files to `public/`:
  - HTML templates
  - Images
  - Stylesheets

- [ ] Add data/docs to `data/`:
  - JSON data files
  - Markdown documentation
  - Configuration files

## 4. Create Agents (if needed)

- [ ] Create agent definitions in `.claude/agents/`:
  - Copy `TEMPLATE.md` for each agent
  - Define agent responsibilities
  - Document dependencies and handoffs

## 5. Create Skills (if needed)

- [ ] Add project-specific skills in `.claude/skills/`:
  - Copy `TEMPLATE.md` for each skill
  - Document when and how to use
  - Include examples

## 6. Set Up Memory System

- [ ] Create memory files in `.claude/memories/`:
  - `user_*.md` for team/user context
  - `feedback_*.md` for project guidance
  - `project_*.md` for timeline/goals
  - `reference_*.md` for external resources

- [ ] Link all memories in `.claude/MEMORY.md`

## 7. Initialize Version Control

```bash
git init
git add .
git commit -m "Initial commit: Standard Claude Code project structure"
```

## 8. Install Dependencies

```bash
npm install
# or: yarn install, pnpm install
```

## 9. Start Development

See `.claude/launch.json` for how to start your dev server.

---

For more information on Claude Code features, see `/help` in Claude Code or visit the documentation.
