---
stoplight-id: f3hn9cxx2c4np
---

# Product management

## Make your systme up-to-date

You can create a Webhook in Shippingbo to notify your system that a new product has been created

So, we recommend to follow the [Webhook tutorial](https://developer.shippingbo.com/docs/api/6fkoxdhf5024p-how-to-configure)
You can chose to be notified for each product creation or for every update

To retrieve already created products we recommend to follow the next step, if there is no products in the Shippingbo account you can go to the next chapter

## Get all products from a Shippingbo account

> This step is recommended only if the Shippingbo account has already products created

You can list products with a simple request and integrate it in your system

See [GET Products endpoint](https://developer.shippingbo.com/docs/api/b644b1bfa845a-list-products) for more info.

```curl
curl --request GET \
  --url https://app.shippingbo.com/products \
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
  CURLOPT_URL => "https://app.shippingbo.com/products",
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

url = URI("https://app.shippingbo.com/products")

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

You can retrive products from a precise date by adding the next params to the URL: `?search[updated_at__gt][]=datetime&sort[updated_at]=asc`, and **datetime** is a date (ex: 2020-10-13T08:44:47+00:00)