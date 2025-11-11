# Claude Code AI Agent Guidelines

## Purpose

This document provides guidelines for AI agents interacting with the AI Agent Marketplace repository.

## Repository Overview

This is a Claude Code plugin marketplace for distributing:
- MCP (Model Context Protocol) integration skills
- Custom slash commands
- Sub-agent configurations
- Project hooks

## AI Agent Responsibilities

### 1. Plugin Development

When creating or modifying plugins:

**DO:**
- Follow the directory structure outlined in README.md
- Include complete metadata in `plugin.json`
- Write clear SKILL.md files with YAML frontmatter
- Provide comprehensive documentation
- Test configurations before committing
- Update marketplace.json when adding plugins

**DON'T:**
- Commit sensitive tokens or credentials
- Create plugins without proper metadata
- Skip documentation
- Use inconsistent naming conventions

### 2. MCP Configuration

When working with MCP configurations:

**DO:**
- Use environment variables for authentication
- Provide clear setup instructions
- Include troubleshooting sections
- Document all required permissions
- Test configurations in different environments

**DON'T:**
- Hardcode tokens in `.mcp.json` templates
- Skip authentication documentation
- Omit security best practices
- Make assumptions about user environments

### 3. Documentation

When writing documentation:

**DO:**
- Write in clear, concise language
- Provide step-by-step instructions
- Include usage examples
- Link to official documentation
- Cover common troubleshooting scenarios
- Use code blocks with syntax highlighting

**DON'T:**
- Assume prior knowledge
- Skip verification steps
- Omit prerequisites
- Write ambiguous instructions

### 4. Version Control

When managing versions:

**DO:**
- Follow semantic versioning (MAJOR.MINOR.PATCH)
- Update version numbers in plugin.json
- Document breaking changes
- Test upgrades from previous versions

**DON'T:**
- Make breaking changes in minor versions
- Skip version increments
- Forget to update marketplace.json

## File Structure Reference

```
ai-agent-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace metadata
├── plugins/
│   └── {plugin-name}/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin metadata
│       ├── commands/             # Slash commands (*.md)
│       ├── agents/               # Sub-agents (*.md)
│       └── skills/               # Project skills
│           └── {skill-name}/
│               ├── SKILL.md      # Entry point (required)
│               └── *.md          # Supporting docs
├── README.md
└── CLAUDE.md                     # This file
```

## MCP Integration Plugin

### Components

The MCP integration plugin consists of:

1. **SKILL.md**: Entry point with YAML frontmatter
2. **mcp-config-template.md**: Complete `.mcp.json` template
3. **mcp-authentication-guide.md**: Token setup instructions
4. **mcp-setup-guide.md**: Step-by-step installation

### Maintenance Tasks

When updating MCP integration:

1. Check for new MCP servers
2. Update configuration templates
3. Verify authentication methods
4. Test with latest Claude Code version
5. Update documentation
6. Increment version in plugin.json

### Supported MCPs

Current MCPs in the integration:

1. **GitHub MCP** (HTTP)
   - Tools: PR management, issue tracking, code search
   - Auth: GitHub Personal Access Token
   - Documentation: https://github.com/github/github-mcp-server

2. **Notion MCP** (HTTP)
   - Tools: Search, page creation, database operations
   - Auth: Notion integration token
   - Documentation: https://mcp.notion.com

3. **Serena MCP** (Local)
   - Tools: Code analysis, symbol editing, memory management
   - Auth: None (local)
   - Documentation: https://github.com/oraios/serena

4. **AWS Documentation MCP** (Local)
   - Tools: Documentation search, content reading
   - Auth: None (public docs)
   - Documentation: https://github.com/awslabs/aws-documentation-mcp-server

5. **BigQuery MCP** (Local)
   - Tools: Query execution, table operations
   - Auth: Google Cloud credentials
   - Documentation: https://github.com/ergut/mcp-server-bigquery

## Common Tasks

### Adding a New MCP Server

1. Update `mcp-config-template.md` with new MCP configuration
2. Add authentication instructions to `mcp-authentication-guide.md`
3. Include usage examples in `mcp-setup-guide.md`
4. Update SKILL.md description
5. Test the configuration
6. Increment plugin version
7. Commit changes

### Creating a New Plugin

1. Create directory structure:
   ```bash
   mkdir -p plugins/{plugin-name}/{.claude-plugin,commands,agents,skills}
   ```

2. Create plugin.json with metadata

3. Add content (skills, commands, or agents)

4. Register in marketplace.json

5. Write documentation

6. Test installation process

7. Commit and push

### Updating Documentation

1. Review existing content for accuracy
2. Check for broken links
3. Update version-specific information
4. Add new troubleshooting scenarios
5. Improve clarity and completeness
6. Test all commands and examples
7. Commit changes

## Security Guidelines

### Tokens and Credentials

**NEVER commit:**
- GitHub Personal Access Tokens
- Notion integration tokens
- Google Cloud service account keys
- Any other credentials or secrets

**ALWAYS use:**
- Environment variables for tokens
- Placeholder values in templates
- Clear warnings about credential security
- Instructions for secure credential storage

### Configuration Templates

**Include:**
- `${VARIABLE_NAME}` syntax for environment variables
- Comments explaining required permissions
- Security best practices section
- Links to official authentication guides

**Avoid:**
- Example tokens that look real
- Encouraging inline credential storage
- Skipping security considerations

## Testing Checklist

Before committing changes:

- [ ] All JSON files are valid
- [ ] YAML frontmatter parses correctly
- [ ] Links point to correct destinations
- [ ] Code examples are syntactically correct
- [ ] Installation steps work from scratch
- [ ] Documentation is clear and complete
- [ ] Version numbers are updated
- [ ] No sensitive data in commits

## References

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [MCP Specification](https://modelcontextprotocol.io)
- [Plugin Development Guide](https://docs.claude.com/en/docs/claude-code/plugins)
- [Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)

## Support

For questions about marketplace development:
1. Review this document
2. Check Claude Code documentation
3. Examine existing plugins for patterns
4. Open an issue if needed
