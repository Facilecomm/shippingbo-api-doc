---
stoplight-id: pxxd8ubh04d84
tags: [erp]
---

# Connect an ERP

To connect an ERP we recommend to follow those steps:

- Follow the tutorial to [connect an order source](https://developer.shippingbo.com/docs/api/branches/main/wmyaww0hkv422-connect-an-order-source)
- [Join an invoice to an Order](https://developer.shippingbo.com/docs/api/branches/main/1c2c1a2a5fd05-join-an-invoice-to-an-order) (only if you want to inject invoices in Shippingbo)
- [Manage incoming goods](https://developer.shippingbo.com/docs/api/branches/main/yyk111bjoc3h8-manage-incoming-goods)

## Order sequence diagram

See [B2C workflow](https://developer.shippingbo.com/docs/api/wmyaww0hkv422-connect-an-order-source)

```mermaid
sequenceDiagram
    autonumber
    ERP->>Shippingbo: Product creation
    ERP->>Shippingbo: Product update
    par Decrement product stock
      ERP->>Shippingbo: Inject B2B Orders
    end
    ERP->>Shippingbo: Inject invoices
    par Send notification to ERP or customer
      Shippingbo->>ERP: Notify order state changed
    end
```

## Incoming goods sequence diagram

```mermaid
sequenceDiagram
    autonumber
    ERP->>Shippingbo: Create incoming goods (SupplyCapsule)
    opt Cancel or update incoming goods
      ERP->>Shippingbo: Update incoming goods
      ERP->>Shippingbo: Update status
    end
    par Receive incoming goods
      Shippingbo->>ERP: Notify stock changed
      Shippingbo->>ERP: Notify goods received with details
    end
```