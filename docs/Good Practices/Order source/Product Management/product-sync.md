---
tags: [product, api]
stoplight-id: 09955951018bf
---

# Product synchronization

## Introduction

If you have already created your products on your side, you can synchronize the product catalog with Shippingbo by with the following article

## Product creation

With the Shippingbo API you can easily create a Product with only one request.

You must follow the [Basic creation](./#basic-creation) if:

- you created a product on OMS
- your account doesn't have warehouse slots

### Basic creation

```curl
curl --request POST \
  --url https://app.shippingbo.com/products \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
  "user_ref": "my-user-ref",
  "stock": 0,
  "ean13": "1234567891234",
  "title": "My super product",
  "location": "A1",
  "picture_url": "https://picture.exemple.com/pic.png",
  "weight": 200,
  "height": 150,
  "length": 150,
  "width": 150,
  "hs_code": "32165478",
  "supplier": null
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/products');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
]);

$request->setBody('{
  "user_ref": "my-user-ref",
  "stock": 0,
  "ean13": "1234567891234",
  "title": "My super product",
  "location": "A1",
  "picture_url": "https://picture.exemple.com/pic.png",
  "weight": 200,
  "height": 150,
  "length": 150,
  "width": 150,
  "hs_code": "32165478",
  "supplier": null
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

url = URI("https://app.shippingbo.com/products")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"user_ref\": \"my-user-ref\",\n  \"stock\": 0,\n  \"ean13\": \"1234567891234\",\n  \"title\": \"My super product\",\n  \"location\": \"A1\",\n  \"picture_url\": \"https://picture.exemple.com/pic.png\",\n  \"weight\": 200,\n  \"height\": 150,\n  \"length\": 150,\n  \"width\": 150,\n  \"hs_code\": \"32165478\",\n  \"supplier\": null\n}"

response = http.request(request)
puts response.read_body
```

### With multi-EAN

You product may have several references in your warehouse and Shippingbo must be able to know wich product it is when you flash a product or when you fill in OrderItem `product_ean`

If you work with multi-ean you do not have to pass the `ean13` when you create a product, but you have to create a `ProductBarcode` to associate the ean13 to a previously created Product

> Note: The `ProductBarcode` can be propagated from the OMS to the WMS

```curl
curl --request POST \
  --url https://app.shippingbo.com/product_barcodes \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
  "product_id": 0,
  "key": "ean",
  "value": "1236547893214"
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/product_barcodes');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
]);

$request->setBody('{
  "product_id": 0,
  "key": "ean",
  "value": "1236547893214"
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

url = URI("https://app.shippingbo.com/product_barcodes")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"product_id\": 0,\n  \"key\": \"ean\",\n  \"value\": \"1236547893214\"\n}"

response = http.request(request)
puts response.read_body
```

### Advanced creation

<!-- theme: warning -->

> If your **WMS** is connected to an **OMS**, we recommend the enabling of the product propagation, then synchronizing your product catalog on the **OMS**

If your account is configured with warehouse slots management, you cannot pass the available stock and the location in the paylaod. The available stock will be computed after you have synchronized your warehouse (see Article Warehouse Synchronization)

```curl
curl --request POST \
  --url https://app.shippingbo.com/products \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
  "user_ref": "my-user-ref",
  "ean13": "1234567891234",
  "title": "My super product",
  "picture_url": "https://picture.exemple.com/pic.png",
  "weight": 200,
  "height": 150,
  "length": 150,
  "width": 150,
  "hs_code": "32165478",
  "supplier": null
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/products');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
]);

$request->setBody('{
  "user_ref": "my-user-ref",
  "ean13": "1234567891234",
  "title": "My super product",
  "picture_url": "https://picture.exemple.com/pic.png",
  "weight": 200,
  "height": 150,
  "length": 150,
  "width": 150,
  "hs_code": "32165478",
  "supplier": null
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

url = URI("https://app.shippingbo.com/products")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"user_ref\": \"my-user-ref\",\n  \"ean13\": \"1234567891234\",\n  \"title\": \"My super product\",\n  \"picture_url\": \"https://picture.exemple.com/pic.png\",\n  \"weight\": 200,\n  \"height\": 150,\n  \"length\": 150,\n  \"width\": 150,\n  \"hs_code\": \"32165478\",\n  \"supplier\": null\n}"

response = http.request(request)
puts response.read_body
```

## Update products

```curl
curl --request PATCH \
  --url https://app.shippingbo.com/products/productId \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
    "user_ref": "my-new-user-ref",
  }'
```
