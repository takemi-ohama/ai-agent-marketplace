---
name: MCP Integration Setup
description: Automatically set up GitHub, Serena, BigQuery, and Notion MCP servers for enhanced Claude Code capabilities including code analysis, data operations, and project management
---

# MCP Integration Project Skill

This skill helps you set up Model Context Protocol (MCP) servers in your Claude Code projects, enabling powerful integrations with:

- **GitHub MCP**: Repository management, PR reviews, issue tracking
- **Serena MCP**: Semantic code understanding, symbol-based editing, memory management
- **BigQuery MCP**: Database queries, table operations, schema management
- **Notion MCP**: Document search, page creation, database operations

## When This Skill Is Invoked

This skill activates when you mention:
- Setting up MCP servers
- Integrating external services (GitHub, BigQuery, Notion)
- Enabling code analysis capabilities
- Configuring Serena for semantic operations

## What This Skill Provides

### 1. MCP Configuration Template

A complete `.mcp.json` configuration with:
- GitHub MCP (HTTP-based with authentication)
- Notion MCP (HTTP-based)
- Serena MCP (local via uvx)
- AWS Documentation MCP (optional)
- BigQuery MCP (local via uvx)

### 2. Setup Instructions

Detailed guides for:
- Authentication requirements (GitHub PAT, Notion tokens)
- Environment variable configuration
- MCP server activation
- Verification steps

### 3. Best Practices

- Security considerations for token management
- Performance optimization tips
- Troubleshooting common issues

## Quick Start

1. **Create `.mcp.json`** in your project root (see mcp-config-template.md)
2. **Set environment variables** for authentication
3. **Restart Claude Code** to load MCP servers
4. **Verify** by checking available tools

For detailed setup instructions, see [mcp-setup-guide.md](./mcp-setup-guide.md).

## Configuration Template

See [mcp-config-template.md](./mcp-config-template.md) for the complete `.mcp.json` template.

## Authentication Guide

See [mcp-authentication-guide.md](./mcp-authentication-guide.md) for setting up tokens and credentials.
