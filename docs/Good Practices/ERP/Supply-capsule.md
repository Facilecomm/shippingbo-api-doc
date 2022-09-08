---
stoplight-id: yyk111bjoc3h8
tags: [ERP, Replenishment]
---

# Restock Management

Shippingbo allow user to plan restocking of your warehouse in a LMS context or OMS/WMS context.

A planned restocking is materialized by a `SupplyCapsule` in Shippingbo

## Create a SupplyCapsule in LMS context

> Your Shippingbo plan must be compatible with supply capsule, you can check on [Shippingbo.com](https://www.shippingbo.com/lms/)

You can create it with a simple request

<!-- theme: warning -->
> Warning: The duo **supplier_code** and **source_ref** is unique, you will receive a *409 Duplicate Entry* if you try to create with the same duo. In the **supply_capsule_items_attributes** field the **source_ref** must be unique too within a `SupplyCapsule`

```curl
curl --request POST \
  --url https://app.shippingbo.com/supply_capsules \
  --header 'Content-Type: application/json' \
  --data '{
  "source_ref": "my_source_ref",
  "expected_delivery_date": "",
  "supplier_name": "My Supplier",
  "supplier_code": "MY_SUPPLIER_001",
  "supply_capsule_items_attributes": [
    {
      "source_ref": "MY_PRODUCT_SOURCE_REF",
      "product_ref": "MY_SKU",
      "quantity": 5
    }
  ]
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/supply_capsules');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "source_ref": "my_source_ref",
  "expected_delivery_date": "",
  "supplier_name": "My Supplier",
  "supplier_code": "MY_SUPPLIER_001",
  "supply_capsule_items_attributes": [
    {
      "source_ref": "MY_PRODUCT_SOURCE_REF",
      "product_ref": "MY_SKU",
      "quantity": 5
    }
  ]
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://app.shippingbo.com/supply_capsules")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"source_ref\": \"my_source_ref\",\n  \"expected_delivery_date\": \"\",\n  \"supplier_name\": \"My Supplier\",\n  \"supplier_code\": \"MY_SUPPLIER_001\",\n  \"supply_capsule_items_attributes\": [\n    {\n      \"source_ref\": \"MY_PRODUCT_SOURCE_REF\",\n      \"product_ref\": \"MY_SKU\",\n      \"quantity\": 5\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

## Create a SupplyCapsule in OMS context

When you drive your WMS with an OMS Supervision you must create your SupplyCapsule in the OMS

To indicate which warehouse will receive the stock you must pass **supplier_id** with the ID of the warehouse

```curl
curl --request POST \
  --url https://app.shippingbo.com/supply_capsules \
  --header 'Content-Type: application/json' \
  --data '{
  "source_ref": "my_source_ref",
  "expected_delivery_date": "",
  "supplier_name": "My Supplier",
  "supplier_code": "MY_SUPPLIER_001",
  "supplier_id": 1,
  "supply_capsule_items_attributes": [
    {
      "source_ref": "MY_PRODUCT_SOURCE_REF",
      "product_ref": "MY_SKU",
      "quantity": 5
    }
  ]
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/supply_capsules');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "source_ref": "my_source_ref",
  "expected_delivery_date": "",
  "supplier_name": "My Supplier",
  "supplier_code": "MY_SUPPLIER_001",
  "supplier_id": 1,
  "supply_capsule_items_attributes": [
    {
      "source_ref": "MY_PRODUCT_SOURCE_REF",
      "product_ref": "MY_SKU",
      "quantity": 5
    }
  ]
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://app.shippingbo.com/supply_capsules")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"source_ref\": \"my_source_ref\",\n  \"expected_delivery_date\": \"\",\n  \"supplier_name\": \"My Supplier\",\n  \"supplier_code\": \"MY_SUPPLIER_001\",\n \"supplier_id\": 1\n  \"supply_capsule_items_attributes\": [\n    {\n      \"source_ref\": \"MY_PRODUCT_SOURCE_REF\",\n      \"product_ref\": \"MY_SKU\",\n      \"quantity\": 5\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

If your SupplyCapsule has been correctly send to the selected supplier the state of the record will pass from *waiting* to *dispatched*

<!-- theme: warning -->
> Note: the dispatching is asynchronous, so the return of the creation will be always waiting, if the dispatch fails the state will pass from *waiting* to *in_trouble*. We actively recommend to implement a [webhook](https://developer.shippingbo.com/docs/api/branches/main/yyk111bjoc3h8-replenishment-management#make-your-system-up-to-date) on it


## Make your system up-to-date

As a standard implementation you will link your Shippingbo account to your ERP, CRM, etc.. or your system that manage your restocking.

To do that, you have to create a Webhook on the ressource SupplyCapsule (c.f. [Webhook documentation](https://developer.shippingbo.com/docs/api/branches/main/90c5b4e8466fe-webhook))

Once Shippingbo has notified your server you can send notification, integrate it, alert or whatever

## Update your SupplyCapsule

Sometimes you will need to cancel a SupplyCapsule because your supplier canceled an order, so you have to cancel the SupplyCapsule in Shippingbo. You can do it with a simple request

```curl
curl --request PUT \
  --url https://app.shippingbo.com/supply_capsules/supply_capsule_id \
  --header 'Content-Type: application/json' \
  --data '{
  "state": "canceled"
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/supply_capsules/supply_capsule_id');
$request->setMethod(HTTP_METH_PUT);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "state": "canceled"
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```
```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://app.shippingbo.com/supply_capsules/supply_capsule_id")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Put.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"state\": \"canceled\"\n}"

response = http.request(request)
puts response.read_body
```

