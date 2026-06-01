# Zoho Billing MCP

Connect [Zoho Billing](https://www.zoho.com/billing/) to AI agents using the [Model Context Protocol (MCP)](https://www.zoho.com/mcp/).

Zoho Billing MCP lets you connect your AI agent to your Zoho Billing organization using the Model Context Protocol (MCP), an open standard that lets AI models interact with external tools and data sources in a structured, secure way.

Once configured, your AI agent can query, create, and manage billing data directly from your prompts. When you prompt your AI agent which is the MCP client, the request flows through the MCP server, which maps it to the appropriate Zoho Billing API call and returns the result back to your agent.

This means instead of manually navigating the Zoho dashboard or writing custom API scripts, you can ask your agent to pull overdue invoices, create a new subscription plan, or reconcile charges — and it will handle the API calls for you.

## Key Concepts

| Component | Description |
|-----------|-------------|
| **MCP Server** | The configured server that exposes a set of tools (e.g., Create Invoice, Get Contact Person, Update Subscription) to your AI agent. |
| **MCP Client** | The AI agent (Claude, ChatGPT, Cursor, etc.) that receives and processes your prompts. |
| **Tools** | The specific actions your AI agent is permitted to perform in Zoho Billing. You select tools from the available list in Zoho MCP. |

> **Note:** Every action your AI agent performs is subject to API limits, permissions, and audit logs.


## How It Works

```
You (prompt) → AI Agent (MCP Client) → Zoho MCP Server → Zoho Billing API → Action executed
```

1. You type a prompt in your AI agent's interface — e.g., *"In [Org ID], create an invoice for Zylker Corp for $500."*
2. The AI agent identifies the right tool from the enabled tools on your MCP server.
3. The AI agent sends the request to the Zoho MCP server.
4. The MCP server calls the Zoho Billing API to carry out the action.
5. The action is executed in your Zoho Billing account.

## Supported MCP Clients

Zoho MCP works with any MCP-compatible client.

- [Claude](https://claude.ai) (claude.ai / Desktop)

Also compatible with:

- Codex
- Cursor
- Windsurf
- VS Code (GitHub Copilot in Agent mode)
- Zed
- Continue
- Cline
- n8n

## Prerequisites

- You must be a user in an active Zoho Billing organization.
- Account-level access to your Zoho Billing organization.
- MCP client installed and ready to use (e.g., Claude, ChatGPT, Cursor).
- For **Claude**: You must be a Claude Organization Admin (Team plan or higher) to connect the integration.
- For **ChatGPT**: A ChatGPT Plus, Pro, or Team plan is required.

> **Note:** Plan and role requirements may change. Always verify the latest requirements in [Claude's documentation](https://support.claude.com) or [ChatGPT's pricing page](https://openai.com/pricing) before getting started.

## Setup

### 1. Generate Your Server URL

1. Log in to [Zoho MCP](https://www.zoho.com/mcp/) using your Zoho Billing account credentials.
2. Create or select an MCP server.
3. Go to **Connect** in the left sidebar.
4. Copy the **Server URL** shown on the page.

Your Server URL follows this format:

```
https://[mcp-server-name]-[org-id].zohomcp.com/mcp/[api-key]/message
```

> **⚠️ Warning:** Treat your Server URL like a password. It grants access to your Zoho Billing tools and data. Do not share it in public repositories, shared documents, or anywhere others can view it. If compromised, regenerate it immediately.

### 2. Configure Tools

Tools are the building blocks of everything your AI agent can do. Each tool maps to a specific Zoho Billing API call. With Tools, you can choose exactly which actions the AI agent is permitted to perform. 

1. Log in to your Zoho MCP account.
2. Select your MCP server.
3. Go to **Tools** and enable the actions you need.

[Learn more about configuring tools →](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Manage_Tools)

### 3. Connect Your MCP Client

#### Claude

1. Go to [claude.ai/settings/connectors](https://claude.ai/settings/connectors), or open the Claude desktop app → **Settings** → **Connectors**.
2. Click **Add Custom Connector**.
3. Enter a name (e.g., "Zoho Billing").
4. Paste the Server URL and click **Save**.
5. Scroll to the connector and click **Connect**.
6. Review and accept the authorization screens.

#### ChatGPT

1. Log in to ChatGPT.
2. Click your profile icon and go to **Settings**.
3. Go to the **Apps** tab > **Advanced Settings**.
4. Toggle **Developer Mode** and click **Create App**.
5. Enter a name, paste the MCP Server URL, set Authentication to **OAuth**.
6. Check *I trust this application* and click **Create**.

#### Custom MCP Clients (Cursor, Windsurf, VS Code, etc.)

[Follow the general MCP client setup guide →](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Connect_the_Zoho_MCP_Server_with_Your_MCP_Client)

## Activity Logs

Every action your AI agent performs is logged. You can view logs from the Zoho MCP dashboard:

1. Log in to your Zoho MCP account.
2. Select the MCP server.
3. Go to **Logs** in the left sidebar to view the logs.
4. You can filter by Date, Time, Status, Tool, ZUID, or Execution ID.

Each log entry captures the following fields.

| Field | Description |
|-------|-------------|
| **Tool** | The Zoho Billing action that was called (e.g., Create Invoice). |
| **Status** | Whether the tool call succeeded or failed. |
| **ZUID** | The Zoho User ID of the person whose AI agent triggered the action. |
| **Execution ID** | A unique identifier for tracing and debugging. |

## Managing Connections

Connections provide your AI agent with the access credentials (OAuth tokens) needed to interact with Zoho Billing. You can:

- Let each user authorize access individually when they first use a tool.
- Set up a single admin-managed connection for all tool calls across the organization.

[Learn more about managing connections →](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation#Manage_Your_Zoho_MCP_Server_Connections)

## Regenerating Your API Key

If your Server URL is exposed or compromised, regenerate your API key immediately. This invalidates the old URL and prevents anyone who has it from accessing your Zoho Billing data.

1. Go to **Connect** module in the left sidebar.
2. Click **Regenerate API Key**.
3. Copy the new Server URL and update it in all connected MCP clients.

> **Note:** Regenerating the API key disconnects all existing MCP client integrations. You'll need to reconfigure each client with the new URL.

## Example Prompts

### Invoices

- *"In [Org Name] (Org ID), create an invoice for Zylker Corp with three items — Professional Services at $500, Support at $200, and Setup at $100."*
- *"In [Org Name] (Org ID), collect payment for John's invoice using his saved bank account."*

### Subscriptions

- *"In [Org Name] (Org ID), set up a new subscription for Zylker Corp on the Growth plan."*

### Reports

- *"In [Org Name] (Org ID), get the ARR report for Q1 2025 and summarize the month-on-month trend."*
- *"In [Org Name] (Org ID), get the AR aging summary report for January and list all customers with outstanding invoices."*

### Quotes & Plans

- *"In [Org Name] (Org ID), create a quote for Acme Corp for Professional Services, Support, and Onboarding."*
- *"In [Org Name] (Org ID), create a new plan called Business Pro, billed monthly at $99."*

### Payments

- *"In [Org Name] (Org ID), record a payment of $500 from Zylker Corp against their latest invoice."*
- *"In [Org Name] (Org ID), create a payment link for Sarah for her outstanding invoice."*

> **Pro Tip:** Select only the tools you need for the current task before starting a chat. This keeps responses focused and reduces unintended actions.

## Prompt Best Practices

- **Name things specifically** — Use exact field names, IDs, and module names as they appear in Zoho Billing.
- **Reference records by unique IDs** — avoid ambiguity about which customer, invoice, or plan you mean.
- **State your intent clearly** — what you want done, to what, and under what conditions.
- **Include edge cases upfront** — if something should only happen when a condition is met, say so.

## Resources

- [Zoho Billing Help Documentation](https://www.zoho.com/billing/help/)
- [Zoho MCP Documentation](https://help.zoho.com/portal/en/kb/mcp/getting-started/articles/zoho-mcp-help-documentation)
- [Zoho Billing API](https://www.zoho.com/billing/api/v1/)
- [Community Forum](https://help.zoho.com/portal/en/community/zoho-billing)

## License

© Zoho Corporation Pvt. Ltd. All Rights Reserved.
