# Triggers & Nodes Deep Dive
## 1. Triggers
A **trigger** starts a workflow automatically. It can be thought of as the **start button**

### Type of Triggers

| Trigger Type              | Description                                      | Example Use Case             |
| ------------------------- | ------------------------------------------------ | ---------------------------- |
| **Webhook**               | Starts workflow when an HTTP request is received | Receive form submissions     |
| **Cron**                  | Runs workflow on a schedule                      | Daily report generation      |
| **App-specific triggers** | e.g., Gmail Trigger, Slack Trigger               | New email, new Slack message |
| **Manual Trigger**        | Starts workflow manually                         | Testing or debugging         |

### Key points about trigger:
- Only **one trigger per workflow** is allowed.
- They define **when the workflow runs.**
- Most workflows start with a **Webhook** for real-time automation.


## 2. Action Nodes
Action nodes **perform tasks** in the workflow.
### Examples:
- **Google Sheets** → append, update, or read data
- **Slack** → send messages
- **HTTP Request** → call APIs
- **Set / Function** → manipulate data
  
### Key points:
- Action nodes process the input data from previous nodes.
- Nodes can output JSON to pass data along the workflow.
  

## 3. Utility Nodes
Utility nodes help **transform, filter, or combine data.**

| Node         | Purpose                       | Example                                   |
| ------------ | ----------------------------- | ----------------------------------------- |
| **Set**      | Create or modify data         | Add custom fields before saving to Sheets |
| **Merge**    | Combine multiple node outputs | Merge CRM and Email data                  |
| **IF**       | Conditional branching         | Send Slack alert only if order > \$100    |
| **Function** | Custom JavaScript logic       | Calculate totals, format strings          |


## Practical Example: Multi-Node Workflow
**1. Scenario:** When a new order is received via a webhook:
**2. Webhook Trigger** → receives order data
**3. Set Node** → format order data
**4. IF Node** → check if order total > $100     
**5. Slack Node** → send alert if order > $100
**6. Google Sheets Node** → log order in a spreadsheet