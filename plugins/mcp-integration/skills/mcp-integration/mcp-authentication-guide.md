# MCP Authentication Guide

This guide explains how to set up authentication for each MCP server.

## GitHub MCP

### Requirements
- GitHub Personal Access Token (PAT) with appropriate scopes

### Setup Steps

1. **Create a GitHub Personal Access Token**
   - Go to: https://github.com/settings/tokens
   - Click "Generate new token (classic)"
   - Required scopes:
     - `repo` (full control of private repositories)
     - `read:org` (read organization data)
     - `workflow` (update GitHub Action workflows)

2. **Set Environment Variable**
   ```bash
   export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_your_token_here"
   ```

3. **Persist in Shell Configuration**
   Add to `~/.bashrc`, `~/.zshrc`, or equivalent:
   ```bash
   export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_your_token_here"
   ```

### Security Best Practices
- Never commit tokens to repositories
- Use environment variables or secure credential managers
- Rotate tokens periodically
- Use fine-grained tokens when possible

## Notion MCP

### Requirements
- Notion integration token

### Setup Steps

1. **Create a Notion Integration**
   - Go to: https://www.notion.so/my-integrations
   - Click "New integration"
   - Give it appropriate permissions (read, write, comment)
   - Copy the integration token

2. **Grant Integration Access**
   - Open your Notion workspace
   - Navigate to the page/database you want to access
   - Click "..." → "Connections" → Add your integration

3. **Authentication Method**
   Notion MCP handles authentication automatically through the HTTP endpoint.
   No additional environment variables needed in `.mcp.json`.

## Serena MCP

### Requirements
- Python 3.10 or higher
- `uvx` (installed via `pip install uv`)

### Setup Steps

No authentication required. Serena operates locally on your codebase.

1. **Install uvx**
   ```bash
   pip install uv
   ```

2. **Verify Installation**
   ```bash
   uvx --version
   ```

### Project Activation

After Serena MCP is loaded, activate your project:
```bash
mcp__serena__activate_project(".")
```

## BigQuery MCP

### Requirements
- Google Cloud Project with BigQuery API enabled
- Service account or application default credentials

### Setup Steps

#### Option 1: Service Account (Recommended for Production)

1. **Create Service Account**
   - Go to: https://console.cloud.google.com/iam-admin/serviceaccounts
   - Click "Create Service Account"
   - Grant "BigQuery Data Editor" and "BigQuery Job User" roles

2. **Create and Download Key**
   - Click on the service account → "Keys" → "Add Key" → "JSON"
   - Download the JSON key file

3. **Set Environment Variable**
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account-key.json"
   ```

4. **Update `.mcp.json`**
   ```json
   "mcp-server-bigquery": {
     "command": "uvx",
     "args": ["mcp-server-bigquery@latest"],
     "env": {
       "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/service-account-key.json",
       "BIGQUERY_PROJECT_ID": "your-project-id"
     }
   }
   ```

#### Option 2: Application Default Credentials (Development)

1. **Install Google Cloud SDK**
   - Download from: https://cloud.google.com/sdk/docs/install

2. **Authenticate**
   ```bash
   gcloud auth application-default login
   ```

3. **Set Default Project**
   ```bash
   gcloud config set project your-project-id
   ```

### Security Best Practices
- Use service accounts with minimal required permissions
- Never commit service account keys to repositories
- Use Workload Identity for GKE/Cloud Run deployments
- Rotate service account keys periodically

## AWS Documentation MCP

### Requirements
None - this MCP accesses public AWS documentation.

### Setup Steps
No authentication required. The MCP is ready to use after configuration.

## DBHub MCP

### Requirements
- Node.js (for npx)
- Database server running (PostgreSQL, MySQL, SQL Server, MariaDB, or SQLite)
- Database connection credentials

### Setup Steps

1. **Prepare Database Connection String (DSN)**

   Create a connection string based on your database type:

   **PostgreSQL:**
   ```
   postgres://username:password@localhost:5432/database_name?sslmode=disable
   ```

   **MySQL:**
   ```
   mysql://username:password@localhost:3306/database_name
   ```

   **SQL Server:**
   ```
   sqlserver://username:password@localhost:1433/database_name
   ```

   **SQLite:**
   ```
   sqlite:///path/to/database.db
   ```

2. **Set Environment Variable**

   Add to your `.env` file:
   ```bash
   DATABASE_DSN=postgres://user:password@localhost:5432/mydb?sslmode=disable
   ```

3. **Verify Database Connection**

   Ensure:
   - Database server is running
   - Username and password are correct
   - Database exists
   - Network/firewall allows connection

### Security Best Practices
- Never commit `.env` file with credentials
- Use read-only database users when possible
- Restrict database access by IP when applicable
- Use SSL/TLS for remote database connections

## Chrome DevTools MCP

### Requirements
- Node.js (for npx)
- Chrome browser installed

### Setup Steps

No authentication required. Chrome DevTools MCP runs locally and controls Chrome browser instances on your machine.

1. **Verify Chrome Installation**
   ```bash
   # Check Chrome is installed
   google-chrome --version  # Linux
   # or
   "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --version  # macOS
   ```

2. **Node.js Verification**
   ```bash
   node --version
   npx --version
   ```

### Optional Configuration

For advanced use cases, you can:
- Connect to remote Chrome instances via WebSocket
- Configure viewport size
- Set headless/headful mode
- Use different Chrome channels (stable, canary, beta, dev)

These are configured in `.mcp.json` args, not via authentication.

## Codex CLI MCP

### Requirements
- Codex CLI installed on your system
- OpenAI account (ChatGPT Plus/Pro/Team/Edu/Enterprise) OR OpenAI API key

### Setup Steps

**1. Install Codex CLI**

Download and install from the official releases:
https://github.com/openai/codex/releases

For the latest Rust-based version:
```bash
# Follow installation instructions from the GitHub releases page
# Example for macOS/Linux (adjust based on your platform):
# curl -sSL https://github.com/openai/codex/releases/download/rust-v0.36.0/codex-<platform> -o codex
# chmod +x codex
# sudo mv codex /usr/local/bin/
```

**2. Verify Installation**
```bash
codex --version
```

**3. Authenticate (Choose one method)**

**Method 1: ChatGPT Login (Recommended)**

For users with ChatGPT Plus, Pro, Team, Edu, or Enterprise plans:

```bash
codex login
```

This will:
- Start a local server on `localhost:1455`
- Open your browser for authentication
- Save credentials to `~/.codex/auth.json`

**Method 2: API Key Login**

For pay-as-you-go users with OpenAI API keys:

```bash
# From environment variable (recommended)
printenv OPENAI_API_KEY | codex login --with-api-key

