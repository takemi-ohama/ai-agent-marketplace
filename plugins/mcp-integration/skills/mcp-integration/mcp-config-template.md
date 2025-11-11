# MCP Configuration Template

Create a `.mcp.json` file in your project root with this configuration:

```json
{
  "name": "Your Project MCP Configuration",
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp",
      "headers": {
        "Authorization": "Bearer ${GITHUB_PERSONAL_ACCESS_TOKEN}"
      }
    },
    "notion": {
      "type": "http",
      "url": "https://mcp.notion.com/mcp"
    },
    "serena": {
      "command": "uvx",
      "args": [
        "--from",
        "git+https://github.com/oraios/serena",
        "serena",
        "start-mcp-server",
        "--context",
        "ide-assistant",
        "--enable-web-dashboard",
        "False"
      ]
    },
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": [
        "awslabs.aws-documentation-mcp-server@latest"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR",
        "AWS_DOCUMENTATION_PARTITION": "aws"
      },
      "disabled": false
    },
    "mcp-server-bigquery": {
      "command": "uvx",
      "args": [
        "mcp-server-bigquery@latest"
      ],
      "env": {},
      "disabled": false
    }
  }
}
```

## MCP Server Descriptions

### GitHub MCP (HTTP-based)
- **Purpose**: Repository operations, PR management, issue tracking
- **Authentication**: Requires `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable
- **Tools**: create_pull_request, add_issue_comment, merge_pull_request, search_code, etc.

### Notion MCP (HTTP-based)
- **Purpose**: Document management, database operations
- **Authentication**: Automatic via Notion's authentication flow
- **Tools**: notion-search, notion-fetch, notion-create-pages, notion-update-page, etc.

### Serena MCP (Local)
- **Purpose**: Semantic code understanding and editing
- **Requirements**: Python 3.10+, uvx
- **Tools**: find_symbol, replace_symbol_body, search_for_pattern, write_memory, etc.

### AWS Documentation MCP (Local, Optional)
- **Purpose**: AWS service documentation access
- **Tools**: read_documentation, search_documentation, recommend

### BigQuery MCP (Local)
- **Purpose**: Database operations
- **Authentication**: Google Cloud credentials via application default credentials
- **Tools**: execute-query, list-tables, describe-table

## Customization

### Disabling MCP Servers

Add `"disabled": true` to any server configuration:

```json
"serena": {
  "command": "uvx",
  "args": ["..."],
  "disabled": true
}
```

### Adding Environment Variables

Extend the `env` object for any MCP server:

```json
"mcp-server-bigquery": {
  "command": "uvx",
  "args": ["mcp-server-bigquery@latest"],
  "env": {
    "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/credentials.json",
    "BIGQUERY_PROJECT_ID": "your-project-id"
  }
}
```

## Verification

After creating `.mcp.json`, restart Claude Code and verify MCP servers are loaded by checking available tools.
