# BuildMVPFast Project Docs

Generate professional project documentation directly from Claude Code. Create PRDs, technical specs, database schemas, and more with a single command.

**The open-source version of [ProjectDocsEngine.com](https://projectdocsengine.com)**

## Features

- **15 Document Types**: 6 core + 9 custom document types
- **Two Modes**: Conversational wizard for new projects, or analyze existing codebases
- **Cross-Referencing**: Consistent FR-X, TS-X, UF-X numbering across documents
- **Command Mode Updates**: Keep docs in sync as your project evolves
- **Professional Quality**: 1,500-3,000 words per document

## Installation

### Option 1: npx skills (Recommended)

Install skills directly using [npx skills](https://skills.sh):

```bash
# Install all skills
npx skills add r-sri-ram/buildmvpfast-project-docs

# Install specific skills only
npx skills add r-sri-ram/buildmvpfast-project-docs --skill project-docs
npx skills add r-sri-ram/buildmvpfast-project-docs --skill add-doc

# List available skills
npx skills add r-sri-ram/buildmvpfast-project-docs --list
```

This automatically installs to your `~/.claude/skills/` directory.

### Option 2: Manual Installation

Clone and copy skills to your Claude Code directory:

```bash
# Clone the repository
git clone https://github.com/r-sri-ram/buildmvpfast-project-docs.git

# Copy skills to Claude Code skills directory
cp -r buildmvpfast-project-docs/skills/* ~/.claude/skills/
```

## Quick Start

### Generate Documentation for a New Project

```
/project-docs new
```

Claude will guide you through a conversation to gather:
- Project name and description
- Project type (SaaS, e-commerce, mobile, etc.)
- Target audience
- Main features
- Tech stack preferences
- Business model

Then generates 6 core documents automatically.

### Document an Existing Codebase

```
/project-docs analyze
```

Claude analyzes your codebase to extract:
- Tech stack from package.json, requirements.txt, etc.
- Database models from migrations/schemas
- API endpoints from routes
- Features from components and services

### Add Custom Documents

```
/add-doc api          # API Specification
/add-doc testing      # QA & Testing Strategy
/add-doc integration  # Integration Architecture
/add-doc performance  # Performance Optimization
/add-doc legal        # Legal & Compliance
/add-doc marketing    # Marketing & SEO
/add-doc accessibility # Accessibility Guidelines
/add-doc monetization  # Monetization Plan
/add-doc governance    # Governance Plan
```

### Update Existing Docs

```
/update-docs "Add Stripe payment integration"
/update-docs "Add user authentication with OAuth"
/update-docs "Add real-time notifications feature"
```

## Document Types

### 6 Core Documents (Always Generated)

| Document | Description | Prefix |
|:---------|:------------|:-------|
| PRD | Product Requirements Document | FR-, NFR- |
| Tech Stack | Technical Architecture | TS- |
| User Flows | User Journeys & Flows | UF- |
| Database Schema | Database Design | DB- |
| Design Guidelines | Design System | DG- |
| Task List | Development Tasks | Task- |

### 9 Custom Documents (Optional)

| Document | Description |
|:---------|:------------|
| API Specification | REST/GraphQL endpoints |
| QA Testing | Testing strategy & cases |
| Integration Architecture | Third-party integrations |
| Performance Optimization | Performance guidelines |
| Legal Compliance | Privacy, GDPR, terms |
| Marketing SEO | Marketing strategy |
| Accessibility Guidelines | WCAG compliance |
| Monetization Plan | Revenue models |
| Governance Plan | Project governance |

## Output Structure

Documents are saved to `./docs/`:

```
docs/
├── README.md              # Index of all documentation
├── 01-prd.md              # Product Requirements
├── 02-tech-stack.md       # Technical Architecture
├── 03-user-flows.md       # User Flows
├── 04-database-schema.md  # Database Design
├── 05-design-guidelines.md # Design System
├── 06-task-list.md        # Development Tasks
└── custom/                # Custom documents
    ├── api-specification.md
    ├── qa-testing.md
    └── ...
```

## Cross-Referencing

Documents automatically cross-reference each other:

```markdown
<!-- In Task List -->
- Task-2.3: Implement checkout flow
  - Implements FR-12 (PRD)
  - Uses TS-4.2 payment gateway (Tech Stack)
  - Follows UF-7 checkout journey (User Flows)
  - Creates DB-5 orders table (Database Schema)
```

## Example Projects

See the `examples/` folder for sample documentation generated for:
- SaaS Analytics Dashboard
- E-commerce Platform
- Mobile Fitness App

## Contributing

Contributions welcome! Please read our contributing guidelines.

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## License

MIT License - see [LICENSE](LICENSE) for details.

---

*Built by [BuildMVPFast](https://buildmvpfast.com) • Open-source version of [ProjectDocsEngine.com](https://projectdocsengine.com)*
