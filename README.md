# Topic 11: Database Design for Automation (SQLite, PostgreSQL & MySQL)

## 1. Overview

This project demonstrates relational database concepts using n8n. It simulates an order management system that creates customer and order records, inserts order line items, validates customer credit limits, handles transaction rollbacks, and retrieves consolidated order information.

The workflow uses JavaScript Code nodes and n8n workflow static data to simulate database tables and relationships without requiring an external database setup.

## 2. Objectives

- Understand relational database concepts and table relationships.
- Initialize and seed customer, order, and order item data.
- Insert order header and child order item records.
- Calculate order totals automatically.
- Validate customer credit limits before committing an order.
- Handle failed transactions through rollback logic.
- Retrieve consolidated order details using a simulated JOIN operation.
- Generate a final transaction audit.

## 3. Technologies Used

- n8n
- JavaScript
- n8n Code Node
- n8n IF Node
- n8n Workflow Static Data
- Postman (optional, for webhook-based testing if configured)

## 4. Workflow Architecture

The workflow contains the following nodes:

1. **Manual_Trigger** – Starts workflow execution manually.
2. **Code_InitRelationalDB** – Initializes simulated database tables and customer records.
3. **Code_StageIncomingOrder** – Creates the sample order payload with customer and line item details.
4. **Code_InsertOrderHeader** – Calculates the order total and inserts the parent order record.
5. **Code_InsertOrderItems** – Inserts child order item records linked to the parent order.
6. **Code_UpdateCustomerCredit** – Checks the customer's credit limit and commits or rolls back the transaction.
7. **IF_TransactionSucceeded** – Routes execution based on the transaction status.
8. **Log_RollbackAudit** – Records details when a transaction fails.
9. **Code_QueryFullOrderJoin** – Retrieves order, customer, and line item information using a simulated JOIN.
10. **Code_FinalTransactionAudit** – Generates the final audit result for a successful transaction.

## 5. Database Structure

The workflow simulates three relational tables.

### Customers

| Customer ID | Customer Name | Current Balance | Credit Limit |
|---|---|---:|---:|
| CUST-001 | Acme Corp | 1200 | 5000 |
| CUST-002 | Globex Inc | 450 | 500 |

### Orders

Stores the parent order information:

- Order ID
- Customer ID
- Order Date
- Total Amount
- Order Status

### Order Items

Stores individual products belonging to an order:

- Item ID
- Order ID
- SKU
- Description
- Quantity
- Unit Price
- Line Total

The `customerId` connects customers to orders, and `orderId` connects orders to their order items.

## 6. Transaction Processing

The workflow calculates the order total using:

`(Quantity × Unit Price)` for each line item.

For the sample order:

- Cloud Managed Server: 2 × 450 = 900
- Enterprise Security Seat: 5 × 75 = 375
- **Total Order Amount = 1275**

### Successful Transaction

For CUST-001:

- Current Balance: 1200
- Credit Limit: 5000
- Order Amount: 1275
- Projected Balance: 2475

Since the projected balance is within the credit limit, the transaction is committed and the order status is updated to `COMMITTED`.

### Failed Transaction and Rollback

For CUST-002:

- Current Balance: 450
- Credit Limit: 500
- Order Amount: 1275
- Projected Balance: 1725

Since the projected balance exceeds the credit limit, the workflow rolls back the transaction by removing the pending order header and its related order items.

The IF node routes the failed transaction to `Log_RollbackAudit`.

## 7. JOIN Operation

The `Code_QueryFullOrderJoin` node retrieves:

- Order details
- Customer information
- Related order line items

It combines the information using matching customer IDs and order IDs, simulating a relational SQL JOIN operation.

## 8. Final Transaction Audit

The `Code_FinalTransactionAudit` node generates the final audit output, including:

- Audit status
- Order ID
- Customer ID and customer name
- Transaction status
- Persisted order total
- Number of line items
- Updated customer balance
- Audit timestamp

Git Url:

https://github.com/SagarBommela/n8n-Hands-On-Level-2-Topic-11

Loom Video:

https://www.loom.com/share/00fa9b8a051c4f5facd5e37488d79f45





