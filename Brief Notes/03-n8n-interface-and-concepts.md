# Understanding n8n Interface & Core Concepts

## Concept Explanation

n8n is built around a **workflow canvas** where **nodes** are connected to perform tasks. Understanding the interface and core concepts is key before creating complex automations.

## Core Concepts

**1. Workflow*
*
- A **workflow** is a series of connected nodes that automate tasks.
- Workflows **start with a trigger** and flow through **action nodes.**

**2. Node**

- A node is a **building block** of a workflow.
- Types of nodes:
  - **Trigger Nodes**: Start workflows (e.g., Webhook, Cron).
  - **Action Nodes**: Perform tasks (e.g., send email, update sheet).
  - **Utility Nodes**: Handle data or logic (e.g., Set, Function, Merge).

**3. Connections**

- Nodes are connected by **lines** that represent **data flow**.
- Data passes from one node to another in **JSON format**.
  
**4. Credentials**

- Securely store authentication info for external apps (e.g., Google Sheets, Slack).
- Reusable across workflows.

**5. Execution Panel**

- Displays workflow run results, including successes, errors, and logs.

## n8n Interface Overview

| Section                      | Description                                          |
| ---------------------------- | ---------------------------------------------------- |
| **Workflow Canvas**          | Central area to add and connect nodes                |
| **Node Sidebar**             | Search and add available nodes                       |
| **Node Configuration Panel** | Configure selected node’s parameters & credentials   |
| **Execution Panel**          | Shows workflow run results and logs                  |
| **Top Toolbar**              | Save, undo/redo, workflow settings, execute workflow |

## Practical Example: Simple Workflow

**Scenario:** When you receive a new email in Gmail, save the sender and subject to Google Sheets.

**Workflow Structure:**
**1. Trigger:** Gmail node → detects new email
**2. Action:** Set node → format sender & subject
**3. Action:** Google Sheets node → save data

**Workflow JSON Example:**

```json
{
  "nodes": [
    {
      "name": "Gmail Trigger",
      "type": "n8n-nodes-base.gmailTrigger",
      "parameters": {
        "label": "INBOX",
        "criteria": "UNREAD"
      }
    },
    {
      "name": "Set Data",
      "type": "n8n-nodes-base.set",
      "parameters": {
        "values": [
          {"name": "Sender", "value": "={{$json[\"from\"]}}"},
          {"name": "Subject", "value": "={{$json[\"subject\"]}}"}
        ]
      }
    },
    {
      "name": "Google Sheets",
      "type": "n8n-nodes-base.googleSheets",
      "parameters": {
        "operation": "append",
        "sheetId": "your-sheet-id",
        "range": "Sheet1!A:B"
      }
    }
  ],
  "connections": {
    "Gmail Trigger": {"main":[[{"node":"Set Data","type":"main","index":0}]]},
    "Set Data": {"main":[[{"node":"Google Sheets","type":"main","index":0}]]}
  }
}
```

## Best Practices & Tips

- **Name nodes clearly** → e.g., “Save Email to Sheets”
- **Use Set nodes** for data formatting before sending to other nodes
- **Test nodes individually** before connecting full workflow
- **Store credentials securely** and reuse across workflows
