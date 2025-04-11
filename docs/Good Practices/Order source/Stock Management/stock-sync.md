---
tags: [api, stock, product]
stoplight-id: d1cad0c28846a
---

# Virtual stock synchronization

We recommend to not interact directly with the resource to avoid throttling with a GET request on **product/{id}** or **/products** but we actively recommend the use of Webhooks

## Configure your webhook

We recommend following this [article](https://developer.shippingbo.com/docs/webhook/)

## The notification

Everytime a stock movement has been done on a product we will call your endpoint with the name of the resource, a seriliazed **Product** and an object with reason of the call: **additional_data**
You will find the new value of the stock in the field `stock`

```json
{
  object_class: "Product"
  object: {
    id: 922957134
    created_at: "2021-05-18T09:58:40+00:00"
    updated_at: "2022-06-30T14:53:00+00:00"
    source: "Manual product"
    source_ref: "sku-test-50"
    user_ref: "sku-test-50"
    stock: -6
    ean13: null
    title: null
    location: ""
    picture_url: null
    weight: null
    height: null
    length: null
    width: null
    hs_code: null
    supplier: "test1"
    other_ref1: null
    other_ref2: null
    other_ref3: null
    other_ref4: null
    other_ref5: null
    other_ref6: null
    other_ref7: null
    other_ref8: null
    other_ref9: null
    other_ref10: null
    other_ref11: null
    other_ref12: null
    other_ref13: null
    other_ref14: null
    other_ref15: null
    additional_references: []
    total_physical_stock: 0
    supplier_product_ids: []
    pack_components: []
  },
  additional_data: {
    field: "stock"
    from: 10
    to: 5
  },
  hook_info: {}
}
```

## Update the stock

Once the notification is received, you can update the stock on your website and leave it available or not on your catalog

## 🔥 Caution: can be spammy 🔥

The number of calls are equal to the number of stock movements on your Shippingbo account. Your server must be ready to receive calls as much as necessary

If your server does not respond, Shippingbo can turn off the webhook to avoid spamming, protect your server and keep it safe

We recommend to manage all requests asynchronously to avoid long response times and timeouts


