---
stoplight-id: i15i5zn8ewpo0
---

# Introduction

<!-- theme: warning -->

## General principles

Shippingbo uses a **REST API** that accepts **JSON** format.

The API is accessible through the same endpoint as the backoffice: <https://app.shippingbo.com>

You can interact with the different resources using the following actions:

> You must check which HTTP verbs are available on a resource before using it, if the verb is not authorized on a resource the API will respond with a **404 Not found**

| Verb HTTP | Path                          | Use for                  |
| --------- | ----------------------------- | ------------------------ |
| GET       | /resources                    | List records             |
| GET       | /resources/{id}               | Record details           |
| POST      | /resources                    | Create a record          |
| PATCH     | /resources/{id}               | Update a record          |
| DELETE    | /resources/{id}               | Delete a record          |
| *         | /resources/{id}/custom_action | Custom action on record  |
| *         | /resources/custom_action      | Custom action on records |

## API specifications

- A resource's name is snake_cased and plural in the route
- Actions may be restricted per resource - depending on business rules and your account configuration.
- Empty fields can be omitted
- Returned data can be found in a field named as the resource (snake_cased) - pluralized when listing. E.g.: _Order creation returns { "order" => {...}}_
- GET /orders returns { "orders" => \[{..}, {...}]}
- Read, create and update return the serialized object. In particular, the field _id_ gives you our internal reference after creation
- Returned date are in UTC with server timezone: "%FT%T%:z" (2018-07-06T15:54:13+00:00)
- During updates, you should only send the fields to update to avoid race conditions with other users or other parts of the system
- On a same API, fields can be added after system updates, fields order in the response is not guaranteed as well. Your integration must be robust to such changes. However existing fields will be preserved.
- The following fiel, y are always returned: _id_, _created_at_ and _updated_at_
- Output will be UTF-8 encoded and all input data must be UTF-8 as well.
- Return format of errors are: _{"errors":{"message":\["error message"]}}_
- API users are expected to have a reasonable usage of API resources by following these principles - actions ensuring platform stability may be taken otherwise :
  - Limit the number of simultaneous requests
  - Use webhooks instead of polling when possible
  - Do not create or update resources you do not need

## Pagination

Get parameters usable in the index requests to control the pagination:

- limit (default and maximum: 50)
- offset

For exemple to get the 50 first records you can make:

```curl
curl --request GET \
  --url https://app.shippingbo.com/orders?limit=50&offset=0 \
  --header 'Content-Type: application/json' \
  --header 'X-API-ACCESS-TOKEN: ' \
  --header 'X-API-VERSION: ' \
  --header 'X-API-APP-ID: ' \
}'
```

## Environments

In the API documentation you will see 2 differents environments to test your request: Staging and Production

### Staging

Staging is a specific environment for special integration that cannot be tested on the production environment
This environment is not stable, new features can be tested on it before being deployed on Production

### Production

Be careful with the Production environment, requests are not mocked and will be accepted. All tested requests will be not rollback-able

## SFTP implementations

Some workflows are available in SFTP like `Order` creation, stock synchronisation, etc...

If an article has an SFTP tab available that means you can implements this flow with SFTP. You will find files exemples in the tab.

## Get help

### Ask to ️ ️t️h️e️ ️c️o️m️m️u️n️i️t️y️ on [Stackoverflow](https://stackoverflow.com/search?q=shippingbo)

Create a topic on [Stackoverflow](https://stackoverflow.com/search?q=shippingbo) tagged **OMS**, **WMS** (and **shippingbo** if possible) and mention Shippingbo in the question

Shippingbo developers and the community will be happy to help you

### Ask to support and give us your feedback ✉️

If you have any question or feedback about our APIs and documentation please contact: <support.api@shippingbo.com>
