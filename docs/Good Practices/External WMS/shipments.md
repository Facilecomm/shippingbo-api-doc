---
stoplight-id: wq90larmmqgdf
---

# Manage shipments

## Find the carrier

> Note: You can save the response in you database or in a file, the response will not change

You can list all carriers by implementing the next request, then keep the ID from the response for the next request

```curl
curl --request GET \
  --url https://app.shippingbo.com/carriers \
  --header 'Accept: application/json' \
  --header 'X-API-TOKEN: 123' \
  --header 'X-API-USER: 123' \
  --header 'X-API-USER-ID: 123' \
  --header 'X-API-VERSION: 123'
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/carriers",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "X-API-TOKEN: 123",
    "X-API-USER: 123",
    "X-API-USER-ID: 123",
    "X-API-VERSION: 123"
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

url = URI("https://app.shippingbo.com/carriers")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["Accept"] = 'application/json'
request["X-API-USER"] = '123'
request["X-API-TOKEN"] = '123'
request["X-API-VERSION"] = '123'
request["X-API-USER-ID"] = '123'

response = http.request(request)
puts response.read_body
```

## Find the shipping method

> Note: You can save the response in you database or in a file, the response will not change

You can list all shipping methods for a given carriers

```curl
curl --request GET \
  --url 'https://app.shippingbo.com/shipping_methods?search%5Bcarrier_id__eq%5D=123' \
  --header 'Accept: application/json' \
  --header 'X-API-TOKEN: 123' \
  --header 'X-API-USER: 123' \
  --header 'X-API-USER-ID: 123' \
  --header 'X-API-VERSION: 123'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/shipping_methods?search%5Bcarrier_id__eq%5D=123",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "X-API-TOKEN: 123",
    "X-API-USER: 123",
    "X-API-USER-ID: 123",
    "X-API-VERSION: 123"
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

url = URI("https://app.shippingbo.com/shipping_methods?search%5Bcarrier_id__eq%5D=123")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["Accept"] = 'application/json'
request["X-API-USER"] = '123'
request["X-API-TOKEN"] = '123'
request["X-API-VERSION"] = '123'
request["X-API-USER-ID"] = '123'

response = http.request(request)
puts response.read_body
```
## Ship an Order

To ship an `Order` you must create a `Shipment`, after the creation the `Order` will pass in state `shipped` or `partially_shipped`

Don't forget to inject the ids from previous requests (*shipping_method_id* and *carrier_id*)

```curl
curl --request POST \
  --url https://app.shippingbo.com/shipments \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --header 'X-API-USER-ID: ' \
  --header 'X-API-VERSION: ' \
  --data '{
  "order_id": 29283,
  "tracking_url": "https://dpd.com/012012",
  "shipping_ref": "012012",
  "shipping_method_id": 1,
  "carrier_id": 2,
  "order_items": [
    {
      "order_item_id": 81328,
      "item_quantity": 2
    }
  ]
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/shipments",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "{\n  \"order_id\": 29283,\n  \"tracking_url\": \"dpd.com/012012\",\n  \"shipping_ref\": \"012012\",\n  \"shipping_method_id\": \"1\",\n  \"carrier_id\": \"2\",\n  \"order_items\": [\n    {\n      \"order_item_id\": \"81328\",\n      \"item_quantity\": 2\n    }\n  ]\n}",
  CURLOPT_HTTPHEADER => [
    "Content-Type: application/json",
    "X-API-TOKEN: ",
    "X-API-USER: ",
    "X-API-USER-ID: ",
    "X-API-VERSION: "
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

url = URI("https://app.shippingbo.com/shipments")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-USER"] = ''
request["X-API-TOKEN"] = ''
request["X-API-VERSION"] = ''
request["X-API-USER-ID"] = ''
request.body = "{\n  \"order_id\": 29283,\n  \"tracking_url\": \"dpd.com/012012\",\n  \"shipping_ref\": \"012012\",\n  \"shipping_method_id\": \"1\",\n  \"carrier_id\": \"2\",\n  \"order_items\": [\n    {\n      \"order_item_id\": \"81328\",\n      \"item_quantity\": 2\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

> You can ommit the `order_items` field if all items are in the same parcel

