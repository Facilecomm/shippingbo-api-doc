---
internal: true
---

# Authentication

<!--
type: tab
title: API
-->
The Shippingbo API has implemented four additonnals headers **X-API-TOKEN**, **X-API-USER**, **X-API-CLIENT-ID** and **X-API-VERSION** they will be communicated by the Shippingbo Team

You should provide them in the header of each request

For exemple if you create an [Address](https://developer.shippingbo.com/docs/api/5eb654bfa3b51-create-an-address):

```curl
curl --request POST \
  --url https://app.shippingbo.com/addresses \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: __YOUR_API_TOKEN__' \
  --header 'X-API-USER: __YOUR_API_USER__' \
  --header 'X-API-USER-ID: __YOUR_API_USER_ID__' \
  --header 'X-API-VERSION: 1' \
  --data '{
  "city": "Toulouse",
  "zip": "31400",
  "firstname": "John",
  "lastname": "Doe",
  "instructions": null,
  "phone1": "0102030405",
  "phone2": "0501020304",
  "email": "john.doe@exemple.com",
  "street1": "3 avenue de l'\''europe",
  "street2": null,
  "street3": null,
  "country": "FR",
  "company_name": "Shippingbo"
}'
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('https://app.shippingbo.com/addresses');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders([
  'Content-Type' => 'application/json',
  'X-API-TOKEN' => '__YOUR_API_TOKEN__',
  'X-API-USER' => '__YOUR_API_USER__',
  'X-API-USER-ID' => '__YOUR_API_USER_ID__',
  'X-API-VERSION' => '1'
]);

$request->setBody('{
  "city": "Toulouse",
  "zip": "31400",
  "firstname": "John",
  "lastname": "Doe",
  "instructions": null,
  "phone1": "0102030405",
  "phone2": "0501020304",
  "email": "john.doe@exemple.com",
  "street1": "3 avenue de l'europe",
  "street2": null,
  "street3": null,
  "country": "FR",
  "company_name": "Shippingbo"
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

url = URI("https://app.shippingbo.com/addresses")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["X-API-TOKEN"] = '__YOUR_API_TOKEN__'
request["X-API-USER"] = '__YOUR_API_USER__'
request["X-API-USER-ID"] = '__YOUR_API_USER_ID__'
request["X-API-VERSION"] = '1'
request.body = "{\n  \"city\": \"Toulouse\",\n  \"zip\": \"31400\",\n  \"firstname\": \"John\",\n  \"lastname\": \"Doe\",\n  \"instructions\": null,\n  \"phone1\": \"0102030405\",\n  \"phone2\": \"0501020304\",\n  \"email\": \"john.doe@exemple.com\",\n  \"street1\": \"3 avenue de l'europe\",\n  \"street2\": null,\n  \"street3\": null,\n  \"country\": \"FR\",\n  \"company_name\": \"Shippingbo\"\n}"

response = http.request(request)
puts response.read_body
```

If the API respond with a status code **200** your credentials and connection are operationals

<!--
type: tab
title: FTP
-->


<!-- type: tab-end -->