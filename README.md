# AI Agent Marketplace

Internal marketplace for sharing Claude Code plugins, project skills, and MCP (Model Context Protocol) configurations.

## Overview

This marketplace provides pre-configured plugins to accelerate Claude Code adoption across teams, including:

- **MCP Integration Skills**: Automated setup for GitHub, Serena, BigQuery, and Notion MCPs
- **Custom Commands**: Reusable slash commands for common tasks
- **Sub-Agents**: Specialized AI agents for different domains
- **Hooks**: Automated workflows triggered by events

## Available Plugins

### MCP Integration (`mcp-integration`)

Project skill for setting up MCP servers in Claude Code projects.

**Features:**
- Complete `.mcp.json` template with 5 MCP servers
- Authentication setup guides
- Usage examples and best practices
- Troubleshooting documentation

**Included MCPs:**
1. **GitHub MCP**: Repository operations, PR management, issue tracking
2. **Notion MCP**: Document management, database operations
3. **Serena MCP**: Semantic code understanding and editing
4. **AWS Documentation MCP**: AWS service documentation access
5. **BigQuery MCP**: Database queries and schema management

**Documentation:**
- [SKILL.md](./plugins/mcp-integration/skills/mcp-integration/SKILL.md) - Overview
- [Setup Guide](./plugins/mcp-integration/skills/mcp-integration/mcp-setup-guide.md) - Installation steps
- [Config Template](./plugins/mcp-integration/skills/mcp-integration/mcp-config-template.md) - `.mcp.json` template
- [Authentication Guide](./plugins/mcp-integration/skills/mcp-integration/mcp-authentication-guide.md) - Token setup

## Installation

### Add Marketplace to Claude Code

1. Open Claude Code
2. Run command: `/plugin marketplace add`
3. Enter this repository URL: `https://github.com/takemi-ohama/ai-agent-marketplace`

### Install a Plugin

```bash
/plugin install mcp-integration@ai-agent-marketplace
```

Claude Code will:
1. Download the plugin
2. Copy files to your project's `.claude/` directory
3. Make the skill available for autonomous invocation

### Verify Installation

Check that the skill appears in your project:
```bash
ls -la .claude/skills/mcp-integration/
```

## Usage

### MCP Integration Skill

The skill activates automatically when you mention:
- "Set up MCP servers"
- "Integrate GitHub/BigQuery/Notion"
- "Enable code analysis"
- "Configure Serena"

Or invoke manually:
```
@mcp-integration-skill help me set up MCPs
```

## Plugin Development

### Directory Structure

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
│       └── skills/               # Project skills (SKILL.md)
└── README.md
```

### Creating a New Plugin

1. Create plugin directory:
   ```bash
   mkdir -p plugins/my-plugin/{.claude-plugin,commands,agents,skills}
   ```

2. Create `plugin.json`:
   ```json
   {
     "name": "my-plugin",
     "version": "1.0.0",
     "description": "Plugin description",
     "author": {
       "name": "Your Name",
       "url": "https://github.com/username"
     }
   }
   ```

3. Add skills, commands, or agents

4. Register in `marketplace.json`:
   ```json
   {
     "plugins": [
       {
         "name": "my-plugin",
         "path": "plugins/my-plugin",
         "description": "Short description"
       }
     ]
   }
   ```

### Project Skills

Skills are automatically invoked by Claude Code when relevant. See [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills) for details.

**Key Requirements:**
- Entry point must be named `SKILL.md`
- Include YAML frontmatter with `name` and `description`
- Description should contain keywords for autonomous invocation

## Contributing

1. Fork this repository
2. Create a new plugin or improve existing ones
3. Submit a pull request
4. Update marketplace.json if adding new plugins

## Marketplace Management

### Updating Plugins

Plugins are version-controlled. To update:

1. Modify plugin files
2. Increment version in `plugin.json`
3. Commit and push changes
4. Users refresh via Claude Code UI

### Removing Plugins

1. Remove from `marketplace.json`
2. Optionally delete plugin directory
3. Commit changes

## References

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [MCP Specification](https://modelcontextprotocol.io)
- [GitHub MCP Server](https://github.com/github/github-mcp-server)
- [Serena MCP](https://github.com/oraios/serena)
- [BigQuery MCP](https://github.com/ergut/mcp-server-bigquery)
- [Notion MCP](https://mcp.notion.com)

## License

MIT License - See LICENSE file for details

## Support

For issues or questions:
- Open an issue in this repository
- Contact the plugin author (see plugin.json)
