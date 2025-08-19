# Triggers & Nodes Deep Dive
## Concept Explanation
### 1. Triggers
A **trigger** starts a workflow automatically. It can be thought of as the **start button**

#### Type of Triggers

| Trigger Type              | Description                                      | Example Use Case             |
| ------------------------- | ------------------------------------------------ | ---------------------------- |
| **Webhook**               | Starts workflow when an HTTP request is received | Receive form submissions     |
| **Cron**                  | Runs workflow on a schedule                      | Daily report generation      |
| **App-specific triggers** | e.g., Gmail Trigger, Slack Trigger               | New email, new Slack message |
| **Manual Trigger**        | Starts workflow manually                         | Testing or debugging         |

#### Key points about trigger:
- Only **one trigger per workflow** is allowed.
- They define **when the workflow runs.**
- Most workflows start with a **Webhook** for real-time automation.


### 2. Action Nodes
Action nodes **perform tasks** in the workflow.
#### Examples:
- **Google Sheets** → append, update, or read data
- **Slack** → send messages
- **HTTP Request** → call APIs
- **Set / Function** → manipulate data
  
#### Key points:
- Action nodes process the input data from previous nodes.
- Nodes can output JSON to pass data along the workflow.
  

### 3. Utility Nodes
Utility nodes help **transform, filter, or combine data.**

| Node         | Purpose                       | Example                                   |
| ------------ | ----------------------------- | ----------------------------------------- |
| **Set**      | Create or modify data         | Add custom fields before saving to Sheets |
| **Merge**    | Combine multiple node outputs | Merge CRM and Email data                  |
| **IF**       | Conditional branching         | Send Slack alert only if order > \$100    |
| **Function** | Custom JavaScript logic       | Calculate totals, format strings          |


## Practical Example: Multi-Node Workflow
**Scenario:** When a new order is received via a webhook:  
**1. Webhook Trigger** → receives order data  
**2. Set Node** → format order data  
**3. IF Node** → check if order total > $100       
**4. Slack Node** → send alert if order > $100  
**5. Google Sheets Node** → log order in a spreadsheet  

**Workflow JSON Snippet:**
```json
{
  "nodes": [
    {"name": "Webhook", "type": "n8n-nodes-base.webhook"},
    {"name": "Set Order Data", "type": "n8n-nodes-base.set"},
    {"name": "Check High Value", "type": "n8n-nodes-base.if"},
    {"name": "Slack Alert", "type": "n8n-nodes-base.slack"},
    {"name": "Google Sheets Log", "type": "n8n-nodes-base.googleSheets"}
  ],
  "connections": {
    "Webhook": {"main":[[{"node":"Set Order Data"}]]},
    "Set Order Data": {"main":[[{"node":"Check High Value"}]]},
    "Check High Value": {"main":[[{"node":"Slack Alert","type":"main","index":0}],[{"node":"Google Sheets Log","type":"main","index":1}]]}
  }
}
```

## Best Practices & Tips
- **Always name nodes descriptively** → e.g., “Check High Value Orders”
- **Test triggers first** to ensure workflow starts correctly
- **Use Set nodes early** to format or clean data
- **IF nodes for conditional logic** → avoids unnecessary actions
- **Keep workflows modular** → split large workflows into smaller ones