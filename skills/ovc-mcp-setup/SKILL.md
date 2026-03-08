---
name: ovc-mcp-setup
description: Set up MCP (Model Context Protocol) servers to give the AI coding agent direct access to databases, documentation, APIs, and deployment platforms. Use when the user wants to enhance their agent's capabilities, connect tools, set up MCP, or give their agent access to external services. Also trigger on "set up MCP", "connect my database", "agent tools", "upgrade my agent", "add MCP server".
metadata:
  author: r4ph-t
  version: "1.0"
license: MIT
---

# OVC MCP Setup

Upgrade your AI coding agent from a text generator to a fully connected development tool. MCP (Model Context Protocol) servers give your agent direct access to your actual stack: databases, documentation, APIs, project management, and deployment platforms.

## Why this matters

Most developers use their coding agent as a conversation partner. They copy-paste error logs, manually describe their database schema, and switch between browser tabs to look things up. Every context switch is friction, and every manual copy-paste is an opportunity for information loss.

MCP servers eliminate this. The agent can query your database directly, read live documentation, interact with your deployment platform, and access your project management tools. The more your agent can interact with your real environment, the better its output.

## Steps

### 1. Check current MCP setup

Look for existing MCP configuration:
- Claude Code: `~/.claude/claude_code_config.json` or project-level `.mcp.json`
- Cursor: `.cursor/mcp.json` in the project root
- Other agents: check the agent's documentation for MCP support

### 2. Identify what to connect

Ask the user about their stack and recommend relevant MCP servers:

**Database access**
- PostgreSQL: gives the agent direct access to query your schema, inspect tables, and understand your data model without you describing it manually.
- Redis, MongoDB, MySQL: similar benefits for other database engines.

**Documentation**
- Documentation servers that serve framework/library docs directly into the agent's context. Eliminates hallucinated API calls.

**Deployment and infrastructure**
- Render MCP: deploy services, check logs, manage environment variables, inspect service status.
- Cloud provider MCPs for AWS, GCP, etc.

**Project management**
- GitHub: create issues, review PRs, read repository context.
- Linear, Jira: access tickets and project context.

**Search and knowledge**
- Web search MCPs: let the agent look up current documentation, Stack Overflow answers, and package information.
- Internal documentation servers for company wikis or knowledge bases.

### 3. Install and configure

For each selected MCP server:

1. Find the package (most are published on npm or PyPI).
2. Add it to the agent's MCP configuration file.
3. Configure authentication (API keys, connection strings) using environment variables, never hardcoded.
4. Test the connection by asking the agent to use the new tool.

Example configuration structure for Cursor (`.cursor/mcp.json`):
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

Example for Claude Code (`.mcp.json`):
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

### 4. Security considerations

- Store MCP credentials in environment variables, never in committed config files.
- Add MCP config files with secrets to `.gitignore`.
- Use read-only database connections for the agent unless writes are specifically needed.
- Review what permissions each MCP server requires and scope them as narrowly as possible.
- For database access, consider using a read replica or a restricted user with SELECT-only permissions.

### 5. Verify the setup

After configuration, test each MCP server:
- Ask the agent to list available tools (it should see the new MCP tools).
- Run a simple query or action through each server.
- Verify the agent can access the data it needs without errors.

### 6. Document what's connected

Add a note to the project's rules file listing which MCP servers are configured and what they provide. This helps future sessions and other developers on the team understand what the agent has access to.
