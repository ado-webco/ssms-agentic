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
- **Claude Code CLI** installed and authenticated — see below
- Windows (amd64)

## Setting up the Claude CLI

SsmsAgentic launches the [Claude Code CLI](https://code.claude.com/docs/en/quickstart) as a child process, so SSMS must be able to find `claude` on the system `PATH`. The native Windows installer adds it for you automatically.

1. **Install [Git for Windows](https://git-scm.com/downloads/win)** if you don't already have it — the native Claude installer depends on it.
2. **Install the CLI.** Open a terminal (no admin needed) and run one of the official one-liners:

   PowerShell:
   ```powershell
   irm https://claude.ai/install.ps1 | iex
   ```
   Command Prompt:
   ```
   curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
   ```
   Other install methods (WinGet, Homebrew, WSL) are listed in the [official quickstart](https://code.claude.com/docs/en/quickstart).
3. **Verify it's on `PATH`.** Open a *new* terminal so it picks up the updated environment, then run:
   ```
   claude --version
   ```
4. **Authenticate.** Run `claude` once and follow the prompts to sign in. SsmsAgentic uses the same credentials.
5. **Restart SSMS.** SSMS reads `PATH` only at process start, so any already-running instances need to be closed and reopened after the install. If the chat pane reports that `claude` cannot be found, this is almost always the cause.

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

SsmsAgentic hosts the [Claude Code CLI](https://code.claude.com/docs/en/quickstart) as a child process, communicating over its stream-json protocol. SQL tools are exposed to Claude via an MCP server that routes queries back through SSMS's own connection, so the same authentication and server context you're already using applies to every query Claude runs.

All tool calls require your approval — Claude proposes actions, you decide what runs.

## Feedback and issues

Found a bug or have a feature request? Open an issue on the [issue tracker](https://github.com/ado-webco/ssms-agentic/issues).
