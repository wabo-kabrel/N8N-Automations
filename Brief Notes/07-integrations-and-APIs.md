# Integrations & APIs in n8n

## Concept Explanation  

### 1. What Are Integrations?

- Integrations are connections between n8n and external services (apps, APIs, databases).  
- They allow workflows to:  
  - Read data from apps
  - Send data to apps
  - Automate cross-app processes  

Examples:

- Gmail → Slack → Google Sheets
- Shopify → Airtable → Email notification

### 2. APIs in n8n

- n8n connects to external apps using **APIs** (Application Programming Interfaces).
- API Basics:
  - **Endpoint** → URL to access data [API endpoint](https://api.example.com/orders)
  - **Method** → GET, POST, PUT, DELETE
  - **Authentication** → API Key, OAuth2, Basic Auth
- n8n provides pre-built **nodes** for many popular apps: Gmail, Slack, Shopify, Airtable, Google Sheets, Twitter, etc.
- If a pre-built node doesn’t exist → use **HTTP Request node** to call APIs manually.  

### 3. Authentication & Credentials

- Most services require credentials:
  - **API Key / Token** → Simple string to access API  
  - **OAuth2** → Authorization protocol for secure access  
  - **Username & Password** → Basic auth for older services  
- In n8n:
  1. Go to **Credentials** → **New Credentials**
  2. Select app type (e.g., Google Sheets)
  3. Authenticate with the service
  4. Reuse credentials across workflows

### 4. HTTP Request Node

- Use this when:
  - No pre-built node exists
  - Custom API endpoints are needed

Example: **GET request to a fake API**

```json
{
  "nodes": [
    {
      "name": "HTTP Request",
      "type": "n8n-nodes-base.httpRequest",
      "parameters": {
        "url": "https://jsonplaceholder.typicode.com/posts/1",
        "method": "GET",
        "responseFormat": "json"
      }
    },
    {
      "name": "Set Data",
      "type": "n8n-nodes-base.set",
      "parameters": {
        "values": [
          {"name": "title", "value": "={{$json[\"title\"]}}"},
          {"name": "body", "value": "={{$json[\"body\"]}}"}
        ]
      }
    }
  ],
  "connections": {
    "HTTP Request": {"main":[[{"node":"Set Data"}]]}
  }
}
```

- `$json["title"]` and `$json["body"]` dynamically extract data from API response.
  
## Practical Example: Shopify → Slack

**Scenario:** When a new Shopify order is created, notify your Slack channel.

1. **Trigger:** Shopify Trigger Node → New Order
2. **Set Node:** Extract order details (customer name, total)
3. **Slack Node:** Send message to channel:

```scss
New order received! Customer: {{$json["customer"]["first_name"]}} {{$json["customer"]["last_name"]}} - Total: ${{$json["total_price"]}}
```

- Workflow dynamically updates with every new order.

## Best Practices & Tips

- Use **pre-built nodes** when possible → simplifies workflows and handles authentication automatically
- Test API calls using **HTTP Request node** in development
- Always **store credentials securely** in n8n
- Use **Set or Function nodes** to clean API data before sending it to other apps
- Limit API requests to avoid **rate limiting**
