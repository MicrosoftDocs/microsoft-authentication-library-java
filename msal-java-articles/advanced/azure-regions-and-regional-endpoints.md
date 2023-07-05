---
title: Azure regions and regional endpoints
description: "To increase the reliability, availability and performance of applications running in Azure, multiple Azure regions are available that aim to keep all traffic for the application inside the same geographical area."
---

# Azure regions and regional endpoints

To increase the reliability, availability, and performance of applications running in Azure, multiple Azure regions are available that aim to keep all traffic for the application inside the same geographical area. Relevant for MSAL are the regional endpoints for the secure token service, allowing an application using MSAL to retrieve tokens from a regional endpoint rather than the default global endpoint.

See [Azure geographies](https://azure.microsoft.com/global-infrastructure/geographies/#geographies) for more information about specific Azure regions.

In order to make use of regional endpoints, MSAL Java allows developers to either set the region that the library should try to use for its token calls, or if the application is running on an Azure product (such as an Azure VM or Azure Functions) the library could attempt to detect the region automatically. If for some reason the region can't be detected or accessed during a token request, then the library will fallback to using the global endpoint and continue to use the global endpoint when refreshing that token.

To enable regional endpoints in MSAL Java, two values must be set when creating your client application object:

* Either set the [`azureRegion`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.Builder.azureRegion(java.lang.String)) field to the short name for the Azure region your application is running in (such as `westus`), or set the [`autoDetectRegion`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.Builder.autoDetectRegion(boolean)) field to `true`
  * If [`azureRegion`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.Builder.azureRegion(java.lang.String)) is set then library will try to send requests to an endpoint using that specific region, whereas if [`autoDetectRegion`](xref:com.microsoft.aad.msal4j.AbstractClientApplicationBase.Builder.autoDetectRegion(boolean)) is true then the library will try to detect the region for you based on some metadata that can be found in the Azure environment
* Set the [`sendx5c`](xref:com.microsoft.aad.msal4j.ConfidentialClientApplication.Builder.sendX5c(boolean)) field to `true`
  * This informs the service handling the auth request to send [SubjectName/Issuer (SNI)](https://github.com/AzureAD/microsoft-authentication-library-for-java/issues/219) information as part of the token request, which is required by the regional token service.
