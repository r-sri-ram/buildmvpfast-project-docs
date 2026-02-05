# Contributing to BuildMVPFast Project Docs

Thank you for your interest in contributing! This guide will help you get started.

## How to Contribute

### 1. Fork the Repository

Click the "Fork" button at the top right of [the repo](https://github.com/r-sri-ram/buildmvpfast-project-docs).

### 2. Clone Your Fork

```bash
git clone https://github.com/YOUR-USERNAME/buildmvpfast-project-docs.git
cd buildmvpfast-project-docs
```

### 3. Create a Branch

```bash
git checkout -b feature/your-feature-name
```

Use descriptive branch names:
- `feature/add-new-template` - New features
- `fix/wizard-question-typo` - Bug fixes
- `docs/update-readme` - Documentation updates

### 4. Make Your Changes

#### Project Structure

```
buildmvpfast-project-docs/
├── skills/
│   ├── project-docs/      # Main documentation generator
│   │   └── SKILL.md
│   ├── add-doc/           # Add custom documents
│   │   └── SKILL.md
│   ├── analyze-project/   # Codebase analysis
│   │   └── SKILL.md
│   └── update-docs/       # Update existing docs
│       └── SKILL.md
├── templates/
│   ├── core/              # 6 core document templates
│   └── custom/            # 9 custom document templates
├── prompts/               # AI prompt guides
└── examples/              # Sample output
```

#### Types of Contributions Welcome

- **New document templates** - Add templates to `templates/custom/`
- **Improved wizard questions** - Better AskUserQuestion options in SKILL.md files
- **Bug fixes** - Fix issues with skill behavior
- **Documentation** - Improve README, examples, or comments
- **New skills** - Add new related skills

#### SKILL.md Guidelines

When editing SKILL.md files:

1. **Use AskUserQuestion for all selections** - Never use plain text menus
2. **Provide 2-4 options per question** - Tool limitation
3. **Include "Other" implicitly** - Users can always type custom responses
4. **Use multiSelect: true** where multiple choices make sense

Example:
```
AskUserQuestion({
  questions: [{
    question: "What type of project is this?",
    header: "Type",        // Max 12 chars
    options: [
      { label: "SaaS", description: "Subscription web app" },
      { label: "E-commerce", description: "Online store" },
      { label: "Mobile App", description: "iOS/Android app" },
      { label: "API/Backend", description: "Backend service" }
    ],
    multiSelect: false
  }]
})
```

### 5. Test Your Changes

Install the skill locally and test:

```bash
# Copy to Claude Code skills directory
cp -r skills/* ~/.claude/skills/

# Test in Claude Code
# Run: /project-docs new
# Run: /add-doc api
# etc.
```

### 6. Commit Your Changes

```bash
git add .
git commit -m "feat: add new template for security documentation"
```

Commit message format:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `refactor:` - Code refactoring
- `test:` - Adding tests

### 7. Push and Create Pull Request

```bash
git push origin feature/your-feature-name
```

Then go to GitHub and click "Compare & pull request".

## Pull Request Guidelines

### PR Title Format

```
feat: Add security documentation template
fix: Correct wizard question for API auth
docs: Update installation instructions
```

### PR Description Template

```markdown
## Summary
Brief description of what this PR does.

## Changes
- Added new template for X
- Fixed bug in Y
- Updated wizard question for Z

## Testing
- [ ] Tested /project-docs new
- [ ] Tested /add-doc [type]
- [ ] Verified documents generate correctly

## Screenshots (if applicable)
[Add screenshots of wizard interactions or generated docs]
```

## Review Process

1. **Automated checks** - Ensure files are valid markdown
2. **Maintainer review** - We'll review within 3-5 days
3. **Feedback** - We may request changes
4. **Merge** - Once approved, we'll merge your PR

## Code of Conduct

- Be respectful and constructive
- Focus on the contribution, not the person
- Help others learn and grow

## Questions?

- Open an [Issue](https://github.com/r-sri-ram/buildmvpfast-project-docs/issues)
- Tag `@r-sri-ram` for questions

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

*Built by [BuildMVPFast](https://buildmvpfast.com)*
