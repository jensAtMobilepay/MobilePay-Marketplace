---
layout: default
---

# Overview
This document explains how to make a technical integration to the **MobilePay Marketplace** product as a Seller on the Marketplace.

## <a name="marketplaceseller_onboarding"></a>**Part 1: Onboarding a Marketplace Seller**

1. **Log-in on the developer portal.** Go to [Sandbox developer portal](https://sandbox-developer.mobilepay.dk/) and log in with your credentials.

2. **Create an app in the developer portal.** Go to My Apps > Create new App to register a new application. You need to supply the `x-ibm-client-id` and `x-ibm-client-secret` when calling APIs. You should always store the two values in a secure location, and never reveal them publicly. More details about the usage of values below in the authentication section. You will need to create multiple apps per Market. For example one app to be used for adding products to the `DK` market and another app to be used for adding products to the `FI` market.

3. **Provide your info to MobilePay.** Hand over your `x-ibm-client-id` to your contact at MobilePay through secure communication. This will be used to associate your endpoint calls to your organization. Additionally, please hand over `Company name`, `Business registration number`, `Address`, `Puchase terms url` for each Market.

4. **Subscribe the app to APIs.**  Go to [APIs](https://sandbox-developer.mobilepay.dk/product) and subscribe to the following API:
-  Marketplace Seller

5. **Read API documentation.** 

## <a name="authenticationapi"></a> Part 2: Authentication of API requests 
The MobilePay API Gateway is ensuring the authentication of all MobilePay Marketplace API requests. All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

To be able to use and connect to the API there are a few requirements. In order to authenticate to the API, all requests to the API must contain the following two headers containing the values from above:
1. `x-ibm-client-id`
2. `x-ibm-client-secret`
