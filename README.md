# SsmsAgentic

An AI-powered chat extension for **SQL Server Management Studio 22** that brings Claude directly into your database workflow.

SsmsAgentic adds a dockable chat panel to SSMS where you can ask Claude to explore schemas, write queries, explain execution plans, and execute SQL — all within your existing SSMS session, using your live database connection.

> **Free trial release.** This extension is currently distributed as a free trial. For pricing, licensing terms, and more information, visit [ssmsagentic.io](https://ssmsagentic.io).

<!-- TODO: Add screenshots here -->

## What it does

- **Chat with Claude inside SSMS** — a dockable tool window, accessible from the Tools menu
- **Uses your live connection** — Claude sees the server and database you're connected to, including Entra authentication
- **Schema exploration** — ask Claude about tables, views, columns, and relationships in your database
- **Query generation** — describe what you need in plain language, get T-SQL back
- **Query explanation** — paste a query and ask Claude to explain it or review its execution plan
- **You stay in control** — every tool call (schema reads, queries, mutations) is gated by an in-chat permission prompt; nothing runs against your database without your explicit approval
- **Open in Query Editor** — every SQL statement Claude proposes or runs gets a one-click button to open it in a native SSMS query editor tab against your current connection

## SQL tools available to Claude

| Tool | Description |
|---|---|
| `get_schema` | List tables and views with optional database/schema filters |
| `describe_object` | Return columns, types, and metadata for a table or view |
| `run_select_query` | Execute read-only SELECT queries (capped row limit) |
| `run_mutation_query` | Execute INSERT/UPDATE/DELETE/DDL with user approval |
| `explain_query` | Return estimated execution plan (SHOWPLAN_XML) without executing |

All SQL operations are validated using SQL Server's ScriptDom AST to enforce read/write boundaries.

## Requirements

- **SQL Server Management Studio 22** (SSMS 22)
- **Claude Code CLI** installed and authenticated — [installation guide](https://docs.anthropic.com/en/docs/claude-code/overview)
- Windows (amd64)

## Installation

1. Download the latest `.vsix` file from the [Releases](https://github.com/ado-webco/ssms-agentic/releases) page
2. Close SSMS if it's running
3. Double-click the `.vsix` file to install

   Or install from command line:
   ```
   "C:\Program Files\Microsoft SQL Server Management Studio 22\Release\Common7\IDE\VSIXInstaller.exe" /q SsmsAgentic.SsmsExtension.vsix
   ```
4. Launch SSMS — find **SsmsAgentic** under the **Tools** menu

## Updating

Download the new `.vsix` from [Releases](https://github.com/ado-webco/ssms-agentic/releases) and install it. The installer will replace the previous version automatically.

To uninstall:
```
"C:\Program Files\Microsoft SQL Server Management Studio 22\Release\Common7\IDE\VSIXInstaller.exe" /q /u:SsmsAgentic.SsmsExtension.4a8e7b12-9c3d-4e5f-a1b2-c7d8e9f0a1b2
```

## How it works

SsmsAgentic hosts the [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code/overview) as a child process, communicating over its stream-json protocol. SQL tools are exposed to Claude via an MCP server that routes queries back through SSMS's own connection, so you get the same authentication, server context, and results grid you're already using.

All tool calls require your approval — Claude proposes actions, you decide what runs.

## Feedback and issues

Found a bug or have a feature request? Open an issue on the [issue tracker](https://github.com/ado-webco/ssms-agentic/issues).
