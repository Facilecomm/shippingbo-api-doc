---
stoplight-id: wmyaww0hkv422
---

# Connect an order source

To connect an order source we recommend to follow those steps:

- [Create your first order](https://developer.shippingbo.com/docs/api/branches/main/c3fa417ce0854-create-your-first-order)
- [Join an invoice to an Order](https://developer.shippingbo.com/docs/api/branches/main/1c2c1a2a5fd05-join-an-invoice-to-an-order) (only if you want to inject invoices in Shippingbo)
- [Product synchronisation](https://developer.shippingbo.com/docs/api/branches/main/09955951018bf-product-synchronisation)
- [Virtual stock synchronisation](https://developer.shippingbo.com/docs/api/branches/main/d1cad0c28846a-virtual-stock-synchronisation)

## Sequence diagram

```mermaid
sequenceDiagram
    actor Customer
    autonumber
    Customer->>Your server: Order completed
    Your server->>Shippingbo API: Create a shipping address
    Shippingbo API->>Your server: Return serialized shipping address
    Your server->>Shippingbo API: Create a billing address
    Shippingbo API->>Your server: Return serialized billing address
    par Decrement product stock
      Your server->>Shippingbo API: Create an Order
      Shippingbo API->>Your server: Return serialized order
    end
    Shippingbo API-->>Your server: Stock changed webhook
    Shippingbo API-->>Your server: Order created webhook
    opt Notification
      Shippingbo API->>Customer: Send email/SMS confirmation
      Shippingbo API->>Customer: Send order state notification
    end
    Shippingbo API-->>Your server: The order is shipped
    Note over Shippingbo API,Your server: Notification as a webhook
```
