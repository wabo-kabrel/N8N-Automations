# Conditional Logic & Branching

## Concept Explanation

### 1. What is Conditional Logic?

- Conditional logic allows workflows to make decisions based on data.
- Example: “If an order total > $100, send a VIP alert; else, just log the order.”

### 2. 2. IF Node

- The **IF Node** is the main tool for branching in n8n.
- It evaluates conditions and creates **two paths:**
  - **True**→ executed if condition passes
  - **False**→ executed if condition fails

#### Key Points:

- Multiple conditions can be combined with **AND / OR**
- You can compare numbers, strings, booleans, or dates

### 3. Switch Node

- The **Switch Node** is used when you have multiple conditions to route data to different paths.
- Example: Route orders based on payment method:
  
  - Credit Card → Node A
  - Credit Card → Node A
  - PayPal → Node B
  
### 4. Merge Node

- The **Merge Node** combines data from different branches back into a single workflow.
- The Merge Node combines data from different branches back into a single workflow.

#### Merge Modes:

- **Append** → combine items sequentially
- **Wait** → waits for all branches to finish before continuing

## Practical Example: Order Workflow with Conditional 

**Scenario:** Process new orders based on order value:

**1. Webhook Trigger:** Receive order data
**2. IF Node:** Check if order_total > 100
**3. True Branch:** Slack Node → send VIP order alert
**4. False Branch:** Google Sheets Node → log order
**5. Merge Node:** Combine both paths for final reporting

**IF Node Expression Example:**

```text
{{$json["order_total"] > 100}}
```

- Evaluates order total dynamically from the webhook data

**Workflow JSON Snippet:**

```json
{
  "nodes": [
    {"name":"Webhook","type":"n8n-nodes-base.webhook"},
    {"name":"Check High Value","type":"n8n-nodes-base.if","parameters":{"conditions":[{"value1":"={{$json[\"order_total\"]}}","operation":"greater","value2":100}]}},
    {"name":"Slack VIP Alert","type":"n8n-nodes-base.slack"},
    {"name":"Log Order","type":"n8n-nodes-base.googleSheets"},
    {"name":"Merge Branches","type":"n8n-nodes-base.merge","parameters":{"mode":"append"}}
  ],
  "connections":{
    "Webhook":{"main":[[{"node":"Check High Value"}]]},
    "Check High Value":{"main":[[{"node":"Slack VIP Alert"}],[{"node":"Log Order"}]]},
    "Slack VIP Alert":{"main":[[{"node":"Merge Branches"}]]},
    "Log Order":{"main":[[{"node":"Merge Branches"}]]}
  }
}
```

## Best Practices & Tips

- Use **IF Nodes** for simple true/false decisions
- Use **Switch Nodes** for multiple routing options
- Use **Merge Nodes** to combine results from branches
- Keep conditions **simple and readable**
- Test each branch independently before merging

