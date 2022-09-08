---
internal: true
tags: [invoice, order, api]
stoplight-id: 1c2c1a2a5fd05
---

# Join an invoice to an Order

Sometimes you need to join an invoice to you Order, the document can be printed with the carrier label and/or it can be send by email to your customer

You can join an invoice by creating an [UploadedFile](https://shippingbo.stoplight.io/docs/api-rest/branches/main/f6f7c3d15275f-uploaded-file) first

```curl
curl --request POST \
  --url https://app.shippingbo.com/uploaded_files \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: __API_TOKEN__' \
  --header 'X-API-USER: __API_USER__' \
  -F "uploaded_file[file]=@path/to/your/file.pdf"
```

<!-- theme: warning -->
> The given file must be a **PDF**

Once your PDF is correctly uploaded, you can create link between your [UploadedFile](https://shippingbo.stoplight.io/docs/api-rest/branches/main/f6f7c3d15275f-uploaded-file) and an [Order](../Shipping-API-V1.json/definitions/Order) by creating an [OrderDocument](url)


```curl
curl --request POST \
  --url https://app.shippingbo.com/order_documents \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
  --data '{
  "order_id": 1,
  "uploaded_file_id": 1,
  "language": "fr",
  "type": "OrderDocument::ExternalInvoice"
}'
```
```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/order_documents');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-TOKEN' => '',
  'X-API-USER' => ''
]);

$request->setBody('{
  "order_id": 1,
  "uploaded_file_id": 1,
  "language": "fr",
  "type": "OrderDocument::ExternalInvoice"
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

url = URI("https://app.shippingbo.com/order_documents")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = ''
request["X-API-USER"] = ''
request.body = "{\n  \"order_id\": 1,\n  \"uploaded_file_id\": 1,\n  \"language\": \"fr\",\n  \"type\": \"OrderDocument::ExternalInvoice\"\n}"

response = http.request(request)
puts response.read_body
```

The document is now available in the Documents tab of the order, and according to your configuration the invoice can be send in a email and/or print with the shipping label

<!-- theme: success -->

> #### Good Job!
>
> You joined an **invoice** to your order!