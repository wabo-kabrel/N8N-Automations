# Introduction to Automation & n8n
## Concept Explanation
Automation is about making computers handle repetitive tasks for you. Instead of manually copying data between apps, you build workflows that run automatically.  
- **Traditional Way**: Manually export data from Gmail → paste into Google Sheets → notify on Slack.
- **With Automation (n8n)**: New email arrives → workflow detects it → extracts details → auto-saves into Google Sheets → sends Slack notification.
  
**n8n** is an **open-source workflow automation tool**.
It is built on **nodes** that forms **workflows**, and lets you connect **APIs, apps,** and **services** without **heavy** coding.

## Practical Example
Imagine you run a small online store. Every time someone makes a purchase:
1. Save order details to Google Sheets.
2. Send a confirmation email.
3. Notify your Slack team.

With n8n, this becomes an **automated workflow** that runs instantly.

## Code/Workflow Example
Here’s how a basic workflow JSON looks in n8n:
```json
{
  "nodes": [
    {
      "parameters": {
        "path": "webhook-test"
      },
      "name": "Webhook Trigger",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 1
    },
    {
      "parameters": {
        "channel": "#orders",
        "text": "New order received!"
      },
      "name": "Slack Notification",
      "type": "n8n-nodes-base.slack",
      "typeVersion": 1
    }
  ],
  "connections": {
    "Webhook Trigger": {
      "main": [
        [
          {
            "node": "Slack Notification",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  }
}
```
- **Trigger**: Webhook (receives order data)
- **Action**: Slack (sends a message)