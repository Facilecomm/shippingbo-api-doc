---
stoplight-id: s0c33xo4dpip3
---

# Create a return order


You can easily connect a return order system to Shippingbo. To anticipate an integration with a third party you can create **ReturnOrder** without getting orders and sensitive data

## Create with your own system

You must implement the next request to create **ReturnOrder** in Shippingbo

The parameter *return_order_expected_items_attributes* will create **ReturnOrderExpectedItem**. That means all products present in *return_order_expected_items_attributes* will be expected at the reception of the parcel in your warehouse

```curl
curl --request POST \
  --url https://app.shippingbo.com/returns_orders \
  --header 'Content-Type: application/json' \
  --data '{
  "order_id": 123,
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
    }
  ]
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/returns_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "order_id": 123,
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
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

url = URI("https://app.shippingbo.com/returns_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"order_id\": 123,\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```


## Create with order source information (for Third party)

For a thid party integration, for example if a company manage your returns directlyt from your CMS, they must inject **ReturnOrder** in Shippingbo by adding *source* and *source_ref*

> All possible *source* must be communicated to the third party

```curl
curl --request POST \
  --url https://app.shippingbo.com/returns_orders \
  --header 'Content-Type: application/json' \
  --data '{
  "source": "Amazon-123",
  "source_ref": "amazon_order_id",
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
    }
  ]
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/returns_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "source": "Amazon",
  "source_ref": "amazon_order_id",
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
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

url = URI("https://app.shippingbo.com/returns_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"source\": \"Amazon\",\n  \"source_ref\": \"amazon_order_id\",\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

Shippingbo will match the right order with information, if not an error **404 Not found** will be returned

## Propagate the return to the supplier

First you may want to known which warehouses can receive a returned order, so you must call **Get suppliers available for returns**


```curl
curl --request GET \
  --url https://app.shippingbo.com/suppliers/available_for_return_order \
  --header 'Content-Type: application/json'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/suppliers/available_for_return_order');
$request->setMethod(HTTP_METH_GET);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

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

url = URI("https://app.shippingbo.com/suppliers/available_for_return_order")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["Content-Type"] = 'application/json'

response = http.request(request)
puts response.read_body
```

Once you have selected the warehouse you must add it to the **Create ReturnOrder** request by adding the field *supplier_id*

```curl
curl --request POST \
  --url https://app.shippingbo.com/returns_orders \
  --header 'Content-Type: application/json' \
  --data '{
  "source": "Amazon-123",
  "source_ref": "amazon_order_id",
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "supplier_id": 1,
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
    }
  ]
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/returns_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
]);

$request->setBody('{
  "source": "Amazon",
  "source_ref": "amazon_order_id",
  "reason": "broken product",
  "reason_ref": "REPAiR",
  "return_order_type": "return_order_customer",
  "supplier_id": 1,
  "return_order_expected_items_attributes": [
    {
      "quantity": 2,
      "user_ref": "my_sku"
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

url = URI("https://app.shippingbo.com/returns_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request.body = "{\n  \"source\": \"Amazon\",\n  \"source_ref\": \"amazon_order_id\",\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\"supplier_id\": 1,\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

Then the return order will be created in the OMS and in the WMS with is mapped products, the warehouse can now control and restock returned products. All those data will be propagated to the OMS 