# Or from file
echo "your-api-key-here" > my_key.txt
codex login --with-api-key < my_key.txt
rm my_key.txt  # Clean up
```

**Important**: Use `--with-api-key` (not `--api-key`) to prevent the key from appearing in shell history or process listings.

**Getting an OpenAI API Key:**
1. Go to https://platform.openai.com/api-keys
2. Create a new API key
3. Ensure it has access to the Responses API

**4. Test MCP Server**
```bash
codex mcp-server
```
This should start the Codex MCP server without errors.

### Features
- **Code Analysis**: Analyze code quality, architecture, and design patterns
- **Performance Optimization**: Identify performance bottlenecks and suggest improvements
- **Security Review**: Detect potential security vulnerabilities
- **Architecture Consultation**: Get AI-powered advice on code structure and design

### References
- [Codex CLI GitHub](https://github.com/openai/codex)
- [Codex CLI Advanced Features](https://github.com/openai/codex/blob/main/docs/advanced.md)

## Troubleshooting

### GitHub MCP Not Working
- Verify token has correct scopes
- Check token hasn't expired
- Ensure environment variable is set correctly
- Restart Claude Code after setting environment variables

### Notion MCP Connection Issues
- Verify integration has been added to the target pages/databases
- Check integration permissions (read, write, comment)
- Ensure you're using the correct workspace

### Serena MCP Errors
- Verify Python 3.10+ is installed
- Check uvx is properly installed: `uvx --version`
- Ensure git repository is initialized

### BigQuery MCP Authentication Failures
- Verify service account has BigQuery permissions
- Check `GOOGLE_APPLICATION_CREDENTIALS` path is correct
- Ensure BigQuery API is enabled in your GCP project
- Verify project ID is correct

### DBHub MCP Connection Issues
- Verify database server is running
- Check DATABASE_DSN format is correct
- Ensure database credentials are valid
- Test network connectivity to database server
- Check firewall rules allow database connections

### Chrome DevTools MCP Issues
- Verify Chrome browser is installed
- Check Node.js is installed (`node --version`)
- Ensure no other processes are using Chrome DevTools Protocol port
- Try running in non-headless mode for debugging

### Codex CLI MCP Issues
- Verify Codex CLI is installed (`codex --version`)
- **Authentication errors**: Run `codex login` or `codex login --with-api-key` first
- Check authentication status: `cat ~/.codex/auth.json` (should exist after login)
- Test MCP server: `codex mcp-server` (should start without errors)
- Ensure Codex is in your system PATH
- For API key users: Verify OPENAI_API_KEY environment variable is set
- Restart Claude Code after authentication

## Environment Variable Summary

**Using `.env` file (Recommended):**

Create a `.env` file in your project root:

```bash
# GitHub MCP (Required)
GITHUB_PERSONAL_ACCESS_TOKEN=ghp_your_token_here

# Notion MCP (Optional)
NOTION_API_KEY=secret_your_token_here

# BigQuery MCP (Optional)
GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account-key.json

# DBHub MCP (Optional)
DATABASE_DSN=postgres://user:password@localhost:5432/mydb?sslmode=disable

# Codex CLI MCP (Required - authenticate before use)
# Run either: codex login (ChatGPT) or printenv OPENAI_API_KEY | codex login --with-api-key
# Authentication is stored in ~/.codex/auth.json

# Note: Serena MCP, AWS Documentation MCP, and Chrome DevTools MCP do not require authentication
```

**Legacy: Using shell exports:**

```bash
# GitHub MCP
export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_your_token_here"

# BigQuery MCP
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account-key.json"

# DBHub MCP
export DATABASE_DSN="postgres://user:password@localhost:5432/mydb?sslmode=disable"
```

The MCP Integration skill automatically creates a `.env` template and adds it to `.gitignore` for security.
