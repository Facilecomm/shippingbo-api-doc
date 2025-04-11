---
stoplight-id: r7ilvzdlqtrtp
---

# Manage shipments

## Make your system up-to-date

### With webhooks (recommended)

To warn your customers of their orders state changes, we recommend the creation of a webhook on Orders, to do that follow those steps:


- Go to the [Webhooks](https://app.shippingbo.com/#/updateHooks) configuation page
- Then click on the **Add** button, select [**Order**](https://developer.shippingbo.com/docs/api/branches/main/f78cd5e08619e-order) as the object and select **state** in the field

- Fill in the endpoint with your API URL that should be called when the Order state is updated
- So now, every time an Order has its state updated your endpoint will be called
- See: [Webhooks Documentation](https://developer.shippingbo.com/docs/api/branches/main/6fkoxdhf5024p-how-to-configure) for advanced usages

So now, each time the state of the order has changed you can update the status for your customer

### By requesting Orders

If you cannot implement webhooks you can use the [Get Orders endpoint](https://developer.shippingbo.com/docs/api/branches/main/e8225c7cc9bcc-get-orders) and filter on the `shipped_at` field

```curl
curl --request GET \
  --url https://app.shippingbo.com/orders?limit=50&offset=0&search[shipped_at__gt][]=2022-09-20+00:00&search[shipped_at__lteq][]=2022-09-21+00:00&sort[id]=desc \
  --header 'Content-Type: application/json' \
  --header 'X-API-VERSION: 1' \
  --header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
  --header 'X-API-APP-ID: 2'
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://app.shippingbo.com/orders?limit=50&offset=0&search[shipped_at__gt][]=2022-09-20+00:00&search[shipped_at__lteq][]=2022-09-21+00:00&sort[id]=desc",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "GET",
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
require 'openssl'

url = URI("https://app.shippingbo.com/orders?limit=50&offset=0&search[shipped_at__gt][]=2022-09-20+00:00&search[shipped_at__lteq][]=2022-09-21+00:00&sort[id]=desc")

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

## Shipping number and tracking URL

Once the Order is shipped, it will be represented with a [Shipment](https://developer.shippingbo.com/docs/api/branches/main/ea131f369e35f-shipment). A Shipment is created for each parcel you ship and store all information about the parcel

The shipping number is stored in the field `shipping_ref` and the tracking URL in `tracking_url`

> The object Shipment is serialized in Order webhook or [Get an Order](https://developer.shippingbo.com/docs/api/branches/main/994d201f214c0-get-an-order) endpoint, you don't have to request it directly


## Serial number, batch number and DLUO

Sometimes you will need more information about the Shipment like: DLUO, serial numbers or batch number

You can find all that information in the serialized [**Order**](https://developer.shippingbo.com/docs/api/branches/main/f78cd5e08619e-order), then in the field `shipments`

For example for one [Shipment](https://developer.shippingbo.com/docs/api/branches/main/ea131f369e35f-shipment) you can find serial numbers in `items_shipment_serial_numbers`, DLUO in the field `last_preparation_day` and batch numbers in `batch_number`

```json
{
  "id": 900886325,
  "created_at": "2022-03-30T13:22:52+00:00",
  "updated_at": "2022-03-30T13:22:52+00:00",
  "order_id": 980191358,
  "carrier_name": "SboLabelByApi",
  "shipping_method_name": "Classic API",
  "carrier_id": 1060660107,
  "shipping_method_id": 1060660120,
  "shipping_ref": "12346",
  "tracking_url": "http://tracking.fake/123456",
  "package_id": 3425,
  "label_price_cents": null,
  "label_price_currency": null,
  "items_shipment_serial_numbers": [
    {
      "order_item_id": 0,
      "serial_number": "string"
    }
  ],
  "carrier_barcode": null,
  "order_items_shipments": [
    {
      "order_item_id": 1004159358,
      "quantity": 1,
      "order_item_shipment_picking_histories": [
        {
          "id": 123456,
          "batch_number": "LM2105",
          "last_preparation_day": "2022-09-30"
        }
      ]
    },
    {
      "order_item_id": 1004159359,
      "quantity": 2,
      "order_item_shipment_picking_histories": []
    }
  ],
  "delivery_at": null
}
```


