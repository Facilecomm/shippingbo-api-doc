---
stoplight-id: azqm8hjs8olad
---

<!-- theme: warning -->
> Caution ! Here is the old version of Shippingbo API (V1.50), we recommend to use the [current version of the documentation](https://developer.shippingbo.com/docs/api/branches/main/i15i5zn8ewpo0-introduction). If you don't find what you are looking for please refer to the old documentation

# General principles:

Shippingbo API is REST.

API is accessible through the same endpoint as the backoffice: https://app.shippingbo.com

You can interact with the different resources with the following actions:


<table>
  <tr>
   <td>Verb HTTP
   </td>
   <td>Path
   </td>
   <td>Used for
   </td>
  </tr>
  <tr>
   <td>GET
   </td>
   <td>/resources
   </td>
   <td>List entries
   </td>
  </tr>
  <tr>
   <td>GET
   </td>
   <td>/resources/:id
   </td>
   <td>Entry details
   </td>
  </tr>
  <tr>
   <td>POST
   </td>
   <td>/resources
   </td>
   <td>Create new entry
   </td>
  </tr>
  <tr>
   <td>PATCH
   </td>
   <td>/resources/:id
   </td>
   <td>Modify an entry
   </td>
  </tr>
  <tr>
   <td>DELETE
   </td>
   <td>/resources/:id
   </td>
   <td>Delete an entry
   </td>
  </tr>
  <tr>
   <td>*
   </td>
   <td>/resources/:id/custom_action
<p>
or
<p>
/resources/custom_action
   </td>
   <td>Some resources have custom actions
   </td>
  </tr>
</table>

Notes:

* Data format is JSON
* resource name is snake_cased and pluralized in the route
* Actions may be restricted per resource - depending on business rules and your account configuration.
* Empty fields can be omitted
* Returned data can be found in a field named as the resource (snake_cased) - pluralized when listing. E.g.:
    *  Order creation returns { ‘order’ => {...}}
    * GET /orders returns { ‘orders’ => [{..}, {...}]}
* Read, create and updates return the serialized object. In particular, the field ‘id’ gives you our internal reference after create
* Date format is "%FT%T%:z" (2018-07-06T15:54:13+00:00)
* During updates, you should only send the fields to update to avoid race conditions with other users or other parts of the system
* On a same API, fields can be added after system updates, fields order in the response is not guaranteed as well. Your integration must be robust to such changes. However existing fields will be preserved.
* The following fields are always returned: id, created_at and updated_at
* Output will be UTF-8 encoded and all input data must be UTF-8 as well.
* Return format of errors is: {"errors":{"message":["error message"]}}

# Authentication and headers

Authentication is done with HTTP headers

X-API-USER: _API_ACCOUNT_ID_

X-API-TOKEN: _API Token_

X-API-VERSION: 1

Pagination

Get parameters usable in the index requests to control the pagination

limit: int, // Max 50

offset: int // Default 0, allows you to skip the offset-th first results

# Resources

## Address

**address** represents an address (in particular the shipping address), many fields are available to better match your data representation. The system will then map these to the carrier available fields.

```json
{
  "civility": "civility - free field",
  "fullname": "",
  "firstname": "",
  "lastname": "",
  "street1": "",
  "street2": "",
  "street3": "",
  "street4": "",
  "city": "",
  "zip": "",
  "state": "",
  "country":"2 letter country code",
  "email": "",
  "phone1": "Land phone",
  "phone2": "Mobile phone",
  "instructions": "Comments for shipping",
  "building": "",
  "company_name": "",
  "apartment_number": "",
  "place_name": ""

}
```

**Remarks:**



* It is best to precise firstname et lastname instead of fullname if you already have the details
* state: Best left empty for France
* Only country is mandatory - but that won’t be enough for a successful shipment
* If you do not know if a phone is mobile or land, you can use either field. The system will later reclassified them.


## Order

```json
{
  "source": "Order source => technical field",
  "source_ref": "Id field for the source => technical field",
  "state": "Current order state (see list below)",
  "shipping_address_id": "_id_address_",
  "origin": "Order origin - Display field",
  "origin_ref": "Origin reference - display field",
  "custom_state": "Raw order state (optional)",
  "origin_created_at": "Order creation date",
  "chosen_delivery_service": "Carrier - free field",
  "relay_ref": "Pickup point reference in the carrier system (optional)",
  "total_price_cents": "Total price cents - amount paid by the buyer",
  "total_without_tax_cents": "Same as total price, bit tax excluded",
  "total_tax_cents": "taxes corresponding to total price",
  "total_price_currency": "3 letter code (EUR)",
  "total_shipping_tax_included_cents": 0, // int >= 0 Shipping price in cents
  "total_shipping_cents": 0, // Shipping prices tax excluded,
  "total_shipping_tax_cents": "EUR", // Taxes for shipping prices,
  "total_shipping_tax_included_currency": "3 letter code (EUR)",
  "total_discount_tax_included_cents": "Total order discount - tax included",
  "total_discount_tax_included_currency": "3 letter code (EUR)",
  "total_weight": "Total weight - in grams",
  "shipped_at": "datetime of shipped",
  "closed_at": "datetime of delivery (state closed)",
  "state_changed_at": "latest change of state",
  "earliest_shipped_at": "earliest date for shipping",
  "latest_shipped_at": "latest date for shipping",
  "earliest_delivery_at": "earliest date for delivery",
  "latest_delivery_at": "latest date for delivery",
  "earliest_chosen_delivery_at": "earliest chosen date for delivery",
  "latest_chosen_delivery_at": "latest chosen date for delivery"
}
```

**Remarks:**

* Orders to be shipped, should have the "to_be_prepared" status
* (source, source_ref) as a unicity constraint
* On the other side (origin, origin_ref) unicity is not enforced - standard case is to use the same as source, source_ref
* source must be capitalized ("Awesome site" and not "Awesome Site")
* You can use the same address on many orders, but it means that any change through API or UI will impact all related orders.
* All prices is as written on the order and not as computed - so depending on the source, not the same fields will be filled in
* total_weight can be written but will be overridden by the computed weight if possible
* After changing order_items quantities or product_ref, you have to call **"POST /orders/:order_id:/recompute_mapped_products"** so that is it taken into account.
* In order to make the price computation working correctly, you need to provide **in the creation call** _total_price_cents_, _total_price_currency_, _total_tax_cents_, _total_tax_currency_, _total_shipping_tax_included_cents_, _total_shipping_tax_included_currency_, _total_shipping_tax_cents_ and _total_shipping_tax_currency_ in the Order payload. In the _order_items_attributes_ you need to provide _price_tax_included_cents_, _price_tax_included_currency_ and _tax_cents_ and _tax_currency_

**Additional fields on create:**
```json
{
  "order_items_attributes": [_order_item_, ...], // See next resource
  "tags_to_add": ["tag1", "tag2", ...] // This will add the tags on creation
}
```
**Additionally returned fields :**

* **order_items**: list of items in the order
* **mapped_products**: computed list of products that match the order_items.
* **shipments**: shipments that have been made for this order
* **shipping_address** and **billing_address**: serialized addresses

**Handling create timeout :**

It might happen that the create request times out, but the order is still created.

When retrying the call with the same data, you will get a 409 response code.(source, source_ref) unicity prevents duplication. To retrieve the Shippingbo ID of the order, you can search on these fields

**GET /orders?search[source]=_source_&search[source_ref]=_source_ref_**

It will return at most one order.

**Update order items:**

You can update order items by using the next endpoint:

**POST** **/orders/_:order_id_/update_order_items**

Exemple payload:

```json
{
  "order_items": 
    [
      { 
        "id": order_item_id,
        "product_ref": "my_sku",
        "title": "My Product",
        "quantity": 3,
        "product_source": "my source",
        "product_source_ref": "my product source ref",
        "product_ean": "my product ean13"
      }
    ]
}
```

The **order_id** is the ID returned by Shippingbo at the Order creation

The **order_item_id** is the ID returned by Shippingbo at the Order creation in the order items list or the ID when you create an OrderItem

**Order state list:**

- at_pickup_location
- back_from_client
- canceled
- closed
- dispatched
- handed_to_carrier
- in_preparation
- in_trouble
- merged
- partially_shipped
- rejected
- sent_to_logistics
- shipped
- splitted
- to_be_prepared
- waiting_for_payment
- waiting_for_stock


## OrderItem

```json
{
  "source": "Same as order",
  "source_ref": "id of the line in the source",
  "product_ref": "SKU",
  "title": "Description",
  "quantity": 1, // int >= 0 Quantity
  "price_tax_included_cents": 0, // int >= 0 Price of the items in cents (including taxes, excluding shipping,
  "price_cents": 0,// price_tax_included tax excluded,
  "tax_cents": 0,// taxes linked to price
  "price_tax_included_currency": "3 letter code (EUR)",
}
```
**Remarks:**



* All fields are mandatory
* source_ref: Must only be unique within the order
* product_ref: If it matches a product, then the order will be associated to it.
* The price is as written on the item and not the computed version
* To update order items on an order, you can call:

POST /orders/:id/update_order_items

```json
  {
    "order_items": [
      {
        "id": order_item_id,
        "product_ref": "",
        "title": "",
        "product_ean": "",
        "quantity": int
      },
    ...
  ]
}
```


## OrderEvent

```json
{
  "order_id": 123, // id of the order
  "level": "enum", // info or error
  "title": "Message title",
  "message": "Message body",
  "email": "Read-only - email of the user who created the message"
}
```

**Remarks:**

* Use with parsimony, depending on account configuration, it may send an email to other users on every message.


## MappedProduct

**mapped_product** is a computed link between products and order_items

```json
{
  "order_item_id": 123, // id of the order_item
  "product_id": 123, // id of the product
  "quantity": 1, // int
}
```

**Remarks:**

* These are computed at order creation
* When using packs, you can get multiple mapped products per item
* quantity can be different for the same reason

## Shipment

```json
{
  "order_id": ,
  "carrier_id": "Shippingbo ID of the carrier",
  "carrier_name": "",
  "shipping_method_id": "Shippingbo ID of the shipping method",
  "shipping_method_name": "",
  "shipping_ref": "Tracking number in the carrier system",
  "tracking_url": "",
  "package_id": 0, // id of the underlying package
  "carrier_barcode": "Barcode returned by the carrier",
  "delivery_at": "Date of delivery",
  "user_id": 12, // User having generated the shipment - RO field
  "items_shipment_serial_numbers": [{"order_item_id": ... ,"serial_number":"str"}], // RO field indicating if serial numbers have been attached to the shipment.
  "order_items": [{"order_item_id": 123, "item_quantity": float}], // Write Only field allowing you to specify which items are included in the shipment and in which quantity.
  "order_items_shipment": [
    {
      "order_item_id": "",
      "quantity": "",
      "order_item_shipment_picking_histories": [
        { "batch_number": "Batch number of the product", "last_preparation_day": "best before date format YYYY-MM-DD" }
       ]
    }
  ],
  "supplier_package_id": "package_id found on the shipment on the supplier who send this shipment",
  "supplier_consignment_id": "consignment_id found on the shipment on the supplier who send this shipment",
}
```

**Remarks:**

* When creating a shipment, you  must include carrier_id and shipping_method_id. A mapping will be provided to you.
* An order can have multiple shipments when shipped in multiple packages or when shipped multiple times
* created_at of the shipment is the shipping date of the order
* Make sure that you use tracking url and do not rebuild these as the url cannot be directly rebuild from the shipping ref for all carriers
* package_id can be null when the preparation has not been done through Shippingbo.
* When creating a shipment, you can indicate that the order and all items have been shipped with the additional parameters   "ship_order": true and "ship_items": true or you can specify the quantities in the shipment:

```json
{
  "order_items": [  	
    {
      "order_item_id": "81328",
      "item_quantity": 2
    },
    ...
  ]
}
```

## Consignment

```json
{
  "order_id": 0, // id of the order
  "package_ids": [123, 456], // ids of packages
  "address_label_config_id": 123
}
```

**Remarks:**

* A consignment holds several packages shipped together


## Package

```json
{
  "order_id": 1,
  "total_weight": "(in grams)",
  "insurance_value_cents": 12, // int >= 0 in cents
  "insurance_value_currency": "3 letter code => EUR",
  "picking_control_validated": true/false,
  "length": "Declared length (mm)",
  "height": "Declared height (mm)",
  "width": "Declared width (mm)",
  "order_item_packages": [order_item_package, ...]
}
```

**Remarks:**

* Having a package does not mean the order is shipped. It indicates the start of a preparation.
* Insurance only applies if carrier supports id
* picking_control_validated indicates that the package content has been checked before shipping
* order_item_packages: package content. Only meaningful if picking_control_validated is true





## OrderItemPackage

```json
{
  "package_id": 123,
  "order_item_id": 456,
  "item_quantity": 1, // int >= 0 Item quantity
  "product_id": 789,
  "product_quantity": 1, // int >= 0 Product quantity
  "serial_numbers": ["123", "azerty", ...] 
}
```

**Remarks:**

* item_quantity et product_quantity are incompatible, only one will be filled depending the preparation workflow.

## AddressLabel

**address_label** is a carrier label.

```json
{
  "package_id": 123,
  "address_label_config_id": "Label account id in Shippingbo",
  "label_url": "URL of the PDF label",
  "shipping_ref": "Shipping reference from the carrier"
}
```

**Remarks**:

* There is an access control on the label_url, so you need to authenticate with the same headers
* This is Read-Only


## AddressLabelCollection

```json
{
  "consignment_id": 123, // int
  "address_labels": [list of address_labels]
}
```

**Remarks**:

* This is a create only route

## Product

**product is a physical product**.

```json
{
  "source": "Constant identifying your system",
  "source_ref": "Unique reference in source",
  "user_ref": "SKU",
  "stock": "Available stock",
  "ean13": "",
  "title": "",
  "location": "Location in warehouse",
  "weight": "in grams",
  "picture_url": "Public picture URL",
  "hs_code": "used for customs",
  "supplier": "",
  "other_ref1": "Free field 1",
  "other_ref2": "...",
  "other_ref3": "...",
  "other_ref4": "...",
  "other_ref5": "...",
  "other_ref6": "...",
  "other_ref7": "...",
  "other_ref8": "...",
  "other_ref9": "...",
  "other_ref10": "...",
  "other_ref11": "...",
  "other_ref12": "...",
  "other_ref13": "...",
  "other_ref14": "...",
  "other_ref15": "...",
  "width" : null,
  "height" : null,
  "length": null,
  "additional_references": [Array of OrderItemProductMapping],
  "total_physical_stock": "...",
  "supplier_product_ids": [Array of SupplierProduct]
}
```

**Remarks**:



* source,  source_ref, user_ref and title are required
* (source, source_ref) must be uniq
* dimensions (width, length and height) are in millimeters




## PackComponent

**The pack component defines one element of a pack**
```json
{

  "pack_product_id": "id of the product supporting the pack",
  "component_product_id": "id of the product composing the pack",
  "quantity": "Quantity of the component product within the pack"
}
```

**Remarks**:



* You can list the components of a pack through

  _GET /pack_components?pack_product_id=XXXXX_



## ReturnOrder

```json
{
"user_email": "Email of the user who created the return order",
"state": "ENUM: new / canceled / returned / closed",
"returned_at": "DateTime of processing",
"order_id": "Associated order id",
"source": "The source of the order",
"source_ref": ""Order reference on the source,
"reason": "User provided",
"reason_ref": "User provided",
"return_order_type": "ENUM return_order_label / return_order_carrier / return_order_customer",
"return_order_expected_items": ["Items expected to be returned"],
"return_order_items": ["Actually returned items"]
}
```

**Remarks**:



* At creation, you only need: order_id, reason, reason_ref,  return_order_type
* To propagate the return order in the right warehouse, you can pass _supplier_id _with the id of supplier, the return will be duplicated in the WMS and linked with the OMS
* If you don’t know the _order_id_, you can create the **ReturnOrder** with the _source _and the _source_ref_, Shippingbo will retrieve the right **Order**


## ReturnOrderItem

```json
{
  "return_order_id": "cf ReturnOrder",
  "product_id": "cf Product",
  "quantity": "int"
}
```

## ReturnOrderExpectedItem

Same as ReturnOrderItem


## SupplierProduct

**This only applies to resellers through Multi-Shippingbo setup**

```json
{
  "product_id": "",
  "supplier_id": "",
  "supplier_ref": "Reference of the supplier for this product",
  "physical_stock": "..."
}
```

## OrderItemProductMapping

**Indicates additional references of a product**

```json
{
  "order_item_field": "product_ref", // Constant - deprecated
  "product_field": "id", // Constant - deprecated
  "order_item_value": "ref (string)",
  "product_value": "product_id",
  "matched_quantity": 1 // Integer - default 1
}
```



## StockVariation

**You can only create a stock_variation**

**create_params:**

```json
{
  "product_id": "",
  "variation": int (can be &lt; 0 or => 0)
}
```

**Alternate version using sku:**

```json
{
  "product_key": "user_ref",
  "user_ref": "sku of the product"
  "variation": int (can be 0 or => 0)
}
```

**Response:**

```json
{
  "product_id": 123,
  "stock": 1 // Stock after the variation
}
```

**Remarks:**



* The returned id will be uniq, and is meaningless
* If you use the sku version and your SKUs are not unique, then only one of the products will be affected.
* If the product has a null stock (so unknown stock value), the the call will return 200 OK with an empty body

## WarehouseSlot

**Represents a specific location in the warehouse. May be empty or linked to one or more products.**

```json
{
  "name": "Identifies the slot, unique",
  "priority": "higher value means higher priority",
  "picking_disabled": "If true, then will not be selected for picking",
  "stock_disabled": "If true, then stock will accounted for in the available stock",
  "warehouse_zone_name": "Name of the zone, may be nil",
  "slot_contents": Array of slot contents; See below, 
}
```

## SlotContent

**Represents the content of a warehouse slot. Links it with product**

```json
{
  "product_id": "Id of the associated product", // Integer
  "product_ref": "user_ref of associated product",
  "warehouse_slot_id": "id of the warehouse slot", // Integer
  "warehouse_slot_name": "Name of the warehouse slot",
  "stock": "physical stock",
  "batch_number": "batch number of the products",
  "last_preparation_day": "last preparation day possible (DLUO)", format "YYYY-MM-DD",
  "stored_serial_numbers": [
    {
      "id": "Id of the associated serial_number", // Integer
      "value": "Serial number value", // String
    }
  ],
}
```

**Remarks:**



* You can create or destroy this resource.
* Destroy frees the slot, but requires stock to be 0
* On create, you can specify product_id or product_ref and warehouse_slot_id or warehouse_slot_name


## SlotStockVariation

**This is the equivalent of stock_variation but when using warehouse slots**

**Format:**

```json
{
  "product_id": Shippingbo ID of the product,
  "warehouse_slot_name": Slot name,
  "variation": > 0 if stock in, &lt; 0 if stock out,
  "origin": stock origin (list of values depending of client config),
  "origin_ref": associated origin reference,
  "destination": stock destination (list of values depending of client config),
  "destination_ref": associated destination reference,
  "reason": movement reason (list of values depending of client config),
  "reason_ref": associated movement reason,
  "serial_number": serial number value, // String
}
```

**Remarks:**



* If **serial numbers tracking enabled** and your **product has serial numbers enabled**, slot stock variation can only be made with one quantity (variation: 1 or -1) & serial_number needs to contain the serial number that will be added/removed.


## AddressLabelSummary

**This resource is limited to label providers**

```json
{
  "zip": "Destination zip code",
  "total_weight": "Declared total weight (g)",
  "length": "Declared length (mm)",
  "height": "Declared height (mm)",
  "width": "Declared width (mm)",
  "insurance_value_cents": "Declared insurance value (cents)",
  "insurance_value_currency": "Declared insurance value (currency)",
  "api_client_id": "Shippingbo ID of the shipper",
  "country": "Destination country",
  "merchant_country": "Shipping country",
  "order_id": "Shippingbo ID of the order"
  "address_label": {
    "shipping_ref": "Shipping ref of the label (carrier defined)",
    "shipped_at": "Declared shipping date (order is shipped in Shippingbo)"
  },
  "label_offer_option": {
      "service": "Service code of the used option",
  },
  "label_credential": {
      "type": "Name of the carrier",
  },

  // The following fields are only present in the resource for some authorized partners - they are returned here in the same format as what they were on the the address used for generating the label - all Read Only

  "civility": "free field",
  "fullname": "",
  "firstname": "",
  "lastname": "",
  "street1": "",
  "street2": "",
  "street3": "",
  "street4": "",
  "city": "",
  "state": "",
  "email": "",
  "phone1": "",
  "phone2": "",
  "company_name": ""
}
```

## PreparationRun

```json
{
  "preparation_search_id": int // id of the related search
  "file_url": "URL of the generated PDF",
  "package_ids": [int, ...] // List of the linked package ids.
}
```
**Remark**:



* In the case of a run with included labels,  once the label is shipped, you must call the following route to indicate that so that the order goes to the shipped state:

POST /address_labels/:address_label_id/notify_shipment_from_label


## SupplyCapsule

```json
{
  "source_ref": "identifier for the supply",
  "expected_delivery_date": "datetime",
  "supplier_name": "Display name of the supplier",
  "supplier_code": "Unique code identifying the supplier",
  "state": "State of the supply",
  "supplier_id":  "id of the sbo supplier if multi sbo",
  "supply_capsule_items_attributes": [
    {
      "source_ref": "Unique reference of the line within the supply",
      "product_ref": "SKU of the product",
      "quantity": int, // expected quantity
      "received_quantity": int // received quantity
    }
  ]
}
```

**Remark**:



* Initial state is waiting - possible states are: waiting, canceled, received
* (supplier_code, source_ref) is a unique couple
* state can be omitted on creation, waiting is default
* source_ref must be unique by item within the supply capsule


## ProductAdditionalField

```json
{
  "product_id": 123,
  "key": "Name of the field",
  "value": "Value of the field"
}
```

**Remark**:

* Some keys have a predefined usage:
    * needEan13Barcode (auto prints barcode in B2B picking)
    * serialNumbersEnabled (forces serial numbers while control picking): values can be ‘yes’ or ‘no’
    * customOriginCountry (specific origin country for customs)


## ProductBarcode

```json
{
  "product_id": "identifier for the product",
  "key": "Name of the field",
  "value": "Value of the field"
}
```

**Remark**:

* key possible are: ean
* value need to be unique
* For **INDEX **route you should add **product_id** parameter in your request: /product_barcodes?product_id={**product_id**}


## PackageSlotReservation

```json
{
  "slot_content_id": "identifier for the SlotContent",
  "package_id": "identifier for the Package reserving the stock",
  "order_id": "identifier for the Order reserving the stock",
  "quantity": "quandity of stock reserved in the Slotcontent",
}
```

## LogisticianServiceConfig

GET matching service of logistician carrier mapping :

route :

/logistician_service_configs/matching_service/PredefinedLogistician::GenericLogistician/of/{order_id}

response :

```json
{
  "logistician_service_config": {
    "id": "id of the service",
    "created_at": "creation of the service",
    "updated_at": "last update of the service",
    "predefined_logistician_id": "logistician id",
    "name": " name of the matching service",
    "config": {
      "service_code": "code of the matching service",
      "shipping_options": "shipping option if it is configured",
      "tags_to_send": "if it is configured"
    }
  }
}
```

## ProductStockVariation

GET stock variation of the product (work only for OMS with multi shippingbo enabled) :

route :

/product_stock_variations?search[product_id__eq]={product_id}

response :

```json
{
  "product_stock_variations": [
    {
      "id": "id of the service",
      "created_at": "creation of the service",
      "updated_at": "last update of the service",
      "product_id": id of the product,
      "quantity": quantity of the variation (negative if stock removed),
      "reason": "reason of the variation",
      "reason_ref": "reference of the variation",
      "origin": "origin of the variation",
      "origin_ref": "reference of the origin of the variation",
      "serial_number": "serial number of the product if have one",
      "batch_number": "batch number of the product if have one",
      "last_preparation_day": "last preparation day of the product if have one"
    }
  ]
}
```

## DangerousGoodsProductInformation

If you send dangerous goods, we need some more data to send the products.

You can configure this data by product using the following routes.

Example of response:

```json
{
    "dangerous_goods_product_information": {
        "id":  "id of the service",
        "created_at": : "creation of the service",
        "updated_at": "last update of the service",
        "product_id":"id of the product related",
        "onu_code": "",
        "packaging_group": "",
        "class_code": "",
        "label_code": "",
        "tunnel_code": "",
        "category": "",
        "packaging_type": "",
        "coefficient":"",
        "unit": "KG|L",
        "onu_description": "",
        "package_code_identification": "",
        "product_description": "",
        "limited_quantity": true|false,
        "environmentally_hazardous": true|false,
        "gross_weight":"must be set when limited_quantity is true",
        "quantity": "must be set when limited_quantity is false",
        "dhl_ref": "",
        "dhl_description": "",
        "dhl_economy_select_ref": ""
    }
}
```

For searching with product_id use:

GET /dangerous_goods_product_informations?search[product_id__eq]={product_id}

Be aware, the field _search[product_id__eq]_ is mandatory.

You will get an array of dangerous goods product information but with only one object if the product is found.


# **Notifications**

We can configure webhooks on various events

Note that most resources support webhooks, the following list is not exhaustive

**Prerequisites:**

For every notification, you must specify:



1. A url
2. A list of headers if needed

**Execution:**

When the event happen, we will make a POST request on the given URL with the following JSON body:
```json
{
  "object_class": "Order",
  "object": { serialized as in the API },
  "additional_data": { Undocumented }
}
```

**Remarks:**

The call will be retried in an increasing time interval for several days until you reply a 2XX status.

This means 2 things:



1. Your implementation must be resilient to a hook called multiple times.
2. If the error goes on for quite some time, the information may not be up to date. You can call the API in that case to get the latest data if needed.


## On orders

State change:
- "in_preparation" => Preparation process has begun. You should no longer modify the order as it may not be accounted for in the preparation.
- "shipped" => Order is shipped, there will be a linked shipment (unless shipping did not happen through Shippingbo)


## On products

When stock value changes. Warning: if you have a lot of orders, or additional product references, this may induce calls spikes on the configured URL.


## On preparation_run

You can be notified when a new run is ready, this means that the pdf is generated and labels are ready to be sent (if any)


## Testing hooks

Hooks can be simulated in dev using various solutions:



1. You have a test / preprod / staging environment and hooks can be configured to hit that environment.
2. There are existing tools to create a valid url pointing to your localhost. We use [ngrok](https://ngrok.com/) internally for that, but there are other solutions.
3. You do the calls yourself with the following calls (with body as described above):

   curl -XPOST -H ‘Content-Type: application/json’ ‘[http://localhost:8080/your.hook.url](http://localhost:8080/your.hook.url)’ -d @body

# Examples

Examples for the most common use cases - payloads are at the end.


## Creating an order:

First create an address, and use the returned id as a shipping_address_id in the next call

curl -XPOST -H 'Content-Type: application/json' 'https://app.shippingbo.com/addresses' -H 'X-API-USER: __api@user__' -H 'X-API-TOKEN: __api_token__' -H 'X-API-VERSION: 1' -H "Content-Type: application/json" -d @address.json_

curl -XPOST -H 'Content-Type: application/json' 'https://app.shippingbo.com/orders' -H 'X-API-USER: __api@user__' -H 'X-API-TOKEN: __api_token__' -H 'X-API-VERSION: 1' -H "Content-Type: application/json" -d @order.json_


## Getting back tracking numbers:

Use notifications as described above


## Modifying an address:

Warning: this will obviously apply to any order linked to that address.

curl -XPATCH 'https://app.shippingbo.com/addresses/__address_id__ -H 'X-API-USER: __api@user__' -H 'X-API-TOKEN: __api_token__' -H 'X-API-VERSION: 1' -H "Content-Type: application/json" -d @address.modify.json


## Canceling an order:

Warning, when canceling an order after preparation started may not prevent the order to actually be shipped (even if the order state will stay canceled), depending on your preparation process.

*curl -XPATCH 'https://app.shippingbo.com/orders/__order_id__ -H 'X-API-USER: __api@user__' -H 'X-API-TOKEN: __api_token__' -H 'X-API-VERSION: 1' -H "Content-Type: application/json" -d @order.cancel.json*


## Creating an external invoice:

Note: the resources are not documented, but you can create an external invoice on an order to have your own invoices attached instead of the Shippingbo generated ones:

It is a 2 step process, first you upload the file in multipart/form-data, and then you attach it to the order. So the id from the first call is the _uploaded_file_id _ of the second body.

_curl -XPOST https://app.shippingbo.com/uploaded_files -F "uploaded_file[file]=@invoice.pdf" -H "X-API-USER: ***" -H 'X-API-VERSION: 1' -H "X-API-TOKEN: ***"_

_curl -XPOST https://app.shippingbo.com/order_documents -H "Content-Type: application/json" -d '{ "order_document": { "order_id": xxxxx, "uploaded_file_id": yyyyy, "language": "fr", "type": "OrderDocument::ExternalInvoice" }}' -H "X-API-USER: ***" -H 'X-API-VERSION: 1' -H "X-API-TOKEN: ***"_


## Creating an additional document specific at an order:

Note: the resources are not documented, but you can create an additional file on an order to have your own documents attached:

It is a 2 step process, first you upload the file in multipart/form-data, and then you attach it to the order. So the id from the first call is the _uploaded_file_id _ of the second body.

_curl -XPOST https://app.shippingbo.com/uploaded_files -F "uploaded_file[file]=@invoice.pdf" -H "X-API-USER: ***" -H 'X-API-VERSION: 1' -H "X-API-TOKEN: ***"_

_curl -XPOST https://app.shippingbo.com/order_documents -H "Content-Type: application/json" -d '{ "order_document": { "order_id": xxxxx, "uploaded_file_id": yyyyy, "language": "fr", "type": "OrderDocument::AdditionalFile", "document_type": "type_you_want" }}' -H "X-API-USER: ***" -H 'X-API-VERSION: 1' -H "X-API-TOKEN: ***"_


## Exemple payloads:

address.json:

```json
{
  "firstname": "Guillaume",
  "lastname": "Leseur",
  "street1": "1244 L'Occitane",
  "city":"Labege",
  "zip":"31670",
  "country":"FR",
  "phone1":"0102030405",
  "company_name":"Facilecomm"
}
```

order.json:

```json
{
  "source": "My company",
  "source_ref": "test-002",
  "state": "to_be_prepared",
  "shipping_address_id": 8940550,
  "origin": "Website",
  "origin_ref": "#001",
  "custom_state": "à envoyer",
  "origin_created_at": "2018-09-06T15:54:13+01:00",
  "chosen_delivery_service": "Colissimo - signature",
  "order_items_attributes": [
    {
    "source": "My company",
    "source_ref": "Item-1",
    "product_ref": "test sku",
    "title": "test product",
    "quantity": 1
    }
  ]
}
```


address.modify.json:

`{"street1": "42 rue du test"}`

order.cancel.json:

`{"state": "canceled"}`
