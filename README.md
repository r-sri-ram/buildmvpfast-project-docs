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

### Option 1: One-Line Install (Recommended)

```bash
git clone https://github.com/r-sri-ram/buildmvpfast-project-docs.git && cp -r buildmvpfast-project-docs/skills/* ~/.claude/skills/ && rm -rf buildmvpfast-project-docs
```

### Option 2: Manual Installation

```bash
# Clone the repository
git clone https://github.com/r-sri-ram/buildmvpfast-project-docs.git

# Copy skills to Claude Code skills directory
cp -r buildmvpfast-project-docs/skills/* ~/.claude/skills/

# Clean up
rm -rf buildmvpfast-project-docs
```

### Option 3: npx skills (Project-Local)

Install to current project only using [npx skills](https://skills.sh):

```bash
npx skills add r-sri-ram/buildmvpfast-project-docs
```

> **Note:** This installs to `.agents/skills/` in your project, not globally.

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

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

**Quick start:**
```bash
# Fork and clone
git clone https://github.com/YOUR-USERNAME/buildmvpfast-project-docs.git

# Create branch
git checkout -b feature/your-feature

# Make changes, then test locally
cp -r skills/* ~/.claude/skills/

# Commit and push
git add . && git commit -m "feat: your feature"
git push origin feature/your-feature

# Open a Pull Request on GitHub
```

**What you can contribute:**
- New document templates
- Improved wizard questions
- Bug fixes
- Documentation improvements

## License

MIT License - see [LICENSE](LICENSE) for details.

---

*Built by [BuildMVPFast](https://buildmvpfast.com) • Open-source version of [ProjectDocsEngine.com](https://projectdocsengine.com)*
