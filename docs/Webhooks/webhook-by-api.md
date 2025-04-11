---
stoplight-id: 431ol7veh7w2x
tags: [webhook]
---

# Implement webhooks by api

## Implement new webhook

To create, list or update webhooks, you can refer [these](https://developer.shippingbo.com/docs/api/t33opda3f7er8-create-webhooks) endpoints

Each hook callback contains the identifier of the Updatehook in his body.

<!--
type: tab
title: Request
-->

```json
{ 
  "update_hook": {
    "object_class": "Order",
    "endpoint_url": "https://your_call_back_url.com",
    "activated": true
  }
}

```

<!--
type: tab
title: Response
-->

```json
{ 
  "update_hook": {
    "id": 952149888,
    "created_at": "2024-03-06T10:14:40+00:00",
    "updated_at": "2024-03-06T10:14:40+00:00",
    "object_class": "Order",
    "field": null,
    "endpoint_url": "https://your_call_back_url.com",
    "from_value": null,
    "to_value": null,
    "trigger_on_destroy": false,
    "activated": true,
    "visible": true,
    "auth_scheme": null,
    "filters_as_json": "null",
    "available_keys": [
        "fulfilled_by_marketplace",
        "source",
        "origin",
        "associated_label_config",
        "state"
    ],
    "available_operators": [
        "==",
        "!="
    ]
  }
}
```

<!--
type: tab
title: Example
-->

```curl
curl --request POST \
  --url https://app.shippingbo.com/update_hooks \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'X-API-ACCESS-TOKEN: 123' \
  --header 'X-API-APP-ID: 123' \
  --header 'X-API-VERSION: 123' \
  --data '{
  "object_class": "Order",
  "endpoint_url": "https://your_call_back_url.com",
  "activated": true
}'
```

<!-- type: tab-end -->

### Verify the webhook

Each webhook request includes a base64-encoded `X-Shippingbo-Hmac-SHA256` header, which is generated using the app's client secret along with the data sent in the request. If you're using PHP, or a Rack-based framework such as Ruby on Rails or Sinatra, then the header is `HTTP_X_SHIPPINGBO_HMAC_SHA256`.

#### Compute the HMAC digest according to the following algorithm

```ruby
# The following example uses Ruby and the Sinatra web framework to verify a webhook request:
require 'rubygems'
require 'base64'
require 'openssl'
require 'sinatra'
require 'active_support/security_utils'
# The Shippingbo app's client secret, viewable from the Developer Dashboard. In a production environment, set the client secret as an environment variable to prevent exposing it in code.
CLIENT_SECRET = 'my_client_secret'
helpers do
  # Compare the computed HMAC digest based on the client secret and the request contents to the reported HMAC in the headers
  def verify_webhook(data, hmac_header)
    calculated_hmac = Base64.strict_encode64(OpenSSL::HMAC.digest('sha256', CLIENT_SECRET, data))
    ActiveSupport::SecurityUtils.secure_compare(calculated_hmac, hmac_header)
  end
end
# Respond to HTTP POST requests sent to this web service
post '/' do
  request.body.rewind
  data = request.body.read
  verified = verify_webhook(data, env["HTTP_X_SHIPPINGBO_HMAC_SHA256"])

  halt 401 unless verified

  # Process webhook payload
  # ...
end
```

```php
# The following example uses PHP to verify a webhook request:
<?php
# The Shippingbo app's client secret, viewable from the Developer Dashboard. In a production environment, set the client secret as an environment variable to prevent exposing it in code.
define('CLIENT_SECRET', 'my_client_secret');
function verify_webhook($data, $hmac_header)
{
  $calculated_hmac = base64_encode(hash_hmac('sha256', $data, CLIENT_SECRET, true));
  return hash_equals($calculated_hmac, $hmac_header);
}

$hmac_header = $_SERVER['HTTP_X_SHIPPINGBO_HMAC_SHA256'];
$data = file_get_contents('php://input');
$verified = verify_webhook($data, $hmac_header);
error_log('Webhook verified: '.var_export($verified, true)); // Check error.log to see the result
if ($verified) {
  # Process webhook payload
  # ...
} else {
  http_response_code(401);
}
?>
```

```python
# The following example uses Python and the Flask framework to verify a webhook request:

from flask import Flask, request, abort
import hmac
import hashlib
import base64

app = Flask(__name__)

# The Shippingbo app's client secret, viewable from the Developer Dashboard. In a production environment, set the client secret as an environment variable to prevent exposing it in code.
CLIENT_SECRET = 'my_client_secret'

def verify_webhook(data, hmac_header):
    digest = hmac.new(CLIENT_SECRET.encode('utf-8'), data, digestmod=hashlib.sha256).digest()
    computed_hmac = base64.b64encode(digest)

    return hmac.compare_digest(computed_hmac, hmac_header.encode('utf-8'))

@app.route('/webhook', methods=['POST'])
def handle_webhook():
    data = request.get_data()
    verified = verify_webhook(data, request.headers.get('X-Shippingbo-Hmac-SHA256'))

    if not verified:
        abort(401)

    # Process webhook payload
    # ...

    return ('', 200)

```

#### Compare values

Compare the computed value to the value in the `X-Shippingbo-Hmac-SHA256` or `HTTP_X_SHIPPINGBO_HMAC_SHA256` header.

If the HMAC digest and header value match, then the webhook was sent from Shippingbo.

#### Respond to the webhook

You must respond with a `HTTP 200 OK` to indicate you received the webhook
