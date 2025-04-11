---
stoplight-id: 6fkoxdhf5024p
tags: [webhook]
---

# How to configure ?

### Why you must implement Webhooks with Shippingbo ?

Webhooks are actively recommended to be used instead of polling, there is no case that justifies a massive polling. Shippingbo is able to call your server when you need the information like Order state changes, product stock changes, etc...

### Which resources are webhookable ?

<!-- theme: warning -->

> Please note the payload serialization will be fixed and cannot be personnalised

- [AddressLabelSummary](https://developer.shippingbo.com/docs/api/b240022c42eb7-address-label-summary)
- LabelproviderClientReference
- [Order](https://developer.shippingbo.com/docs/api/c1901df162b04-order)
- [OrderEvent](https://developer.shippingbo.com/docs/api/8ca608ed50360-order-event)
- [PreOrderReservation](https://developer.shippingbo.com/docs/api/szjid0ge3fj2x-pre-order-reservation)
- [Product](https://developer.shippingbo.com/docs/api/7b347df35970a-product)
- [ProductBarcode](https://developer.shippingbo.com/docs/api/x8k6b0o8ms260-product-barcode)
- [ProductStockVariation](https://developer.shippingbo.com/docs/api/5aoog4z944zus-product-stock-variation)
- Report
- [ReturnOrder](https://developer.shippingbo.com/docs/api/50kfy6766j6tx-return-order)
- [Shipment](https://developer.shippingbo.com/docs/api/931e7a30c2aa3-shipment)
- [SlotContent](https://developer.shippingbo.com/docs/api/1943284cb3dda-slot-content)
- [SlotStockVariation](https://developer.shippingbo.com/docs/api/408385a655a52-slot-stock-variation)
- [SupplyCapsule](https://developer.shippingbo.com/docs/api/rydjpwbyvxbz3-supply-capsule)
- [TrackingInfo](https://developer.shippingbo.com/docs/api/k50z3ltcl0m3z-tracking-info)
- [WarehousesTransfer](https://developer.shippingbo.com/docs/api/k7bcueja8n2i7-warehouses-transfer)

### How to configure a Webhook ?
### Basics

1. First you need to create the Webhook, go to the [Webhook configuration page](https://app.shippingbo.com/#/updateHooks), then click on the **Add** button

2. Select the **Object class**, this is the resource you want to observe. For example you can select **Order** if you want to be notified every time an **Order** is created or updated
3. Fill in the **URL** field and that's it !

Your webhook is already functionnal, the URL will be called every time the selected resource is created or updated

### Trigger webhook on destroy

On destroyable objects, you can decide to be notified or not on object deletion by checking the dedicated checkbox. In this case and when a destroy event occur, an attribute `deleted: true` will be present in the `additional_data` field in the body.

![hookdes.png](../../assets/images/hookdes.png)


### Trigger webhook only on creation

To be notified every time the resource is created and ignore updates you must select **id**

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/np9Q5SbHyYI)


### Trigger webhook only on update

To be notified every time the resource is updated you must select the field you want, for example if you want to be notified every time an **Order** is shipped you can configure it like this:

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/ELyedqU5BIw)

#### Trigger hook on specific values

You can select a specific **field** to trigger the webhook only when the **field** changes. If you configure **From value** the hook will be triggered only if the old value of the **field** is the same, and if you configure **To value** the hook will be triggered only if the new value of the **field** is the same.


### Security parameters

##### By authentication (most secure option)

How to setup webhook authentication ?

1. Go to the Webhook configuration page for the webhook you want to configure 

2. You must select **by authentication** in the authentication scheme

3. Enter the authentication URL to call before calling the endpoint

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/Zfugp1mYdkU)


```curl
curl --request POST \
  --url https://exemple.shippingbo.com/authentication \
  --header 'Content-Type: application/json' \
  --header 'authorization: "Basic my_access_token"'
```

4. The response must be like:

```json
{
  "token_type": "Basic",
  "access_token": "my_freshly_renewed_token"
}
```

5. Then in the webhook call you will find 2 headers:

    - "Authorization: Basic my_freshly_renewed_token" 
    - "x-api-key: my_api_key"

    You must check both of them to make sure Shippingbo is rightly authenticated and you can integrate the information safely


#### With free header

The "Free header" method is the easiest to implement.

How to setup free headers ?

1. You must select Free header
2. Fill in the field with your custom headers in JSON format

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/XSmNuNqhsj4)

3. And we will integrate the custom header to each request


### Example:

Here is an example of a body for an `Order` creation:


```json body
{
  "object_class": "Order", // Describe which type it is
  "object": {
    {
      "id":980191553,
      "created_at":"2023-08-31T08:53:17+00:00",
      "updated_at":"2023-08-31T08:53:17+00:00",
      "source":"Shippingbo",
      "source_ref":"0b94450b-24fb-4215-9974-b7386995ed33",
      "state":"to_be_prepared",
      "origin":"Shippingbo",
      "origin_ref":"abc",
      "shipping_address_id":1014264783,
      "custom_state":"unknown",
      "origin_created_at":"2023-08-31T08:53:17+00:00",
      "shipped_at":null,
      "closed_at":null,
      "state_changed_at":"2023-08-29T15:39:14+00:00",
      "earliest_shipped_at":"2021-04-26T16:22:46+00:00",
      "latest_shipped_at":null,
      "earliest_delivery_at":null,
      "latest_delivery_at":"2020-10-23T08:08:00+00:00",
      "latest_chosen_delivery_at":null,
      "earliest_chosen_delivery_at":null,
      "chosen_delivery_service":null,
      "fulfilled_by_marketplace":false,
      "relay_ref":null,
      "total_price_cents":5000,
      "total_without_tax_cents":null,
      "total_tax_cents":null,
      "total_price_currency":null,
      "total_shipping_tax_included_cents":0,
      "total_shipping_cents":0,
      "total_shipping_tax_cents":0,
      "total_shipping_tax_included_currency":"EUR",
      "total_discount_tax_included_cents":0,
      "total_discount_tax_included_currency":"EUR",
      "total_weight":800,
      "order_items":[
          {
            "id":1004159586,
            "created_at":"2023-08-31T08:53:17+00:00",
            "updated_at":"2023-08-31T08:53:17+00:00",
            "source":"980191551-0",
            "source_ref":null,
            "product_ref":"from-sku-002",
            "product_ean":null,
            "title":"product2",
            "quantity":4,
            "price_tax_included_cents":5000,
            "price_cents":null,
            "tax_cents":null,
            "price_tax_included_currency":"EUR",
            "shipping_price_tax_included_cents":null,
            "shipping_price_cents":null,
            "shipping_tax_cents":null,
            "shipping_price_tax_included_currency":null,
            "supplier_order_items":[
                
            ],
            "matching_product":null
          }
      ],
      "order_tags":[
          {
            "id":325184381,
            "created_at":"2023-08-31T08:53:17+00:00",
            "updated_at":"2023-08-31T08:53:17+00:00",
            "value":"GROUPED_ORDER"
          },
          {
            "id":325184382,
            "created_at":"2023-08-31T08:53:17+00:00",
            "updated_at":"2023-08-31T08:53:17+00:00",
            "value":"BAD_PHONE"
          },
      ],
      "mapped_products":[
          {
            "id":964,
            "created_at":"2023-08-31T08:53:17+00:00",
            "updated_at":"2023-08-31T08:53:17+00:00",
            "order_item_id":1004159586,
            "product_id":922957152,
            "quantity":4,
            "product_user_ref":"from-sku-002"
          }
      ],
      "shipments":[],
      "order_documents":[],
      "shipping_address":{
          "id":1014264783,
          "created_at":"2023-08-31T08:53:17+00:00",
          "updated_at":"2023-08-31T08:53:17+00:00",
          "civility":"M",
          "fullname":null,
          "firstname":"Raphaël",
          "lastname":"ShippingBo",
          "street1":"1 All. Gabriel Biénès",
          "street2":null,
          "street3":null,
          "street4":null,
          "city":"Toulouse",
          "zip":"31000",
          "state":null,
          "country":"FR",
          "email":"test@shippingbo.com",
          "phone1":"0102030405",
          "phone2":null,
          "instructions":null,
          "building":null,
          "company_name":null,
          "apartment_number":null,
          "place_name":null,
          "vat_importer":null
      },
      "billing_address":{
          "id":1014264784,
          "created_at":"2023-08-31T08:53:17+00:00",
          "updated_at":"2023-08-31T08:53:17+00:00",
          "civility":null,
          "fullname":null,
          "firstname":"Jérôme",
          "lastname":"Dupont",
          "street1":"6 rue de la paix",
          "street2":null,
          "street3":null,
          "street4":null,
          "city":"Strasbourg",
          "zip":"67000",
          "state":null,
          "country":"FR",
          "email":"test@shippingbo.com",
          "phone1":"0102030405",
          "phone2":null,
          "instructions":null,
          "building":null,
          "company_name":null,
          "apartment_number":null,
          "place_name":null,
          "vat_importer":null
      },
      "initial_order_id":null,
      "invoice_ref":null,
      "external_computed_carrier_service":null
    }
  }
```

### Debug a webhook

In the webhook page you will find a button **Logs**, this button will redirect to the Shippingbo debug application. Using this app, you can see every call Shippingbo has made to your endpoint, you can check headers, status code, body and more.

This application is very useful to debug your implementation and we encourage each developer to use this application before asking the Support team or your Shippingbo contact. If the error seems to come from Shippingbo please contact us with the link to your webhook logs, without it Shippingbo will redirect you to the Logs page