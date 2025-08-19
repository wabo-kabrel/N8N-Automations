# Data Handling & Expressions in n8n
## Concept Explanation
### 1. Understanding Data in n8n
- All data in n8n flows between nodes as JSON objects.
- Each node receives input data, processes it, and sends output data to the next node.
- Example JSON from a webhook: 
```json
{
  "body": {
    "name": "John Doe",
    "email": "john@example.com",
    "order_total": 120
  },
  "headers": {
    "content-type": "application/json"
  }
}
```
### 2. The Role of Expressions
- **Expressions** are used to **dynamically access and manipulate data** from previous nodes.
- They are written in **double curly braces** `{{ }}`.
- Example: Access `name` from a webhook node called “Webhook”:
```scss
{{$json["body"]["name"]}}
```
- This value can be used in Slack messages, Google Sheets, emails, or anywhere in n8n.

### 3. Expression Editor
- Click the gear icon next to a field in a node → Add Expression
- You can access:
  - `$json` → the node’s JSON data
  - `$node["Node Name"].json` → data from another node
  - `$now` → current timestamp
  - `$workflow` → workflow information

### 4. Data Handling Techniques

| Task                        | Node/Method             | Example                                                                         |
| --------------------------- | ----------------------- | ------------------------------------------------------------------------------- |
| Create custom data          | **Set Node**            | Set `fullName = {{$json["body"]["name"]}}`                                      |
| Conditional logic           | **IF Node**             | `{{$json["body"]["order_total"] > 100}}`                                        |
| Merge multiple data sources | **Merge Node**          | Combine API response with database data                                         |
| Loop through items          | **SplitInBatches Node** | Process 10 items at a time                                                      |
| Transform data              | **Function Node**       | Custom JS: `items[0].json.total = items[0].json.price * items[0].json.quantity` |


## Practical Example: Dynamic Slack Notification
**Scenario:** Send a Slack message whenever a new order exceeds $100. Include the customer name and order total.

### Workflow Nodes:
**1. Webhook Trigger** → receives order data
**2. IF Node** → checks if order_total > 100
**3. Slack Node** → sends message using expressions

### Slack Node Message Example:
```scss
New high-value order from {{$json["body"]["name"]}}! Total: ${{$json["body"]["order_total"]}}
```
- The `{{$json["body"]["name"]}}` dynamically pulls the customer’s name.
- `$json` refers to the **input data** from the **previous node**.


## Best Practices & Tips
- Use expressions **everywhere you need dynamic data** → avoids hardcoding values
- Test expressions in **Set or Function** nodes before using in external apps
- Use IF nodes **with expressions** for clear conditional workflows
- Keep expressions readable → add comments or use intermediate **Set nodes** for clarity