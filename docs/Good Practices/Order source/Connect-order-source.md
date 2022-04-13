---
tags: [order, source, api]
---

# How to connect an order source ?

## Create orders on Shippingbo

### First, you need to create shipping and billing Address

<!--
type: tab
title: API
-->

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
<br>

## Create a Webhook to sync the Order state

Go to the [Webhooks](https://app.shippingbo.com/#/updateHooks) configuation page

Then click on **Add** button, select [**Order**](reference/Shipping-API-V1.json/definitions/Order) as object and select **state** in field

Fill in the endpoint with your API URL that should update order state on your side

So now, every time an Order has its state updated your endpoint will be called

See: Webhooks Documentation

<!--
type: tab
title: FTP
-->

Exemple CSV/XML/JSON


<!-- type: tab-end -->

<!-- theme: success -->

> #### Mission Accomplished!
>
> Your first Order is created!
