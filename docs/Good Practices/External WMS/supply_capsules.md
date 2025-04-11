---
stoplight-id: rwxng0thle6r4
---

# Manage incoming goods

## List incoming goods

First, you need to check what incoming goods you have to wait. So you can call the [List SupplyCapsule endpoint](https://developer.shippingbo.com/docs/api/branches/main/6be80a82ef53d-list-supply-capsule)

```curl
curl --request GET \
  --url https://app.shippingbo.com/supply_capsules \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --header 'X-API-USER-ID: ' \
  --header 'X-API-VERSION: '
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/supply_capsules",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
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

url = URI("https://app.shippingbo.com/supply_capsules")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request["X-API-VERSION"] = ''
request["X-API-USER-ID"] = ''

response = http.request(request)
puts response.read_body
```

## Notify received products

When you received products in your warehouse you have to notify the merchant that you have received the goods
You have to implement the next request (see: [Increment received product](https://developer.shippingbo.com/docs/api/branches/main/b97428e10a40f-increment-received-product))

```curl
curl --request POST \
  --url https://app.shippingbo.com/supply_capsules/id/increment_received_product \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --header 'X-API-USER-ID: ' \
  --header 'X-API-VERSION: ' \
  --data '{
  "product_ref": "3701013031247",
  "received_quantity": 13
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/supply_capsules/id/increment_received_product",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "{\n  \"product_ref\": \"3701013031247\",\n  \"received_quantity\": 13\n}",
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

url = URI("https://app.shippingbo.com/supply_capsules/id/increment_received_product")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request["X-API-VERSION"] = ''
request["X-API-USER-ID"] = ''
request.body = "{\n  \"product_ref\": \"3701013031247\",\n  \"received_quantity\": 13\n}"

response = http.request(request)
puts response.read_body
```

## Update the incoming goods status

Once you recevied all products you have to update the status of the `SupplyCapsule`.
You can also cancel the `SupplyCapsule` if needed (all available status [here](https://developer.shippingbo.com/docs/api/2a40840df69c4-update-a-supply-capsule))

```curl
curl --request PUT \
  --url https://app.shippingbo.com/supply_capsules/supply_capsule_id \
  --header 'Content-Type: application/json' \
  --data '{
  "state": "received"
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
  "state": "received"
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
request.body = "{\n  \"state\": \"received\"\n}"

response = http.request(request)
puts response.read_body
```

> **Note**: The new status will be propagted to the OMS of your customer