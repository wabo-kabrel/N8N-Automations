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