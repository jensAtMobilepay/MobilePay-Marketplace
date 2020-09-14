---
layout: default
---

## Seller API Reference

This page contains information about the API that Sellers must provide which will be called by MobilePay services to register new Orders and to query details about purchased products.

### <a name="order"/> Order

#### Create Order

When a `Product` is delivered to a User, MobilePay will call the below endpoint to let you register the `Order` on your end. 

```
POST seller-base-url/orders
```

##### <a id="create-order-request" name="create_order_object"/> Input

|Parameter             |Sub Parameter |Type        |Description |
|----------------------|--------------|------------|------------|
|`orderId`||`guid`| **Required.** Id representing the order. You must be idempotent on this value. If you receive multiple requests with the same `orderId` it should only end up with 1 `Order` being created and the responses to the two requests should be identical. |
|`productId`||`guid`| **Required.** Id of the product that has been purchased. Same value as returned after creating a new `Product`. <a href="api_reference#add_product">See endpoint</a>. |
|`productReference`||`string`| **Required.**  Seller's internal reference of the product that has been purchased. Same value as provided when creating a new `Product`. <a href="api_reference#add_product">See endpoint</a>. |
|`productConfigurationValue`||`integer`| **Required.** The configured value of the product. If product configuration type is `StepAmount` then this value is the monetary value of the gift card, otherwise if it is `StepQuantity` then this is the quantity of the product that the User selected. |

##### Example
Request
```json
{
  "orderId": "fc6b8155-624a-4d10-9aad-c1df10809913",
  "productId": "8f845040-c851-b2b2-2cdd-2cacb12006d3",
  "productReference": "Uy9C9PB9kqU8M2tep5MAA",
  "productConfigurationValue": 150
}
```

Response

```
HTTP 200 OK
```
```json
{
  "orderReference": "UxKD92jdNDjapqld",
  "invoiceNumber": 12345
}
```

#### Get Order

When an `Order` has been created by MobilePay calling your endpoint as described above, MobilePay will at some point query the created resource for getting details about the purchased product to be shown to the user, e.g. bar codes. 
```
GET seller-base-url/orders/{orderReference}
```

##### <a id="get-order-response" name="get_order_object"/> Response

|Parameter             |Sub Parameter |Sub Parameter  |Type        |Description |
|----------------------|--------------|---------------|------------|------------|
|`expiryDate`|||`date`| **Required.** The exact date and time when the product can no longer be used |
|`companyLogoUrl`|||`string`| **Required.** Logo portraying the company who is providing the product. W: 60px H: 292px. Should have transparent background. File type should be png. |
|`coupons`|||`object[]`| **Required.** List of coupons that the User should received upon purchase of the product. |
||`reference`||`string`| **Required.** A unique id representing this coupon chosen by `Seller`. |
||`copyToClipboardValue`||`string`| A string that will be copied to the Users clipboard upon tapping a button. Ideal for codes that need to be entered on websites or apps from the User's phone. |
||`localizations`||`object[]`| **Required.** Translations of texts displayed on each coupon. |
|||`language`|`string`| **Required** Language of the given localization. `DA_DK`, `EN_DK`, `FI_FI` or `EN_FI`. | 
|||`title`|`string`| **Required** Title of the coupon. |
|||`subtitle`|`string`| **Required** Subtitle of the coupon. |
|||`elementImageUrl`|`string`| Url to an image of the bar code used to redeem the coupon. If `elementHeaderText` is set, then this must be `null`. |
|||`elementHeaderText`|`string`| Header text used to describe the type of redeem code, e.g. 'Rabatkode'. If `elementImageUrl`is set, then this must be `null`. |
|||`elementText1`|`string`| Line 1 describing the redeem code, e.g. '91823175'. |
|||`elementText2`|`string`| Line 2 describing the redeem code, e.g. 'Pin: 3842'. |
|||`howToUseTitle`|`string`| Title for the how to use section. |
|||`howToUseExplanation`|`string`| Explanation for how to use the product, displayed in the how to use section. |

##### Example
Response

```
HTTP 200 OK
```
```json
{
  "expiryDate": "2020-09-03T12:51:56+00:00",
  "companyLogoUrl": "https://gogift.dk/matas.png",
  "coupons": [
    {
      "reference": "sgze24905962",
      "copyToClipboardValue": "23230923",
      "localizations": [
        {
          "language": "DA_DK",
          "title": "Gavekort",
          "subtitle": "50 kr. til Matas",
          "elementImageUrl": "https://gogift.dk/barcodes/23912309",
          "elementHeaderText": null,
          "elementText1": "23230923",
          "elementText2": "Pin-kode: 3232", 
          "howToUseTitle": "Sådan bruger du dit gavekort",
          "howToUseExplanation": "Få ekspedienten til at scanne dit gavekort ved kassen.",
        },
        {
          "language": "EN_DK",
          "title": "Gift Card",
          "subtitle": "50 kr. for Matas",
          "elementImageUrl": "https://gogift.dk/barcodes/23912309",
          "elementHeaderText": null,
          "elementText1": "23230923",
          "elementText2": "Pin-code: 3232", 
          "howToUseTitle": "How to use your gift card",
          "howToUseExplanation": "Make the cashier scan your gift card by the register.",
        },
      ]     
    }
  ]
}
```

#### Get Order Status

A desired feature of the GiftCards sold on the MobilePay Marketplace is that the User can see the remaining balance for monetary gift cards, and also that gift cards are automatically marked as used. As the gift cards users buy can each contain multiple coupons, we need a status per coupon.
This endpoint returns a list of status for each of the coupons for each of the provided `Orders`.

To avoid issues with maximum url lengths, we use `POST` for this endpoint, and keep the list of `orderReferences` in the request body, even though it behaves exactly like a `GET`. 

```
POST seller-base-url/orders/status
```

##### <a id="get-coupon-status-response" name="get_coupon_status_object"/> Response

|Parameter             |Sub Parameter |Sub Parameter  |Type        |Description |
|----------------------|--------------|---------------|------------|------------|
|`orders`|||`object[]`|**Required.** List of orders. |
||`orderReference`||`string`|**Required.** Reference to the `Order` as provided by `Seller`. |
||`couponStatus`||`object[]`| **Required.** List of status for the coupons. |
|||`reference`|`string`|**Required.** The unique id of the `Coupon` which the status relates to. |
|||`completelyUsed`|`boolean`|**Required.** Should be `true` if the User has fully used the `Coupon` and it no longer holds any value. Otherwise this should be `false`. |
|||`remainingAmount`|`decimal`| Should contain the amount left on the `Coupon` if it has a monetary value. |

##### Example
Request

```json
{
  "orderReferences": ["jf23dDLdo204mcxpaoe24948djO", "Rxdp24mfcma84jdalwp0294dwld"]
}
```

Response

```
HTTP 200 OK
```
```json
{
  "orders": [
    {
      "reference": "Rxdp24mfcma84jdalwp0294dwld",
      "couponStatus": [
        {
          "reference": "kdgb242352352",
          "completelyUsed": false,
          "remainingAmount": 230.56
        },
        {
          "reference": "mvoe235288964",
          "completelyUsed": true,
          "remainingAmount": 0
        }
      ]
    },
    {
      "reference": "jf23dDLdo204mcxpaoe24948djO",
      "couponStatus": [
        {
          "reference": "sgze24905962",
          "completelyUsed": true
        },
        {
          "reference": "pepfa0093986",
          "completelyUsed": true
        }
      ]
    },
  ]
}
```