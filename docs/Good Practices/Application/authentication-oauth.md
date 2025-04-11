---
stoplight-id: eq2qnbmjnwg4y
---

# Authentication with Oauth

The Oauth2 flow with your app must look like (screen from Oauth2 spec):

![image.png](../../../assets/images/image-6.png)

## Obtain the authorization URL

The authorization URL will redirect users to the Shippingbo authorization page with all information about your app

![Capture d’écran du 2024-07-16 14-37-12.png](<../../../assets/images/Capture d’écran du 2024-07-16 14-37-12.png>)


Once, the user has authorized the application he will be redirected to your `redirect_uri` with the `code`

> If the scope of your application is *label_api* we also send two other parameters : *api_client_id* and *label_credential_id* which are respectively the shippingbo client id and the shippingbo transport contract id. They are used to identify which client are making the requests

## Get the access token and the refresh token

All you need to do is to use the previously obtained `code` in the next request:

<!--
type: tab
title: Request
-->

```curl
curl --request POST \
  --url https://oauth.shippingbo.com/oauth/token \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{
  "grant_type": "authorization_code",
  "client_id": "jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14",
  "client_secret": "vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA",
  "code": "fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0",
  "redirect_uri": "https://oauth.yourapp.com/redirect"
}'
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://oauth.shippingbo.com/oauth/token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'grant_type' => 'authorization_code',
    'client_id' => 'jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14',
    'client_secret' => 'vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA',
    'code' => 'fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0',
    'redirect_uri' => 'https://oauth.yourapp.com/redirect'
  ]),
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "Content-Type: application/json"
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

url = URI("https://oauth.shippingbo.com/oauth/token")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["Accept"] = 'application/json'
request.body = "{\n  \"grant_type\": \"authorization_code\",\n  \"client_id\": \"jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14\",\n  \"client_secret\": \"vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA\",\n  \"code\": \"fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0\",\n  \"redirect_uri\": \"https://oauth.yourapp.com/redirect\"\n}"

response = http.request(request)
puts response.read_body
```

<!--
type: tab
title: Response
-->

```json
{
    "access_token": "WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw",
    "token_type": "Bearer",
    "expires_in": 7200,
    "refresh_token": "edxikEVl9oN4SBjLo8gga0B0IitHEQE44_8zL9ewvs0",
    "scope": "order",
    "created_at": 1677505124
}
```

<!-- type: tab-end -->

When the `access_token` is expired you have to get a new one with the previous `refresh_token`

## Refresh the token

<!--
type: tab
title: Request
-->

```curl
curl --request POST \
  --url https://oauth.shippingbo.com/oauth/token \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --data '{
  "grant_type": "refresh_token",
  "client_id": "jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14",
  "client_secret": "vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA",
  "refresh_token": "fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0",
  "redirect_uri": "https://oauth.yourapp.com/redirect"
}'
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, [
  CURLOPT_URL => "https://oauth.shippingbo.com/oauth/token",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'grant_type' => 'refresh_token',
    'client_id' => 'jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14',
    'client_secret' => 'vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA',
    'refresh_token' => 'fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0',
    'redirect_uri' => 'https://oauth.yourapp.com/redirect'
  ]),
  CURLOPT_HTTPHEADER => [
    "Accept: application/json",
    "Content-Type: application/json"
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

url = URI("https://oauth.shippingbo.com/oauth/token")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true

request = Net::HTTP::Post.new(url)
request["Content-Type"] = 'application/json'
request["Accept"] = 'application/json'
request.body = "{\n  \"grant_type\": \"refresh_token\",\n  \"client_id\": \"jZgZZTwwdJXzBTN8btkssqO6UrbZriha5DX0rxdqj14\",\n  \"client_secret\": \"vqVUUuChI7UNZ-z0DKdYILr0XY0QbOslD1clDRlBmoA\",\n  \"refresh_token\": \"fd0847dbb559752d932dd3c1ac34ff98d27b11fe2fea5a864f44740cd7919ad0\",\n  \"redirect_uri\": \"https://oauth.yourapp.com/redirect\"\n}"

response = http.request(request)
puts response.read_body
```

<!--
type: tab
title: Response
-->

```json
{
    "access_token": "WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw",
    "token_type": "Bearer",
    "expires_in": 7200,
    "refresh_token": "edxikEVl9oN4SBjLo8gga0B0IitHEQE44_8zL9ewvs0",
    "scope": "order",
    "created_at": 1677507894
}
```

<!-- type: tab-end -->

## Request Shippingbo API

To request Shippingbo ressources you must include 4 headers:

- **X-API-VERSION**: is the current version of the API
- **Authorization**: the token you get in the previous request as a [Bearer token](https://en.wikipedia.org/wiki/OAuth), the header **X-API-ACCESS-TOKEN** is also accepted to pass the token
- **X-API-APP-ID**: this is the value of `Shippingbo APP ID` in your application dashboard
- **User-Agent**: `[YourOauthApplicationName]/[Version.0][comments]` The "User-Agent" header field contains information about the user
   agent originating the request, which is often used by servers to help
   identify the scope of reported interoperability problems.
   For more information: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent

```curl
curl --location 'https://app.shippingbo.com/orders' \
--header 'X-API-VERSION: 1' \
--header 'User-Agent': "My CMS plugin/1.0" \
--header 'Authorization: Bearer WpS-eTz0ZQMq_91O-vH4Q_FobPrXfk8a4jtLwVt6-gw' \
--header 'X-API-APP-ID: 2'
```
