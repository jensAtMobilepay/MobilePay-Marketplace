---
layout: default
---

## MobilePay API Reference

This page contains information about the MobilePay Marketplace API that Sellers can use to add and remove their products from the Marketplace.

### <a name="product"/> Product

#### <a name="add_product"/> Add Product

You can add a product to the Marketplace. After a new product has been added, the product will need to be manually added by a MobilePay Marketplace Admin before it will show up on the Marketplace in the MobilePay App.

```
POST api/v1/marketplace/seller/products
```

##### <a id="add-product-request" name="add_product_object"/> Input

|Parameter             |Sub Parameter |Type        |Description |
|----------------------|--------------|------------|------------|
|`sellersProductReference`       ||`string`| **Required.** Reference to the product in ther Seller's internal system. |
|`productConfiguration`       ||`object`| **Required.** Options for how the User can configure the Product before buying it. |
||`type`|`string`|**Required.** Type of product configuration. Either `StepAmount` for gift cards with a specific monetary value allowing User to control the value on the gift card, or `StepQuantity` to instead let the User purchase multiple of the fixed-value product. |
||`minimum`|`integer`|**Required.** The minimum amount that the monetary gift card can be on, or the minium quantity that the User can buy depending on `type`.|
||`maximum`|`integer` | **Required.** The maximum amount that the monetary gift card can be on, or the maximum quantity that the User can buy depending on `type`.|
||`default`|`integer`|**Required.** The default amount that the monetary gift card is set to for the User, or the default quantity depending on `type`.|
||`stepSize`|`integer`|**Required.** The step size of increments or decrements in User's chosen amount or quantity based on `type`.|
||`pricePerUnit`|`decimal`|**Required.** The price per product in local market currency. For `StepAmount` this should be set to `1`.|
||`vatPerUnit`|`decimal`|**Required.** The VAT per product in local market currency. For `StepAmount` this should be set to `0`.|
|`localizations`      ||`object[]`      |**Required.** List of product text localizations. Must contain required languages as stated under Market.|
||`language`|`string(5)`|**Required.** Language of the given localization. `DA_DK`, `EN_DK`, `FI_FI` or `EN_FI`.|
||`title`|`string`|**Required.** Title of the product. |
||`subtitle`|`string`|**Required.** Subtitle of the product. |
||`descriptionBuyer`|`string`|**Required.** Description of the product shown to the buyer. |
||`descriptionRecipient`|`string`|**Required.** Description of the product shown to the recipient of the product. |
||`merchantProductUrl`|`string`| Url to the product on the Seller's website. |
|`coverImages`||`object[]`|**Required.** List of images to be used as cover images for the product |
||`x2Url`|`string`|**Required.** Url to image in resolution: W: 720px H: 450px |
||`x3Url`|`string`|**Required.** Url to image in resolution: W: 1080px H: 675px |
||`x4Url`|`string`|**Required.** Url to image in resolution: W: 1440px H: 900px |
|`tileImage`||`object`|**Required.** Image to be used on the product tiles on the Marketplace |
||`x2Url`|`string`|**Required.** Url to image in resolution: W: 384px H: 256px |
||`x3Url`|`string`|**Required.** Url to image in resolution: W: 576px H: 384px |
||`x4Url`|`string`|**Required.** Url to image in resolution: W: 768px H: 512px |
|`productImageUrl`||`string`|**Required.** Image to be shown as small illustration of the product in resolution: W: 324px H: 219px|
|`validityFromPurchase`||`object`|**Required if** `validityFixedDate` **is not given.**  Period that the product is valid from purchase date. Only one of the values can be set, the rest should be 0.|
||`days`|`integer`|**Required.** Validty from purchase date in days.|
||`months`|`integer`|**Required.** Validty from purchase date in months. |
||`years`|`integer`|**Required.** Validty from purchase date in years. |
|`validityFixedDate`|| `date`| **Required if** `validityFromPurchase` **is not given.** A fixed date where the product will expire no matter what time the product was bought. |
| `campaignStartDate` || `date`| A specific date and time when the product is allowed to be sold. If not provided, the `Product` may be made available instantly. |
| `campaignEndDate` || `date`| A specific date and time when the product is no longer allowed to be sold. If not provided, the `Product` will continue to be available until manually removed. |

<div class="note">
  <strong>Note:</strong> Products cannot be updated at the moment. Instead the old product should be deleted and a new one created.
</div>

