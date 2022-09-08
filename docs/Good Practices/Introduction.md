---
stoplight-id: i15i5zn8ewpo0
---

# Introduction

<!-- theme: warning -->
> Somes cases is not yet documented is this version of the documention, if you don't find what you are looking for please refer to [the old documentation](https://developer.shippingbo.com/docs/api/branches/main/azqm8hjs8olad-general-principles)

## General principles

Shippingbo API is **REST** and use **JSON** format.

API is accessible through the same endpoint as the backoffice: https://app.shippingbo.com

You can interact with the different resources with the following actions:

> You must check available HTTP verb on a resource before use it, if the verb is not authorized on a resource the API will respond a **404 Not found**

| Verb HTTP  |Path|Use for|
|---|---|---|
|GET|/resources|List records|
|GET|/resources/{id}|Record details|
|POST|/resources|Create a record|
|PATCH|/resources/{id}|Update a record|
|DELETE|/resources/{id}|Delete a record|
|*|/resources/{id}/custom_action|Custom action on record|
|*|/resources/custom_action|Custom action on records|

## API specifications

- Resources name is snake_cased and pluralized in the route
- Actions may be restricted per resource - depending on business rules and your account configuration.
- Empty fields can be omitted
- Returned data can be found in a field named as the resource (snake_cased) - pluralized when listing. E.g.: *Order creation returns { "order" => {...}}*
- GET /orders returns { "orders" => [{..}, {...}]}
- Read, create and update return the serialized object. In particular, the field *id* gives you our internal reference after create
- Date format is "%FT%T%:z" (2018-07-06T15:54:13+00:00)
- During updates, you should only send the fields to update to avoid race conditions with other users or other parts of the system
- Your integration must be robust to such changes. However existing fields will be preserved.
- The following fields are always returned: *id*, *created_at* and *updated_at*
- Output will be UTF-8 encoded and all input data must be UTF-8 as well.
- Return format of errors is: *{"errors":{"message":["error message"]}}*

## Pagination

Get parameters usable in the index requests to control the pagination:

- limit (max: 50)
- offset

For exemple to get the 50 first records you can make:

```curl
curl --request GET \
  --url https://app.shippingbo.com/orders?limit=50&offset=0 \
  --header 'Content-Type: application/json' \
  --header 'X-API-TOKEN: ' \
  --header 'X-API-USER: ' \
}'
```

## SFTP implementations

Some workflows are available in SFTP like `Order` creation, stock synchronisation, etc...

If an article has an SFTP tab available that means you can implements this flow with SFTP. You will find files exemples in the tab