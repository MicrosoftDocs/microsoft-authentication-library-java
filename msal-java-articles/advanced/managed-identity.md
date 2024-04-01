---
title: Managed identity with MSAL Java
description: "How to use Azure managed identities in MSAL Java applications."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Managed identity with MSAL Java

>[!NOTE]
>This feature is available starting with [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases/tag/v1.15.0) version 1.15.0

A common challenge for developers is the management of secrets, credentials, certificates, and keys used to secure communication between services. [Managed identities](/entra/identity/managed-identities-azure-resources/overview) in Azure eliminate the need for developers to handle these credentials manually. MSAL Java supports acquiring tokens through the managed identity service when used with applications running inside Azure infrastructure, such as:

* [Azure App Service](https://azure.microsoft.com/products/app-service/) (API version `2019-08-01` and above)
* [Azure VMs](https://azure.microsoft.com/free/virtual-machines/)
* [Azure Arc](/azure/azure-arc/overview)
* [Azure Cloud Shell](/azure/cloud-shell/overview)
* [Azure Service Fabric](/azure/service-fabric/service-fabric-overview)

For a complete list, refer to [Azure services that can use managed identities to access other services](/entra/identity/managed-identities-azure-resources/managed-identities-status).

## Which SDK to use - Azure SDK or MSAL?

MSAL libraries provide lower level APIs that are closer to the OAuth2 and OIDC protocols. 

Both MSAL Java and [Azure SDK](/azure/developer/java/sdk/overview) allow acquiring tokens via Managed Identity. Internally, Azure SDK uses MSAL Java. It provides a higher-level API via its <xref:com.azure.identity.DefaultAzureCredential> and <xref:com.azure.identity.ManagedIdentityCredential> abstractions. 

If your application already uses one of the SDKs, continue using the same SDK. Use Azure SDK, if you are writing a new application and plan to call other Azure resources, as this SDK provides a better developer experience by allowing the app to run on private developer machines where managed identity doesn't exist. Consider using MSAL if you need to call other downstream web APIs like Microsoft Graph or your own web API. 

## Quick start

To quickly get started and see Azure Managed Identity in action, you can use one of the samples the team built for this purpose:

> [!div class="nextstepaction"]
> [Use Managed Identity sample](https://github.com/Azure-Samples/msal-managed-identity/tree/main/src/java)

## How to use managed identities

There are two types of managed identities available to developers - **system-assigned** and **user-assigned**. You can learn more about the differences in the [Managed identity types](/entra/identity/managed-identities-azure-resources/overview#managed-identity-types) article. MSAL Java supports acquiring tokens with both. [MSAL Java logging](msal-logging-java.md) allows to keep track of requests and related metadata.

Prior to using managed identities from MSAL Java, developers must enable them for the resources they want to use through Azure CLI or the Azure Portal.

## Examples

For both user-assigned and system-assigned identities, developers can use the <xref:com.microsoft.aad.msal4j.ManagedIdentityApplication.Builder> class. 

### System-assigned managed identities

For system-assigned managed identities, the developer does not need to pass any additional information when creating an instance of <xref:com.microsoft.aad.msal4j.IManagedIdentityApplication>, as it will automatically infer the relevant metadata about the assigned identity.

<xref:com.microsoft.aad.msal4j.IManagedIdentityApplication.acquireTokenForManagedIdentity(com.microsoft.aad.msal4j.ManagedIdentityParameters)> is called with the resource to acquire a token for, such as `https://management.azure.com`.

```java
ManagedIdentityApplication miApp = ManagedIdentityApplication
                .builder(ManagedIdentityId.systemAssigned())
                .build();
                
ManagedIdentityParameters parameters = ManagedIdentityParameters.builder(resource).build();
                
IAuthenticationResult result = miApp.acquireTokenForManagedIdentity(parameters).get();
    
```

### User-assigned managed identities

For user-assigned managed identities, the developer needs to pass either the client ID, full resource identifier, or the object ID of the managed identity when creating <xref:com.microsoft.aad.msal4j.IManagedIdentityApplication>.

Like in the case for system-assigned managed identities, <xref:com.microsoft.aad.msal4j.IManagedIdentityApplication.acquireTokenForManagedIdentity(com.microsoft.aad.msal4j.ManagedIdentityParameters)> is called with the resource to acquire a token for, such as `https://management.azure.com`.

```java
ManagedIdentityApplication miApp = ManagedIdentityApplication
                .builder(ManagedIdentityId.userAssignedClientId(CLIENT_ID))
                .build();
                
 ManagedIdentityParameters parameters = ManagedIdentityParameters.builder(resource).build();
               
 IAuthenticationResult result = miApp.acquireTokenForManagedIdentity(
                ManagedIdentityParameters.builder(resource)
                        .build()).get();
```

## Caching

By default, MSAL Java supports in-memory caching. MSAL does not support cache extensibility for managed identity because of security concerns when using distributed cache. Since a token acquired for managed identity belongs to an Azure resource, using a distributed cache might expose it to the other Azure resources sharing the cache.

## Troubleshooting

For failed requests the error response contains a correlation ID that can be used for further diagnostics and log analysis. Keep in mind that the correlation IDs generated in MSAL Java or passed into MSAL are different than the one returned in server error responses, as MSAL Java cannot pass the correlation ID to managed identity token acquisition endpoints.

### Potential errors

#### `MsalServiceException` Error Code: `managed_identity_failed_response` Error Message: An unexpected error occurred while fetching the Microsoft Entra token

This exception might mean that the resource you are trying to acquire a token for is either not supported or is provided using the wrong resource ID format. Examples of correct resource ID formats include `https://management.azure.com/.default`, `https://management.azure.com`, and `https://graph.microsoft.com`.

#### `MsalServiceException` Error Code: `managed_identity_unreachable_network`.

This exception might mean that you are likely using a resource where MSAL Java does not support acquiring token for managed identity or you are running the sample code from a development machine where the endpoint to acquire the token for managed identities is unreachable.
