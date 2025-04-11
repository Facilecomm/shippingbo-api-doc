---
stoplight-id: drj3mhj4w3s2r
---

# Throttling

## Limitation

### Main routes limitation

| Method| Route                 | Limit (request per minut) |
| ----------|----------- | ------------------------- |
| GET |/address_label_summaries          | 200                       |
| POST| /addresses          | 100                       |
| GET |/addresses/{addressId}          | 100                       |
| PATCH| /addresses          | 100                       |
| GET |/label_provider_client_references          | 100                       |
| POST |/orders/{orderId}/recompute_mapped_products          | 200|
| POST| /order_documents        | 50                        |
| GET| /order_item_product_mappings        | 200                        |
| PATCH| /order_items/{orderItemId}        | 200                        |
| POST| /order_tags        | 200                        |
| DELETE| /order_tags/{orderTagId}        | 200                        |
| POST| /orders        | 200                        |
| GET| /orders        | 200                        |
| GET| /orders/{orderId}        | 200                        |
| PATCH| /orders/{orderId}        | 200                        |
| GET |/pack_components       | 200                        |
| POST |/pack_components       | 200                        |
| DELETE| /pack_components/{packComponentId}       | 200                        |
| GET |/product_additional_fields       | 200                        |
| PACTH| /product_additional_fields/{productAdditionalFieldId}       | 200|
| GET| /product_barcodes        | 200                        |
| POST| /product_barcodes        | 200                        |
| DELETE| /product_barcodes/{productBarcodeId}        | 200                        |
| GET| /product_stocks/{productId}        | 200                        |
| POST |/products        | 200                        |
| PATCH| /products/{productId}        | 200                        |
| GET |/products        | 200                        |
| GET| /products/{productId}        | 200                        |
| GET| /return_label_summaries        | 200                        |
| POST| /shipments        | 200                        |
| POST| /stock_variations        | 200                        |
| POST| /stock_variations/perform_variations        | 200       |
| POST| /supplier_products       | 200       |
| POST| /supply_capsules       | 200       |
| GET| /supply_capsules       | 200       |
| GET |/supply_capsules/{supplyCapsuleId}       | 200       |
| POST| /uploaded_files       | 50       |
| GET| /warehouse_slots       | 200       |

### Basic routes limitation

| Route      | Limit (request per minut) |
| ---------- | ------------------------- |
| INDEX /*   | 50                        |
| SHOW /*    | 60                        |
| DESTROY /* | 60                        |
| * /*       | 50                        |

⚠️ If you reach the limit of basic routes, we highly recommend to use webhooks

## Response in case of throttling

The API will respond with an `HTTP 429 Retry Later`

## Web Application Firewall (WAF)

If you get a 403 error with the error message : “Request blocked by security policy.”, this is related to our Web Application Firewall (WAF). In this case, please contact us.

Make sure that you verify your IP reputation, IP addresses known for malicious behaviors will be automatically blocked (for instance via https://www.apivoid.com/tools/ip-reputation-check/ or https://www.abuseipdb.com/)
