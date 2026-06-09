# Zoho Billing MCP: Connect Zoho Billing to AI Agents

Use [Zoho Billing](https://www.zoho.com/billing/) with AI agents through the [Model Context Protocol (MCP)](https://www.zoho.com/mcp/). This repository explains how to set up a Zoho Billing MCP server, connect MCP clients like Claude and ChatGPT, and safely automate billing workflows.

Zoho Billing MCP connects your AI agent to your Zoho Billing organization. With this integration, your agent can query, create, and manage billing data from natural-language prompts while following your configured tools, permissions, and audit controls.

Instead of manually navigating dashboards or writing custom API scripts, you can ask your AI assistant to create invoices, retrieve overdue payments, update subscriptions, and generate billing reports.

## Table of Contents

- [What Is Zoho Billing MCP?](#what-is-zoho-billing-mcp)
- [How Zoho Billing MCP Works](#how-zoho-billing-mcp-works)
- [Supported MCP Clients](#supported-mcp-clients)
- [Prerequisites](#prerequisites)
- [Setup Guide](#setup-guide)
- [Activity Logs](#activity-logs)
- [Managing Connections](#managing-connections)
- [Regenerate API Key](#regenerate-api-key)
- [Example Prompts](#example-prompts)
- [Prompt Best Practices](#prompt-best-practices)
- [FAQ](#faq)
- [Resources](#resources)
- [License](#license)

## What Is Zoho Billing MCP?

Zoho Billing MCP is an MCP server integration that exposes Zoho Billing actions as tools for AI agents.

| Component | Description |
|-----------|-------------|
| **MCP Server** | A configured server that exposes tools (for example: Create Invoice, Get Contact Person, Update Subscription). |
| **MCP Client** | The AI assistant that receives your prompts (Claude, ChatGPT, Cursor, VS Code, and others). |
| **Tools** | Allowed Zoho Billing actions your AI agent can perform, selected in Zoho MCP. |

> **Note:** Every AI-triggered action is subject to Zoho API limits, user permissions, and audit logs.

## How Zoho Billing MCP Works

```text
You (Prompt) -> AI Agent (MCP Client) -> Zoho MCP Server -> Zoho Billing API -> Action Executed
```

1. You enter a prompt, for example: "In [Org ID], create an invoice for Zylker Corp for $500."
2. The AI agent selects the correct enabled tool.
3. The request is sent to the Zoho MCP server.
4. The server maps the request to the correct Zoho Billing API call.
5. The action is completed in your Zoho Billing account.

## Supported MCP Clients

Zoho MCP works with MCP-compatible clients, including:

- [Claude](https://claude.ai) (Web and Desktop)
- ChatGPT
- Codex
- Cursor
- Windsurf
- VS Code (GitHub Copilot in Agent mode)
- Zed
- Continue
- Cline
- n8n

## Prerequisites

- An active Zoho Billing organization
- Account-level access to that organization
- An MCP client (Claude, ChatGPT, Cursor, or similar)
- For **Claude**: Organization Admin access (Team plan or higher)
- For **ChatGPT**: Plus, Pro, or Team plan

> **Note:** Plan and role requirements may change. Confirm the latest eligibility in [Claude documentation](https://support.claude.com) and [ChatGPT pricing](https://openai.com/pricing).

## Setup Guide

### 1. Generate Your MCP Server URL

1. Sign in to [Zoho MCP](https://www.zoho.com/mcp/) with your Zoho Billing credentials.
2. Create or select an MCP server.
3. Open **Connect** in the left sidebar.
4. Copy the displayed **Server URL**.

Server URL format:

```text
https://[mcp-server-name]-[org-id].zohomcp.com/mcp/[api-key]/message
```

> **Warning:** Treat your Server URL like a password. It grants access to Zoho Billing tools and data. Do not share it publicly. If exposed, regenerate it immediately.

### 2. Configure Tools

Each tool maps to a Zoho Billing API action. Enable only the actions your AI workflows require.

1. Sign in to Zoho MCP.
2. Select your MCP server.
3. Open **Tools**.
4. Enable the required actions.

[Learn more about configuring tools](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Manage_Tools)

### 3. Connect Your MCP Client

#### Claude

1. Open [claude.ai/settings/connectors](https://claude.ai/settings/connectors) or Claude Desktop -> **Settings** -> **Connectors**.
2. Select **Add Custom Connector**.
3. Enter a connector name (for example, "Zoho Billing").
4. Paste your MCP Server URL and select **Save**.
5. Select **Connect**.
6. Complete authorization.

#### ChatGPT

1. Sign in to ChatGPT.
2. Open **Settings** from your profile menu.
3. Open **Apps** > **Advanced Settings**.
4. Enable **Developer Mode** and select **Create App**.
5. Enter a name, paste your MCP Server URL, and set authentication to **OAuth**.
6. Confirm trust and create the app.

#### Other MCP Clients (Cursor, Windsurf, VS Code, and more)

[Follow the general MCP client setup guide](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Connect_the_Zoho_MCP_Server_with_Your_MCP_Client)

## Activity Logs

All AI tool calls are logged in Zoho MCP.

1. Sign in to Zoho MCP.
2. Select your MCP server.
3. Open **Logs**.
4. Filter by Date, Time, Status, Tool, ZUID, or Execution ID.

| Field | Description |
|-------|-------------|
| **Tool** | Zoho Billing action invoked (for example, Create Invoice). |
| **Status** | Success or failure state of the tool call. |
| **ZUID** | Zoho User ID that initiated the action through an AI client. |
| **Execution ID** | Unique identifier for tracing and debugging. |

## Managing Connections

Connections provide OAuth credentials so your AI agent can access Zoho Billing.

- User-managed: each user authorizes access the first time they run tools.
- Admin-managed: one shared organization connection is used for tool calls.

[Learn more about managing connections](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Manage_Your_Zoho_MCP_Server_Connections)

## Regenerate API Key

If your Server URL is compromised, regenerate your API key immediately.

1. Open **Connect** in Zoho MCP.
2. Select **Regenerate API Key**.
3. Copy the new Server URL.
4. Update all connected MCP clients.

> **Note:** Regenerating the key invalidates the previous URL and disconnects existing MCP client integrations until updated.

## Example Prompts

### Invoices

- "In [Org Name] (Org ID), create an invoice for Zylker Corp with three items: Professional Services at $500, Support at $200, and Setup at $100."
- "In [Org Name] (Org ID), collect payment for John's invoice using his saved bank account."

### Subscriptions

- "In [Org Name] (Org ID), create a new subscription for Zylker Corp on the Growth plan."

### Reports

- "In [Org Name] (Org ID), get the ARR report for Q1 2025 and summarize month-over-month trends."
- "In [Org Name] (Org ID), get the AR aging summary report for January and list customers with outstanding invoices."

### Quotes and Plans

- "In [Org Name] (Org ID), create a quote for Acme Corp for Professional Services, Support, and Onboarding."
- "In [Org Name] (Org ID), create a new plan named Business Pro, billed monthly at $99."

### Payments

- "In [Org Name] (Org ID), record a payment of $500 from Zylker Corp against their latest invoice."
- "In [Org Name] (Org ID), create a payment link for Sarah for her outstanding invoice."

> **Tip:** Enable only the tools needed for the current task. This improves response accuracy and reduces unintended actions.

## Prompt Best Practices

- Use exact module names, field names, and record IDs from Zoho Billing.
- Reference records by unique IDs to avoid ambiguity.
- Clearly specify intent, target records, and conditions.
- Include edge-case handling in the initial prompt.

## FAQ

### Is Zoho Billing MCP secure?

Yes. Security depends on your configured permissions, enabled tools, OAuth connections, and how safely you handle your MCP Server URL.

### Can I use Zoho Billing MCP with clients other than Claude and ChatGPT?

Yes. Any MCP-compatible client can work, including Cursor, VS Code, and other custom MCP clients.

### What happens if I regenerate the API key?

The previous Server URL stops working immediately, and all clients must be updated with the new URL.

## Resources

- [Zoho Billing Help Documentation](https://www.zoho.com/billing/help/)
- [Zoho MCP Documentation](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation)
- [Zoho Billing API](https://www.zoho.com/billing/api/v1/)
- [Zoho Billing Community Forum](https://help.zoho.com/portal/en/community/zoho-billing)

## License

Copyright (c) Zoho Corporation Pvt. Ltd. All rights reserved.