##### Example
Request
```json
{
 "sellersProductReference": "456879809+",
 "productConfiguration": {
 "type": "StepQuantity",
 "maximum": 10,
 "minimum": 1,
 "default": 1,
 "stepSize": 1,
 "pricePerUnit": 300.00,
 "vatPerUnit": 0.75
 },
 "localizations": [
    {
        "language": "DA_DK",
        "title": "Brunch menu for 2 på Edwin Rahrs Café",
        "subtitle": "Lækker og forførende brunchoplevelse i smukke omgivelser",
        "descriptionBuyer": "Læn dig tilbage og nyd vores lækre menu, der omfatter knasende sprødt brød og lækre juicer. Modtager af gavekortet kan frit vælge tidspunkt og valgfri marmelade. Derudover serverer vi:\n\t• Rundstykker\n\t• Croissanter\n\t• Nutellamader\n\t• Der er altid gratis kaffe på kanden.",
        "descriptionRecipient": "Tillykke med dit gavekort til Edwin Rahrs Café. Læn dig tilbage og nyd vores lækre menu, der omfatter knasende sprødt brød og lækre juicer. Du kan frit vælge tidspunkt og valgfri marmelade. Derudover serverer vi:\n\t• Rundstykker\n\t• Croissanter\n\t• Nutellamader\n\t• Der er altid gratis kaffe på kanden.",
        "merchantProductUrl": "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
    },
    {
        "language": "EN_DK",
        "title": "Brunch for 2 on Edwin Rahrs Café",
        "subtitle": "Delicious and tempting brunch experience in a beautiful environment",
        "descriptionBuyer": "Congrats med dit gavekort til Edwin Rahrs Café. Læn dig tilbage og nyd vores lækre menu, der omfatter knasende sprødt brød og lækre juicer. Du kan frit vælge tidspunkt og valgfri marmelade. Derudover serverer vi:\n\t• Rundstykker\n\t• Croissanter\n\t• Nutellamader\n\t• Der er altid gratis kaffe på kanden",
        "descriptionRecipient": "Congrats med dit gavekort til Edwin Rahrs Café. Læn dig tilbage og nyd vores lækre menu, der omfatter knasende sprødt brød og lækre juicer. Du kan frit vælge tidspunkt og valgfri marmelade. Derudover serverer vi:\n\t• Rundstykker\n\t• Croissanter\n\t• Nutellamader\n\t• Der er altid gratis kaffe på kanden",
        "merchantProductUrl": "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
    }
 ],
 "coverImages": [
    {
        "x2Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png",
        "x3Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png",
        "x4Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png"
    }
 ],
 "tileImage": {
    "x2Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png",
    "x3Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png",
    "x4Url": "https://imgshare.io/images/2020/08/13/popcorn-cover.png"
 },
 "productImageUrl": "https://imgshare.io/images/2020/08/13/popcorn-cover.png",
 "validityFromPurchase": {
    "days": 0,
    "months": 0,
    "years": 2
 }
}
```

Response

```
HTTP 200 OK
```
```json
{
    "productId" : "63679ab7-cc49-4f75-80a7-86217fc105ea"
}
```

```
HTTP 409 Conflict
```
```json
{
    "code": "232",
    "message": "Localizations must contain required translations"
}
```

#### Get Active Products

```
GET api/v1/marketplace/seller/products/active
```

##### <a id="get-order-response" name="get_order_object"/> Response

|Parameter             |Sub Parameter |Type        |Description |
|----------------------|--------------|------------|------------|
|`activeProducts`||`object[]`| **Required.** List of active products owned by seller. Empty if nothing is found. |
||`productId`|`guid`| **Required.**  Id of the product. Same value as returned after creating a new `Product`. <a href="api_reference#add_product">See endpoint</a>.|
||`sellersProductReference`|`string`| **Required.** Reference to the product in ther Seller's internal system. Same value given when `Product` was added. <a href="api_reference#add_product">See endpoint</a>. |


##### Example
Response

```
HTTP 200 OK
```
```json
{
  "activeProducts": [
    {
      "productId" : "63679ab7-cc49-4f75-80a7-86217fc105ea",
      "sellersProductReference": "MP/Matas/ABC-12345"
    }
  ]
}
```

#### Get Products

You can query a specific product that you have added to the Marketplace:

```
GET api/v1/marketplace/seller/products/{productId}
```

Or list all products owned by you:

```
GET api/v1/marketplace/seller/products
```

Response
```
HTTP 200 OK
```
<div class="note">
  <strong>Note:</strong> Response model is not fixed and is only meant for debugging and verification. If you would like to use this programatically, let your contact at MobilePay know.
</div>

#### Update Product

You can hide a product from the Marketplace. As Users can schedule purchases out in the future, expect that your disabled product will still be purchased and used after being disabled. Disabling it on the Marketplace simply means no new Users can find and purchase the product when browsing the Marketplace.

```
PATCH api/v1/marketplace/seller/products/{productId}
```

##### <a id="update-product-request" name="update_product_object"/> Input

|Parameter             |Type        |Description |
|-----------------|------------|------------|
|`disabled`       |`boolean`| Whether or not the product should be disabled and not be shown on the marketplace |

Furthermore, you can PATCH the same fields as for <strong>Create Product</strong>, except `productConfiguration`.

##### Example
Request

```json
{
    "disabled": "true"
}
```

Response

```
HTTP 200 OK
```


#### Preview product

For testing purposes the following endpoint can be called to instantly add a product to a dummy category. This will only work in Sandbox and not in Production.
This can be useful for testing how a new product will look visually in the MobilePay app. Use the details from the Verification in SandBox section.

```
POST api/v1/marketplace/seller/products/preview/{productId}
```
##### Example

Response

```
HTTP 200 OK
```

#### Delete Product

```
DELETE api/v1/marketplace/seller/products/{productId}
```
##### Example

Response

```
HTTP 200 OK
```
<div class="note">
  <strong>Note:</strong> This endpoint should only be used to clean up products when used for automated testing. The endpoint will <strong>not</strong> be available in the production environment.
</div>

### <a name="general"/> General

#### Market

The MobilePay Marketplace operates with different Markets, one for Denmark (`DK`) and one for Finland (`FI`). Products are not shared across Markets. All prices are in `DKK` for `DK` market and `EUR` for `FI` market.

#### Localizations
Some requests feature a `localizations` list. This list must contain translations in the following languages for their respective market as shown below.

* `DK` market
  * `DA_DK`: Translations in Danish language
  * `EN_DK`: Translations in English language
* `FI` market
  * `FI_FI`: Translations in Finnish language
  * `EN_FI`: Translations in English language

#### Currencies
Currencies are determined automatically by the Market. 
* `DK` Market uses `DKK`
* `FI` Market uses `EUR`

#### Types
* `date` contains both date and time in ISO 8601 format including the UTC offset to avoid any confusions regarding time zones - e.g. `2020-09-03T12:51:56+00:00`.
* `decimal` can have up to 2 decimals with `.` as seperator - e.g. `12.25`.

#### URLs
* All URLs, for images and other resources, must all be HTTPS
