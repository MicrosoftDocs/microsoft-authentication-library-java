---
title: Azure regions and regional endpoints
description: "To increase the reliability, availability and performance of applications running in Azure, multiple Azure regions are available that aim to keep all traffic for the application inside the same geographical area."
---

# Azure regions and regional endpoints

To increase the reliability, availability and performance of applications running in Azure, multiple Azure regions are available that aim to keep all traffic for the application inside the same geographical area. Relevant for MSAL are the regional endpoints for the secure token service, allowing an application using MSAL to retrieve tokens from a regional endpoint rather than the default global endpoint.

See [here](https://azure.microsoft.com/global-infrastructure/geographies/#geographies) for more information about specific Azure regions, and before trying to use a regional endpoint ensure that your authentication flow or scenario is supported by looking at the [regional token service FAQ](/identity/microsoft-identity-platform/ests-r-faq).

In order to make use of regional endpoints, MSAL Java allows developers to either set the region that the library should try to use for its token calls, or if the application is running on an Azure product (such as an Azure VM or Azure Functions) the library could attempt to detect the region automatically. If for some reason the region can't be detected or accessed during a token request, then the library will fallback to using the global endpoint and continue to use the global endpoint when refreshing that token.

To enable regional endpoints in MSAL Java, two values must be set when creating your client application object:

* Either set the [`azureRegion`](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/62927a1f32cfeceaba1afb1bdf982d05d6446823/src/main/java/com/microsoft/aad/msal4j/AbstractClientApplicationBase.java#L103) field to the short name for the Azure region your application is running in (such as `westus`), or set the [`autoDetectRegion`](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/62927a1f32cfeceaba1afb1bdf982d05d6446823/src/main/java/com/microsoft/aad/msal4j/AbstractClientApplicationBase.java#L99) field to `true`
  * If `azureRegion` is set then library will try to send requests to an endpoint using that specific region, whereas if `autoDetectRegion` is true then the library will try to detect the region for you based on some metadata that can be found in the Azure environment
* Set the [`sendx5c`](xref:com.microsoft.aad.msal4j.ConfidentialClientApplication.Builder.sendX5c(boolean)) field to `true`
  * This informs the service handling the auth request to send [SubjectName/Issuer (SNI)](https://github.com/AzureAD/microsoft-authentication-library-for-java/issues/219) information as part of the token request, which is required by the regional token service

For some more background information on regions, check out [this documentation](/identity/microsoft-identity-platform/msal-net-regional-adoption). Although that doc is for MSAL .NET, the underlying ideas behind regions and how to use them are the same for MSAL Java.
