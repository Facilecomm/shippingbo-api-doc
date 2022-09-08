---
stoplight-id: 6fkoxdhf5024p
tags: [webhook]
---

# How to configure ?

### Why you must implement Webhooks with Shippingbo ?

Webhooks are actively recommended to be used istead of polling, there is no case that justifying a massive polling. Shippingbo is able to call your server when you need the information like Order state changes, product stock changes, etc...

### Which ressources are webhookable ?

> Please ask to the Support team to have access to the full documentation API

<!-- theme: warning -->

> Please note the payload serialization will be fixed and cannot be personnalised

- AddressLabelSummary
- LabelproviderClientReference
- Order
- OrderEvent
- Product
- ProductBarcode
- ProductStockVariation
- Report
- ReturnOrder
- Shipment
- SlotContent
- SlotStockVariation
- SupplyCapsule
- TrackingInfo

### How to configure a Webhook ?
### Basics

1. First you need to create the Webhook, go to the [Webhook configuration page](https://app.shippingbo.com/#/updateHooks), then click on **Add** button

2. Select the **Object class**, this is the ressource you want to observe. For exemple you can select **Order** if you want to be notified every time an **Order** is created or updated
3. Fill ine the **URL** field and that's it !

Your webhook is already functionnal, the URL will be called every the selected ressource is created or updated

### Trigger webhook only on creation

To be notified every time the ressource is created and ignore updates you must select **id**

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/np9Q5SbHyYI)

### Trigger webhook only on update

To be notified every time the ressource is updated you must select the field you want, for exemple you want to be notified every time an **Order** is shipped you can configure it like this:

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/ELyedqU5BIw)


### Security parameters

##### By authenticationn (most secure option)

How to setup webhook authentication ?

1. Go to the Webhook configuration page for the webhook you want to configure 

2. You must select **by authentication** in the authentication scheme

3. Enter the URL to call to authenticate before calling the endpoint

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/Zfugp1mYdkU)


```curl
curl --request POST \
  --url https://exemple.shippingbo.com/authentication \
  --header 'Content-Type: application/json' \
  --header 'authorization: "Basic my_access_token"'
```

4. The response must be like:

```json
{
  "token_type": "Basic",
  "access_token": "my_freshly_renewed_token"
}
```

5. Then in the webhook call you will find 2 headers:

    - "Authorization: Basic my_freshly_renewed_token" 
    - "x-api-key: my_api_key"

    You must check both of them ton make sure Shippingbo is rightly authenticated and you can integrate the information safely


#### With free header

The "Free header" method is most easier to implement.

How to setup free headers ?

1. You must select Free header
2. Fill in the field with your custom headers in JSON format

![image.png](https://stoplight.io/api/v1/projects/cHJqOjE0ODExOA/images/XSmNuNqhsj4)

3. And we will integrate the custom header to each request


### Debug a webhook

In the webhook page you will find a button **Logs**, this button will redirect to the Shippingbo debug application. One this app you can see every calls Shippingbo has made to your endpoint, you can check headers, status code, body and more

This application is very usefull to debug your implementation and we encourage each developer to use this application before ask to the Support team or your Shippingbo contact. If the error seems to come from Shippingbo please contact us with the link to your webhook logs, without it Shippingbo will redirect you to the Logs page