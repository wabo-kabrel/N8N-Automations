# Looping & Batch Processing

## Concept Explanation

### 1. Why Looping & Batch Processing Matters

- Some workflows deal with **hundreds or thousands of items** (orders, leads, emails).
- Processing all at once can **overwhelm APIs** or **slow down your workflow**.
- n8n provides **nodes to handle multiple items efficiently**.

### 2. SplitInBatches Node

- Splits incoming data into smaller chunks (batches).
- Example: 1000 orders → process **50 at a time**.
- Prevents **timeouts** and **rate-limit issues**.

#### Key Parameters:

- **Batch Size** → number of items per batch
- **Continue on Fail** → optional, workflow continues if a batch fails

### 3. Looping with Function Node

- The **Function Node** allows custom loops using JavaScript.
- Example: Loop through an array of items and transform data before sending to another node.

```javascript
// Loop through items and add a new field
for (let i = 0; i < items.length; i++) {
  items[i].json.fullName = items[i].json.firstName + ' ' + items[i].json.lastName;
}
return items;
```

### 4. Handling Large API Responses

- Use **SplitInBatches** to process large datasets in chunks.
- Combine results at the end using **Merge Node**.
- Example: Fetch 1000 rows from an API, process 50 at a time, then merge results for reporting.

### 5. Workflow Example: Bulk Email Sending

**Scenario:** Send emails to 500 users using batches of 50.

**1. Trigger:** Webhook or Scheduled Cron
**2. HTTP Request / Database Node:** Fetch all users
**3. SplitInBatches Node:** Batch size = 50
**4. Email Node:** Send email to each batch
**5. Merge Node:** Combine results for logs

## Practical Example: Batch Processing Orders

### Workflow:

- **Webhook Trigger** → receives 1000 orders
- **SplitInBatches Node** → batch of 50 orders
- **Function Node** → format order data for Slack
- **Slack Node** → send notification for each batch
- **Merge Node** → combine batch results

### SplitInBatches Node JSON Snippet:

```json
{
  "nodes": [
    {"name":"Webhook","type":"n8n-nodes-base.webhook"},
    {"name":"Split Orders","type":"n8n-nodes-base.splitInBatches","parameters":{"batchSize":50}},
    {"name":"Format Orders","type":"n8n-nodes-base.function","parameters":{"functionCode":"for (let i = 0; i < items.length; i++) { items[i].json.message = `Order from ${items[i].json.customerName}`; } return items;"}},
    {"name":"Slack Notification","type":"n8n-nodes-base.slack"},
    {"name":"Merge Results","type":"n8n-nodes-base.merge","parameters":{"mode":"append"}}
  ],
  "connections":{
    "Webhook":{"main":[[{"node":"Split Orders"}]]},
    "Split Orders":{"main":[[{"node":"Format Orders"}]]},
    "Format Orders":{"main":[[{"node":"Slack Notification"}]]},
    "Slack Notification":{"main":[[{"node":"Merge Results"}]]}
  }
}
```

## Best Practices & Tips

- **Use SplitInBatches for large datasets** → prevents timeouts & rate-limit issues
- **Merge results** after processing batches for consolidated logs or reporting
- **Test with small batches first** → ensure workflow logic is correct
- **Use Function Nodes** to transform or enrich data in each batch
- Avoid sending too many **API calls at once** → respect external service limits

