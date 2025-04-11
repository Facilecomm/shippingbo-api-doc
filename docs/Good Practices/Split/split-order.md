---
stoplight-id: rliq6380w8g4w
---

# Order splitting

This article is reserved for advanced Shippingbo API users

## Example of a split request:

In this example we will split an order for each quantity of product, there is 4 quantity, after the request we will have 4 new `Order` created

Note: It's just an example, you can split an `Order` according to your need

Payload of the parent Order:

```json
{
    "order": {
        "id": 980191557,
        "source": "Shippingbo",
        "source_ref": "0b94450b-24fb-4215-9974-b7386995ed33",
        "state": "to_be_prepared",
        "total_weight": 800,
        "order_items": [
            {
                "id": 1004159590,
                "product_ref": "from-sku-002",
                "title": "product2",
                "quantity": 4,
            }
        ],
        "mapped_products": [
            {
                "id": 968,
                "created_at": "2023-09-06T09:29:16+00:00",
                "updated_at": "2023-09-06T09:29:16+00:00",
                "order_item_id": 1004159590,
                "product_id": 922957152,
                "quantity": 4,
                "product_user_ref": "from-sku-002"
            }
        ],
        "shipping_address": {...},
        "billing_address": {...},
    }
}
```

Here, the request to separate all quantities of product in different `Order`

```curl
curl --request POST \
  --url https://app.shippingbo.com/orders/orderId/split_order \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
  --data '{
  "suborders": {
    "0": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "1": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "2": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "3": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    }
  }
}'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/orders/orderId/split_order",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'suborders' => [
        '0' => [
                '0' => [
                                'item_id' => 1004159590,
                                'sku' => 'from-sku-002',
                                'title' => 'product2',
                                'quantity' => 1,
                                'product_id' => 922957152
                ]
        ],
        '1' => [
                '0' => [
                                'item_id' => 1004159590,
                                'sku' => 'from-sku-002',
                                'title' => 'product2',
                                'quantity' => 1,
                                'product_id' => 922957152
                ]
        ],
        '2' => [
                '0' => [
                                'item_id' => 1004159590,
                                'sku' => 'from-sku-002',
                                'title' => 'product2',
                                'quantity' => 1,
                                'product_id' => 922957152
                ]
        ],
        '3' => [
                '0' => [
                                'item_id' => 1004159590,
                                'sku' => 'from-sku-002',
                                'title' => 'product2',
                                'quantity' => 1,
                                'product_id' => 922957152
                ]
        ]
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

url = URI("https://app.shippingbo.com/orders/orderId/split_order")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request["Content-Type"] = 'application/json'
request.body = '{
  "suborders": {
    "0": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "1": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "2": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
    "3": {
      "0": {
        "item_id": 1004159590,
        "sku": "from-sku-002",
        "title": "product2",
        "quantity": 1,
        "product_id": 922957152
      }
    },
  }
}'.to_json

response = http.request(request)
puts response.read_body
```
