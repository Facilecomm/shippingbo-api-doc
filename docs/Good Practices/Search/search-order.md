---
stoplight-id: jyho1d3sk2oen
---

# Order searching

To retrieve commands with common characteristics, you can pass search parameters during API calls.

This article is reserved for advanced Shippingbo API users.

## Available parameters

List of **operators**: eq (equal), gt (greater than), lt (less than)

| Field             | Operator(s) |
| ----------------- | ----------- |
| created_at        | gt, lt      |
| shipped_at        | gt, lt      |
| origin_created_at | gt, lt      |
| source            | eq          |
| source_ref        | eq          |
| origin_ref        | eq          |
| state             | eq          |

## Examples of a search request:

Note: It's just an example, you can search an `Order` given parameters according to your need

Payload of the parent Order:

```json
{
    "order": {
        "id": 980191557,
        "source": "Shippingbo",
        "source_ref": "0b94450b-24fb-4215-9974-b7386995ed33",
        "state": "to_be_prepared",
        "total_weight": 800,
        "order_tags": [{ "value": "TAG1" }]
        "order_items": [{ "title": "TITLE", "quantity": 5, ... }],
        "mapped_products": [{ ... }],
        "shipping_address": {...},
        "billing_address": {...},
    }
}
```

### Simple search request:

Here, the request to search the target order

```curl
curl --request POST \
  --url https://app.shippingbo.com/orders/ \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
    "search": {
      "state__eq": ["to_be_prepared"]
    }
  }'
}
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/orders",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'search' => [
        'state__eq' => ['to_be_prepared'],
        'total_weight__eq' => [800]
    ]
  ]),
  CURLOPT_HTTPHEADER => [
    "Content-Type: application/json",
    "X-API-VERSION: ",
    "X-API-APP-ID: ",
    "Authorization: Bearer token"
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

url = URI("https://app.shippingbo.com/orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request["Content-Type"] = 'application/json'
request.body = '{
  "search": {
    "state__eq": ["to_be_prepared"],
    "total_weight__eq": ["800"]
  }
}'.to_json

response = http.request(request)
puts response.read_body
```
