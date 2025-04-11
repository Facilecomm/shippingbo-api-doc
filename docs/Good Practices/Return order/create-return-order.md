---
stoplight-id: s0c33xo4dpip3
---

# Create a return order


You can easily connect a return order system to Shippingbo. To anticipate an integration with a third party you can create a **ReturnOrder** without getting orders or sensitive data

## Create with your own system

You must implement the next request to create a **ReturnOrder** in Shippingbo

The parameter *return_order_expected_items_attributes* will create a **ReturnOrderExpectedItem**. That means all products present in *return_order_expected_items_attributes* will be expected at the reception of the parcel in your warehouse

By default, all the products from the original order are automatically created for the new ReturnOrder. But you can pass the parameter `skip_expected_items_creation: true` to avoid this automatic creation and just create the products you defined in the *return_order_expected_items_attributes* parameter

```curl
curl --request POST \
  --url https://app.shippingbo.com/return_orders \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
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
$request->setUrl('https://app.shippingbo.com/return_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
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

url = URI("https://app.shippingbo.com/return_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"order_id\": 123,\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```


## Create with order source information (for a Third party)

For a thid party integration, for example, if a company manage your returns directly from your CMS, they must inject a **ReturnOrder** in Shippingbo by adding *source* and *source_ref*

> All possible *sources* must be communicated to the third party

```curl
curl --request POST \
  --url https://app.shippingbo.com/return_orders \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
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
$request->setUrl('https://app.shippingbo.com/return_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
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

url = URI("https://app.shippingbo.com/return_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"source\": \"Amazon\",\n  \"source_ref\": \"amazon_order_id\",\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

Shippingbo will match the right order with the information, if not an error **404 Not found** will be returned

## Propagate the return to the supplier

First you may want to known which warehouses can receive a returned order, so you must call **Get suppliers available for returns**


```curl
curl --request GET \
  --url https://app.shippingbo.com/suppliers/available_for_return_order \
  --header 'Content-Type: application/json'
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/suppliers/available_for_return_order');
$request->setMethod(HTTP_METH_GET);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
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
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''

response = http.request(request)
puts response.read_body
```

Once you have selected the warehouse you must add it to the **Create ReturnOrder** request by adding the field *supplier_id*

```curl
curl --request POST \
  --url https://app.shippingbo.com/return_orders \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
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
$request->setUrl('https://app.shippingbo.com/return_orders');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json'
  'X-API-VERSION' => '1',
  'X-API-APP-ID' => '',
  'Authorization' => 'Bearer token'
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

url = URI("https://app.shippingbo.com/return_orders")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-VERSION"] = '1'
request["Authorization"] = 'Bearer token'
request["X-API-APP-ID"] = ''
request.body = "{\n  \"source\": \"Amazon\",\n  \"source_ref\": \"amazon_order_id\",\n  \"reason\": \"broken product\",\n  \"reason_ref\": \"REPAiR\",\n  \"return_order_type\": \"return_order_customer\",\"supplier_id\": 1,\n  \"return_order_expected_items_attributes\": [\n    {\n      \"quantity\": 2,\n      \"user_ref\": \"my_sku\"\n    }\n  ]\n}"

response = http.request(request)
puts response.read_body
```

Then the return order will be created in the OMS and in the WMS with its mapped products, the warehouse can now control and restock returned products. All that data will be propagated to the OMS

## Make your system up-to-date

If you need to know which items are correctly returned and restocked in the warehouse you can create a Webhook on **ReturnOrder**

- Go to the [Webhooks](https://app.shippingbo.com/#/updateHooks) configuation page
- Then click on the Add button, select **ReturnOrder** as the object and select state in *field*
- In *To value* type *returned*, which is state when the **ReturnOrder** is retocked
- Fill in the endpoint with your API URL that should be called when the **ReturnOrder** state is updated
- So now, every time a **ReturnOrder** is returned your endpoint will be called with all details about the return

Recommended article: [See advanced usages of Webhooks](https://developer.shippingbo.com/docs/api/branches/main/6fkoxdhf5024p-how-to-configure)