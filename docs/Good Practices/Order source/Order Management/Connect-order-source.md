---
tags: [order, source, api]
stoplight-id: c3fa417ce0854
---

# Create your first order

## Introduction

In this article we will see how to connect an order source, this integration can be usefull when the source is not natively connected to Shippingbo ([see availables sources](https://www.shippingbo.com/integrations/)) or if you need a special integration or specific behaviour


## Create orders on Shippingbo

<!--
type: tab
title: API
-->

### Create shipping and billing Address


You must create shipping and billing address before you create an Order, you should keep returned IDs to inject them into the order payload


```curl
curl --request POST \
  --url https://app.shippingbo.com/addresses \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --data '{
  "city": "Toulouse",
  "zip": "31400",
  "firstname": "John",
  "lastname": "Doe",
  "instructions": null,
  "phone1": "0102030405",
  "phone2": "0501020304",
  "email": "john.doe@exemple.com",
  "street1": "3 avenue de l'\''europe",
  "street2": null,
  "street3": null,
  "country": "FR",
  "company_name": "Shippingbo"
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/addresses');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-TOKEN' => '',
  'X-API-USER' => ''
]);

$request->setBody('{
  "city": "Toulouse",
  "zip": "31400",
  "firstname": "John",
  "lastname": "Doe",
  "instructions": null,
  "phone1": "0102030405",
  "phone2": "0501020304",
  "email": "john.doe@exemple.com",
  "street1": "3 avenue de l'europe",
  "street2": null,
  "street3": null,
  "country": "FR",
  "company_name": "Shippingbo"
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

url = URI("https://app.shippingbo.com/addresses")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request.body = "{\n  \"city\": \"Toulouse\",\n  \"zip\": \"31400\",\n  \"firstname\": \"John\",\n  \"lastname\": \"Doe\",\n  \"instructions\": null,\n  \"phone1\": \"0102030405\",\n  \"phone2\": \"0501020304\",\n  \"email\": \"john.doe@exemple.com\",\n  \"street1\": \"3 avenue de l'europe\",\n  \"street2\": null,\n  \"street3\": null,\n  \"country\": \"FR\",\n  \"company_name\": \"Shippingbo\"\n}"

response = http.request(request)
puts response.read_body
```

You can reproduce the same request to create the billing address with billing information, don't forget to save the id to inject it in the Order request

### Then, you can create an Order

#### Basic creation

> Each you create an order, keep in mind that **source** and **source_ref** has a unicity constraint, they will identify the order on your website

```curl
curl --request POST \
  --url https://app.shippingbo.com/orders \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --data '{
  "source": "My Website",
  "source_ref": "123",
  "shipping_address_id": 1,
  "billing_address_id": 2,
  "origin": "My Websit",
  "origin_ref": "123",
  "origin_created_at": "2019-08-24T14:15:22Z",
  "chosen_delivery_service": "Fedex",
  "earliest_delivery_at": "2019-08-24T14:15:22Z",
  "latest_delivery_at": "2019-08-24T14:15:22Z",
  "earliest_shipped_at": "2019-08-24T14:15:22Z",
  "latest_shipped_at": "2019-08-24T14:15:22Z",
  "earliest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "latest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "order_item_attributes": [
    {
      "product_ref": "my-product-sku-001",
      "title": "My fabulous product",
      "quantity": 1,
      "source": "My Website",
      "source_ref": "123-1"
    }
  ]
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-TOKEN' => '',
  'X-API-USER' => ''
]);

$request->setBody('{
  "source": "My Website",
  "source_ref": "123",
  "shipping_address_id": 1,
  "billing_address_id": 2,
  "origin": "My Websit",
  "origin_ref": "123",
  "origin_created_at": "2019-08-24T14:15:22Z",
  "chosen_delivery_service": "Fedex",
  "earliest_delivery_at": "2019-08-24T14:15:22Z",
  "latest_delivery_at": "2019-08-24T14:15:22Z",
  "earliest_shipped_at": "2019-08-24T14:15:22Z",
  "latest_shipped_at": "2019-08-24T14:15:22Z",
  "earliest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "latest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "order_item_attributes": [
    {
      "product_ref": "my-product-sku-001",
      "title": "My fabulous product",
      "quantity": 1,
      "source": "My Website",
      "source_ref": "123-1"
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

url = URI("https://app.shippingbo.com/orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request.body = "{\n  \"source\": \"My Website\",\n  \"source_ref\": \"123\",\n  \"shipping_address_id\": 1,\n  \"billing_address_id\": 2,\n  \"origin\": \"My Websit\",\n  \"origin_ref\": \"123\",\n  \"origin_created_at\": \"2019-08-24T14:15:22Z\",\n  \"chosen_delivery_service\": \"Fedex\",\n  \"earliest_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"earliest_shipped_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_shipped_at\": \"2019-08-24T14:15:22Z\",\n  \"earliest_chosen_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_chosen_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"order_item_attributes\": [\n    {\n      \"product_ref\": \"my-product-sku-001\",\n      \"title\": \"My fabulous product\",\n      \"quantity\": 1,\n      \"source\": \"My Website\",\n      \"source_ref\": \"123-1\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

If the order must be delivered to relay point you sould add **relay_ref** field to the body, the reference will be passed to carriers

#### Create with invoicing information

> If you have the invoice option enabled on your account, you should pass some prices information in the body

```curl
curl --request POST \
  --url https://app.shippingbo.com/orders \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --data '{
  "source": "My Website",
  "source_ref": "123",
  "shipping_address_id": 1,
  "billing_address_id": 2,
  "origin": "My Websit",
  "origin_ref": "123",
  "origin_created_at": "2019-08-24T14:15:22Z",
  "chosen_delivery_service": "Fedex",
  "earliest_delivery_at": "2019-08-24T14:15:22Z",
  "latest_delivery_at": "2019-08-24T14:15:22Z",
  "earliest_shipped_at": "2019-08-24T14:15:22Z",
  "latest_shipped_at": "2019-08-24T14:15:22Z",
  "earliest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "latest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "total_price_cents": 1500,
  "total_price_currency": "EUR",
  "total_tax_cents": 500,
  "total_shipping_tax_included_cents": 10,
  "total_shipping_tax_cents": 20,
  "order_item_attributes": [
    {
      "product_ref": "my-product-sku-001",
      "title": "My fabulous product",
      "quantity": 1,
      "source": "My Website",
      "source_ref": "123-1",
      "price_tax_included_cents": 1490,
      "price_tax_included_currency": "EUR",
      "tax_cents": 480,
      "tax_currency": "EUR"
    }
  ]
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-TOKEN' => '',
  'X-API-USER' => ''
]);

