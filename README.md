# E-Commerce-Order-Fulfilment
E-commerce Order Fulfillment System (n8n)
This n8n automation connects an e-commerce platform (like Shopify or Stripe) to fulfillment, inventory, and notification tools. It automates the entire post-purchase process: from confirming the order to sending real-time shipping updates.

This is an intermediate-to-advanced project that integrates sales, payments, logistics, and communication flows into one seamless system.

🚀 Core Features
Instant Order Capture: Uses a Webhook to instantly catch new orders from any storefront (Stripe, Shopify, etc.).

Automatic Payment Verification: An IF node checks the payment_status before triggering fulfillment.

Real-Time Inventory Control: Connects to Airtable, finds the product by SKU, and automatically decrements the Inventory Quantity.

Dynamic Invoice Generation: Automatically finds a Google Doc template, copies it, and fills it with dynamic customer data (order ID, amount paid, etc.).

PDF Conversion: Converts the filled-in Google Doc into a PDF using the Google Drive API.

Dual Notifications:

Internal Alert: Sends a custom email to the fulfillment team.

Customer Confirmation: Sends a professional confirmation email to the customer with their unique PDF invoice attached.

🧩 Tools & Integrations
Core: n8n

Payments: Stripe (via Webhook)

Database/Inventory: Airtable

Invoicing: Google Docs & Google Drive

Notifications: Gmail (or any email node)

⚙️ How It Works (Workflow Diagram)
This workflow is built on a "True/False" logic path. The main fulfillment and invoicing operations only run if the payment is verified.

!(httpsZGF0YTogW2NpdGU6IFNjcmVlbnNob3QgMjAyNS0xMS0wOCBhdCA5LjUzLjAzIFBNLmpwZ10=)

# How to Set Up This Workflow
You will need to configure 4 main components for this workflow to run.

1. Airtable (Inventory Base)
You need an Airtable base to act as your product database.

Create a Base: Name it E-commerce Fulfillment.

Create a Table: Name it Products or Product list.

Add Columns:

SKU (Single Line Text) - This is the unique ID we search for.

Product Name (Single Line Text)

Inventory Quantity (Number)

2. Google Docs (Invoice Template)
Create a Google Doc to act as your invoice template. Use placeholders (mustache syntax) for the data n8n will fill in.

Template Example:

Invoice for Order: {{ order_id }}

Customer: {{ customer_email }} Product: {{ product_sku }} Amount Paid: ${{ amount_paid }}

3. n8n Credentials
You will need to create and authenticate three credentials in your n8n instance:

Airtable: Use a Personal Access Token.

Google Docs: Use OAuth2.

Google Drive: Use OAuth2 (can be the same credential as Google Docs).

Gmail: Use OAuth2 (can be the same credential).

4. Testing the Webhook
The workflow starts with a Webhook node. To get your sample data:

Click "Listen for test event" on the Wait for New Order node.

Use a tool like Postman to send a POST request to the test URL.

Use the sample JSON below in the Body (set to raw -> JSON) to simulate a successful order:

JSON

{
  "id": "evt_1H7XfC2eZvKYlo2CKx3wQ1bA",
  "type": "charge.succeeded",
  "data": {
    "object": {
      "id": "ch_1H7XfC2eZvKYlo2CN2r34JkL",
      "amount": 4999,
      "customer_email": "customer@example.com",
      "payment_status": "paid",
      "lines": {
        "data": [
          {
            "metadata": {
              "sku": "EB1001"
            }
          }
        ]
      }
    }
  }
}
💡 Possible Advanced Add-ons
AI Integration: Use OpenAI to analyze order trends or predict top-selling products.

Analytics: Connect to Google Sheets or Data Studio for real-time sales dashboards.

Refunds: Add a "False" branch to the IF node to automate refund processing for failed orders.

CRM Sync: Add a final step to create/update a customer record in HubSpot or Pipedrive.
