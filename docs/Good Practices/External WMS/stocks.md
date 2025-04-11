---
stoplight-id: zfumk4swv2z9w
---

# Stock management

## Update stock

<!--
type: tab
title: API
-->

You can update stock by using the `stock` field that will replace the current stock or by using `physical_stock` that will trigger the stock recomputation according to the current stock reservations

### With EAN or SKU

See [Update product with EAN or SKU Endpoint](https://developer.shippingbo.com/docs/api/branches/main/95be7535c3a5e-update-a-product-with-sku-or-ean)

In the endpoint you must replace __\_\_KEY\_\___ by "ean13" or "user_ref" and __\_\_VALUE\_\___ by the value of the "ean13" or "user_ref"

```curl
curl --request PATCH \
  --url https://app.shippingbo.com/products/__KEY__/__VALUE__ \
  --header 'Content-Type: application/json' \
  --data '{
  "stock": 10
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/products/__KEY__/__VALUE__",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "PATCH",
  CURLOPT_POSTFIELDS => "{\n  \"stock\": 10\n}",
  CURLOPT_HTTPHEADER => [
    "Content-Type: application/json"
  ],
]);

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://app.shippingbo.com/products/__KEY__/__VALUE__")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Patch.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"stock\": 10\n}"

response = http.request(request)
puts response.read_body
```

### With the ID

See [Update product](https://developer.shippingbo.com/docs/api/branches/main/28d3b50410a7b-update-a-product)

```curl
curl --request PATCH \
  --url https://app.shippingbo.com/products/__ID__ \
  --header 'Content-Type: application/json' \
  --data '{
  "stock": 10
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/products/__ID__",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "PATCH",
  CURLOPT_POSTFIELDS => "{\n  \"stock\": 10\n}",
  CURLOPT_HTTPHEADER => [
    "Content-Type: application/json"
  ],
]);

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://app.shippingbo.com/products/__ID__")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Patch.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"stock\": 10\n}"

response = http.request(request)
puts response.read_body
```

## Stock variation

To update the product stock you can just add or remove stock by [creating a Stock Variation](https://developer.shippingbo.com/docs/api/branches/main/56b34b6674ab1-create-a-stock-variation)
You can also use `ean13` as `product_key` or directly use the field `product_id`

```curl
curl --request POST \
  --url https://app.shippingbo.com/stock_variations \
  --header 'Content-Type: application/json' \
  --data '{
  "product_key": "user_ref",
  "user_ref": "my_sku_001",
  "variation": 5
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/stock_variations",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "{\n  \"product_key\": \"user_ref\",\n  \"user_ref\": \"my_sku_001\",\n  \"variation\": 5\n}",
  CURLOPT_HTTPHEADER => [
    "Content-Type: application/json"
  ],
]);

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```
```ruby
require 'uri'
require 'net/http'
require 'openssl'

url = URI("https://oms-staging.shippingbo.com/stock_variations")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"product_key\": \"user_ref\",\n  \"user_ref\": \"my_sku_001\",\n  \"variation\": 5\n}"

response = http.request(request)
puts response.read_body
```

<!--
type: tab
title: SFTP
-->

If you have an SFTP connector with Shippingbo you can update stocks

First, you must have a `/stock` directory in the `~/` directory

Shippingbo only allows `.csv` files

The column `sku` correspond to the `user_ref` of the Product

Examples of files you can use:

```physical_stock
"sku","physicalStock" 
"007-AZE123","42"
```
```stock
"sku","stock" 
"007-AZE123","42"
```

<!-- type: tab-end -->