$request->setBody('{
  "source": "My Website",
  "source_ref": "123",
  "shipping_address_id": 1,
  "billing_address_id": 2,
  "origin": "My Websit",
  "origin_ref": "123",
  "origin_created_at": "2019-08-24T14:15:22Z",
  "chosen_delivery_service": "Fedex",
  "earliest_delivery_at": "2019-08-24T14:15:22Z",
  "latest_delivery_at": "2019-08-24T14:15:22Z",
  "earliest_shipped_at": "2019-08-24T14:15:22Z",
  "latest_shipped_at": "2019-08-24T14:15:22Z",
  "earliest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "latest_chosen_delivery_at": "2019-08-24T14:15:22Z",
  "total_price_cents": 1500,
  "total_price_currency": "EUR",
  "total_tax_cents": 500,
  "total_shipping_tax_included_cents": 10,
  "total_shipping_tax_cents": 20,
  "order_item_attributes": [
    {
      "product_ref": "my-product-sku-001",
      "title": "My fabulous product",
      "quantity": 1,
      "source": "My Website",
      "source_ref": "123-1",
      "price_tax_included_cents": 1490,
      "price_tax_included_currency": "EUR",
      "tax_cents": 480,
      "tax_currency": "EUR"
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

url = URI("https://app.shippingbo.com/orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request.body = "{\n  \"source\": \"My Website\",\n  \"source_ref\": \"123\",\n  \"shipping_address_id\": 1,\n  \"billing_address_id\": 2,\n  \"origin\": \"My Websit\",\n  \"origin_ref\": \"123\",\n  \"origin_created_at\": \"2019-08-24T14:15:22Z\",\n  \"chosen_delivery_service\": \"Fedex\",\n  \"earliest_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"earliest_shipped_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_shipped_at\": \"2019-08-24T14:15:22Z\",\n  \"earliest_chosen_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"latest_chosen_delivery_at\": \"2019-08-24T14:15:22Z\",\n  \"total_price_cents\": 1500,\n  \"total_price_currency\": \"EUR\",\n  \"total_tax_cents\": 500,\n  \"total_shipping_tax_included_cents\": 10,\n  \"total_shipping_tax_cents\": 20,\n  \"order_item_attributes\": [\n    {\n      \"product_ref\": \"my-product-sku-001\",\n      \"title\": \"My fabulous product\",\n      \"quantity\": 1,\n      \"source\": \"My Website\",\n      \"source_ref\": \"123-1\",\n      \"price_tax_included_cents\": 1490,\n      \"price_tax_included_currency\": \"EUR\",\n      \"tax_cents\": 480,\n      \"tax_currency\": \"EUR\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```
<br>

## Update Order lines

To update OrderItem of an Order you must use a special endpoint: `/orders/{orderId}/update_order_items`

This endpoint will update your current OrderItems or if you ommit the `id` in the payload the API will create a new OrderItem.


```curl
curl --request POST \
  --url https://app.shippingbo.com/orders/orderId/update_order_items \
  --header 'Content-Type: application/json' \
  --data '{
  "order_items": [
    {
      "id": 1,
      "product_ref": "my_sku",
      "title": "My Product",
      "quantity": 3,
      "product_source": "my source",
      "product_source_ref": "my product source ref",
      "product_ean": "my product ean13"
    }
  ]
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/orders/orderId/update_order_items');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "order_items": [
    {
      "id": 1,
      "product_ref": "my_sku",
      "title": "My Product",
      "quantity": 3,
      "product_source": "my source",
      "product_source_ref": "my product source ref",
      "product_ean": "my product ean13"
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

url = URI("https://app.shippingbo.com/orders/orderId/update_order_items")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"order_items\": [\n    {\n      \"id\": 1,\n      \"product_ref\": \"my_sku\",\n      \"title\": \"My Product\",\n      \"quantity\": 3,\n      \"product_source\": \"my source\",\n      \"product_source_ref\": \"my product source ref\",\n      \"product_ean\": \"my product ean13\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

## Create a Webhook to synchronize the Order state

Once the Order is created, you need to keep in touch your customer. We recommend to not interact directly with the ressource to avoid throttling with a GET request on order/{id} but we actively recommend to use Webhooks

Go to the [Webhooks](https://app.shippingbo.com/#/updateHooks) configuation page

Then click on **Add** button, select [**Order**](reference/Shipping-API-V1.json/definitions/Order) as object and select **state** in field

Fill in the endpoint with your API URL that should be called when the Order state is updated

So now, every time an Order has its state updated your endpoint will be called

See: [Webhooks Documentation](https://shippingbo.stoplight.io/docs/api-rest/branches/main/ZG9jOjg3MzY0MzM-order-webhook) for advanced usages

<!--
type: tab
title: SFTP
-->

## Basic fields

| Header  |Description|Mandatory|
|---|---|---|
|date|Order date|Yes|
|orderId|Order reference|Yes|
|origin|Origin of the order|No|
|originRef|Reference on the origin|No|
|companyName|company name of the shipping address|No|
|firstname|firstname|Yes|
|lastname|lastname|Yes|
|address1|address line 1|Yes|
|address2|address line 2|No|
|address3|address line 3|No|
|postcode|zip code|Yes|
|city|city|Yes|
|country|country code|Yes|
|phone|phone number|No|
|phone2|phone number|No|
|email|email|No|
|instructions|delivery instructions|No|
|chosenDeliveryService|choosen delivery method|No|
|relayRef|relay reference|No|
|tags|list of tags|No|

## Billing address fields

| Header  |Description|Mandatory|
|---|---|---|
|billingName|billing name|No|
|billingFirstname|firstname|No|
|billingLastname|lastname|No|
|billingAddress1|line address 1|No|
|billingAddress2|line address 2|No|
|billingAddress3|line address 3|No|
|billingPostalcode|zip code|No|
|billingCity|city|No|
|billingCountry|country code|No|

## Line fields

You can duplicate a CSV line to add a new line in the order by changing those values, all others values must be the same

| Header  |Description|Mandatory|
|---|---|---|
|lineId|Order line reference|Yes|
|sku|sku of the product|Yes|
|ean|ean of the product|No|
|quantity|quantity of the product|Yes|
|description|description of the line|No|
|lineCents|line price in cents|No|
|lineProductsCents|product line price in cents|No|
|lineShippingCents|shipping price of the line in cents|No|

## Price fields

Can be useful if you want to generate invoices with Shippingbo

| Header  |Description|Mandatory|
|---|---|---|
|totalCents|total price in cents|No|
|totalShippingCents|total shipping price in cents|No|
|currency|currency|No|

## Date fields

| Header  |Description|Mandatory|
|---|---|---|
|earliestShippedAt|earliest date to shipped the order|No|
|latestShippedAt|latest date to shipped the order|No|
|earliestDeliveryAt|earliest date to deliver the order|No|
|latestDeliveryAt|latest date to deliver the order|No|


## CSV examples

The next example will show you how to create an order with 2 products and without billing address

```csv
date,orderId,lineId,origin,originRef,sku,ean,quantity,description,companyName,name,firstname,lastname,address1,address2,address3,postcode,city,country,phone,phone2,email,instructions,chosenDeliveryService,relayRef,totalCents,totalShippingCents,lineCents,lineProductsCents,lineShippingCents,currency
2022-09-22 10:22,1234,1,Myorigin,Myorigin-132,sku-001,,1,product title,My Super Company,,John,Doe,Rue de la paix,,,31000,Toulouse,FR,0102030405,,test@shippingbo.com,leave it in the letter box,Home Delivery,1500,500,1500,1500,250,EUR
2022-09-22 10:22,1234,2,Myorigin,Myorigin-132,sku-002,,1,product title 2,My Super Company,,John,Doe,Rue de la paix,,,31000,Toulouse,FR,0102030405,,test@shippingbo.com,leave it in the letter box,Home Delivery,3000,500,1500,1500,250,EUR
```


<!-- type: tab-end -->

<!-- theme: success -->

> #### Mission Accomplished!
>
> Your first Order is created!
