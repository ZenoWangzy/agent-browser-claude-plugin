# Contributing to agent-browser Claude Code Plugin

Thank you for your interest in contributing! This plugin is a community resource and contributions are welcome.

## How to Contribute

### Reporting Issues

1. Check existing issues first
2. Use the issue templates if available
3. Include:
   - Claude Code version
   - agent-browser version
   - OS and platform
   - Steps to reproduce
   - Expected vs actual behavior

### Submitting Pull Requests

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit PR with clear description

### Areas for Contribution

#### New Skills
- Language-specific patterns (Python, Go, Rust scraping)
- Framework-specific configs (Django, Rails, Laravel)
- Domain-specific knowledge (ML data, finance, etc.)

#### New Commands
- Additional browser operations
- Complex workflow helpers
- Integration commands

#### New Agents
- Specialized testing agents
- Data extraction specialists
- Performance monitoring agents

#### Hooks & Rules
- Additional validation hooks
- Best practice rules
- Error recovery patterns

## Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/agent-browser-claude-plugin.git

# Navigate to directory
cd agent-browser-claude-plugin

# Create a branch
git checkout -b feature/your-feature-name

# Make your changes
# ...

# Test locally by copying to Claude config
cp -r agents/* ~/.claude/agents/
cp -r skills/* ~/.claude/skills/
# ... etc
```

## Code Style

### Markdown Files
- Use proper heading hierarchy
- Include code examples
- Add clear descriptions
- Use tables for reference material

### Agent Definitions
- Clear name and description
- List available tools
- Specify recommended model
- Include examples

### Skills
- Start with use cases
- Provide workflows
- Include best practices
- Add error handling

### Commands
- Clear usage syntax
- Practical examples
- List all options
- Notes on behavior

## Testing

Test your changes:
1. Copy to local Claude config
2. Test in real scenarios
3. Verify with different websites
4. Check error handling

## Documentation

- Update README for new features
- Add examples for new commands
- Document any breaking changes
- Keep Chinese README in sync

## Pull Request Process

1. Ensure your PR description clearly explains the changes
2. Link any related issues
3. Include screenshots if applicable
4. Wait for review and address feedback

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing! 🎉
