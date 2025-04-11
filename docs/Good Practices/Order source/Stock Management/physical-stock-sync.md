---
stoplight-id: vdy1hucwr31fs
internal: true
---

# Physical stock synchronisation

## On a WMS

<!-- theme: warning -->

> Before implement requests make sure you have the **warehouse slots option** on your Shippingbo account.
> If not you will not be able to access endpoints

To obtain physical stock for a product you can use the `/warehouse_slots` endpoint:

```curl
curl --request GET \
  --url https://app.shippingbo.com/warehouse_slots?search[product_id__eq]=123 \
  --header 'Accept: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/warehouse_slots?search[product_id__eq]=123",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "X-API-VERSION: ",
    "Authorization: Bearer token",
    "X-API-APP-ID: "
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

url = URI("https://app.shippingbo.com/warehouse_slots?search[product_id__eq]=123")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request["Accept"] = 'application/json'

response = http.request(request)
puts response.read_body
```

The endpoint is very usefull to interact with slots of your warehouse

```json title="Response of the previous request"
{
  "id": 1,
  "created_at": "2019-08-24T14:15:22Z",
  "updated_at": "2019-08-24T14:15:22Z",
  "name": "string",
  "priority": 0,
  "picking_disabled": true,
  "warehouse_zone_name": "string",
  "stock_disabled": true,
  "slot_contents": [
    {
      "id": 0,
      "created_at": "2019-08-24T14:15:22Z",
      "updated_at": "2019-08-24T14:15:22Z",
      "product_id": 123,
      "product_user_ref": "SKU1",
      "product_ref": "SKU1",
      "product_title": "My Product",
      "warehouse_slot_id": 1,
      "warehouse_slot_name": "A",
      "stock": 20,
      "batch_number": null,
      "last_preparation_day": null,
      "stored_serial_numbers": []
    }
  ]
}
```

The object `SlotContent` respresents a product in a slot, here the product in the slot are filtered by `search[product_id__eq]=123`, if you have only the reference of the product you can add this filter `search[user_ref__eq]=SKU1` and you will obtain the same result

## On an OMS

### Retrieve physical stock

To obtain information about a product you can use the next request:

```curl
curl --request GET \
  --url https://app.shippingbo.com/products/productId \
  --header 'Accept: application/json, Example' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/products/productId",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "X-API-VERSION: ",
    "Authorization: Bearer token",
    "X-API-APP-ID: "
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

url = URI("https://app.shippingbo.com/products/productId")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request["Accept"] = 'application/json'

response = http.request(request)
puts response.read_body
```

Once you have the response, you will retrieve the total available stock of your suppliers in the column _stock_ and the total physical stock fo you suppliers in the column _physical_stock_

```json title="Response of the previous request"
{
  "id": 1,
  "stock": 50,
  "total_physical_stock": 65,
  "supplier_product_ids": [],
}
```

### Get more details about physical stock

You can find _available_stock_ and _physical_stock_ by supplier with the next request

```curl
curl --request GET \
  --url https://app.shippingbo.com/supplier_products?search[product_id__eq]=123 \
  --header 'Accept: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/supplier_products?search[product_id__eq]=123",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "X-API-VERSION: ",
    "Authorization: Bearer token",
    "X-API-APP-ID: "
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

url = URI("https://app.shippingbo.com/supplier_products?search[product_id__eq]=123")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request["Accept"] = 'application/json'

response = http.request(request)
puts response.read_body
```

```json title="Response of the previous request"
[
  {
    "id": 456,
    "created_at": "2019-08-24T14:15:22Z",
    "updated_at": "2019-08-24T14:15:22Z",
    "supplier_id": 1,
    "product_id": 123,
    "supplier_ref": "my_ref1",
    "available_stock": 25,
    "physical_stock": 30
  },
    {
    "id": 789,
    "created_at": "2019-08-24T14:15:22Z",
    "updated_at": "2019-08-24T14:15:22Z",
    "supplier_id": 2,
    "product_id": 123,
    "supplier_ref": "my_ref1",
    "available_stock": 25,
    "physical_stock": 35
  }
]
```

Note: The query parameter `search[product_id__eq]` is mandatory
