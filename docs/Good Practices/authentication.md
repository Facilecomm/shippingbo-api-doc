---
internal: true
---

# Authentication

<!--
type: tab
title: API
-->
The Shippingbo API has implemented two additonnals headers **X-API-TOKEN** and **X-API-USER**, both of them will be communicated by the Shippingbo Team

You should provide them in the header of each request

For exemple if you create an [Address](https://shippingbo.stoplight.io/docs/api-rest/c2NoOjUwNDM4NjU2-address):

```curl
curl --request POST \
  --url https://app.shippingbo.com/addresses \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: __YOUR_API_TOKEN__' \
  --header 'X-API-USER: __YOUR_API_USER__' \
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

<!--
type: tab
title: FTP
-->


<!-- type: tab-end -->