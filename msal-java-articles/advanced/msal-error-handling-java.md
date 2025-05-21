---
title: Handle errors and exceptions in MSAL4J
description: Learn how to handle errors and exceptions, Conditional Access claims challenges, and retries in MSAL4J applications.
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Handle errors and exceptions in MSAL for Java

This article gives an overview of the different types of errors and recommendations for handling them.

## MSAL error handling basics

Exceptions in Microsoft Authentication Library (MSAL) are intended for app developers to troubleshoot, not for displaying to end users. Exception messages are not localized.

When processing exceptions and errors, you can use the exception type itself and the error code to distinguish between exceptions. For a list of error codes, see [Microsoft Entra authentication and authorization error codes](/entra/identity-platform/reference-error-codes).

During the sign-in experience, you may encounter errors about consents, Conditional Access (e.g., multi-factor authentication, device management, location-based restrictions), token issuance and redemption, and user properties.

The following section provides more details about error handling for your app.

## Error handling in MSAL for Java

In MSAL for Java, there are three types of exceptions: `MsalClientException`, `MsalServiceException`, and `MsalInteractionRequiredException`; all which inherit from `MsalException`.

- `MsalClientException` is thrown when an error occurs that is local to the library or device.
- `MsalServiceException` is thrown when the secure token service (STS) returns an error response or another networking error occurs.
- `MsalInteractionRequiredException` is thrown when UI interaction is required for authentication to succeed.

### MsalServiceException

`MsalServiceException` exposes HTTP headers returned in the requests to the STS. Access them via `MsalServiceException.headers()`

### MsalInteractionRequiredException

One of common status codes returned from MSAL for Java when calling `AcquireTokenSilently()` is `InvalidGrantError`. This means that additional user interaction is required before an authentication token can be issued. Your application should call the authentication library again, but in interactive mode by sending `AuthorizationCodeParameters` or `DeviceCodeParameters` for public client applications.

Most of the time when `AcquireTokenSilently` fails, it's because the token cache doesn't have a token matching your request. Access tokens expire in one hour, and `AcquireTokenSilently` will try to get a new one based on a refresh token. In OAuth2 terms, this is the Refresh Token flow. This flow can also fail for various reasons such as when a tenant admin configures more stringent login policies.

Some conditions that result in this error are easy for users to resolve. For example, they may need to accept Terms of Use or the request can't be fulfilled with the current configuration because the machine needs to connect to a specific corporate network.

MSAL exposes a `reason` field, which you can use to provide a better user experience. For example, the `reason` field may lead you to tell the user that their password expired or that they'll need to provide consent to use some resources. The supported values are part of the  `InteractionRequiredExceptionReason` enum:

| Reason | Meaning | Recommended Handling |
|---------|-----------|-----------------------------|
| `BasicAction` | Condition can be resolved by user interaction during the interactive authentication flow. | Call `acquireToken` with interactive parameters. |
| `AdditionalAction` | Condition can be resolved by additional remedial interaction with the system outside of the interactive authentication flow. | Call `acquireToken` with interactive parameters to show a message that explains the remedial action to take. The calling app may choose to hide flows that require additional action if the user is unlikely to complete the remedial action. |
| `MessageOnly` | Condition can't be resolved at this time. Launch interactive authentication flow to show a message explaining the condition. | Call `acquireToken` with interactive parameters to show a message that explains the condition. `acquireToken` will return the `UserCanceled` error after the user reads the message and closes the window. The app may choose to hide flows that result in message if the user is unlikely to benefit from the message. |
| `ConsentRequired`| User consent is missing, or has been revoked. |Call `acquireToken` with interactive parameters so that the user can give consent. |
| `UserPasswordExpired` | User's password has expired. | Call `acquireToken` with interactive parameter so the user can reset their password. |
| `None` |  Further details are provided. The condition may be resolved by user interaction during the interactive authentication flow. | Call `acquireToken` with interactive parameters. |

### Code Example

```java
IAuthenticationResult result;
try {
    PublicClientApplication application = PublicClientApplication
            .builder("clientId")
            .b2cAuthority("authority")
            .build();

    SilentParameters parameters = SilentParameters
            .builder(Collections.singleton("scope"))
            .build();

    result = application.acquireTokenSilently(parameters).join();
}
catch (Exception ex){
    if(ex instanceof MsalInteractionRequiredException){
        // AcquireToken by either AuthorizationCodeParameters or DeviceCodeParameters
    } else{
        // Log and handle exception accordingly
    }
}
```

## Conditional Access and claims challenges

When getting tokens silently, your application may receive errors when a [Conditional Access claims challenge](/entra/identity-platform/v2-conditional-access-dev-guide), such as MFA policy, is required by the API you're trying to access.

The pattern for handling this error is to interactively acquire a token using MSAL. This prompts the user and gives them the opportunity to satisfy the required Conditional Access policy.

In certain cases when calling an API requiring Conditional Access, you can receive a claims challenge in the error from the API. For instance if the Conditional Access policy is to have a managed device (Intune) the error will be something like [AADSTS53000: Your device is required to be managed to access this resource](/entra/identity-platform/reference-error-codes) or something similar. In this case, you can pass the claims in the acquire token call so that the user is prompted to satisfy the appropriate policy.

## Retrying after errors and exceptions

You're expected to implement your own retry policies when calling MSAL. MSAL makes HTTP calls to the Microsoft Entra service, and occasionally failures can occur. For example the network can go down or the server is overloaded.  

### HTTP 429

When the Service Token Server (STS) is overloaded with too many requests, it returns HTTP error 429 with a hint about how long until you can try again in the `Retry-After` response field.

## Next steps

Consider enabling [Logging in MSAL for Java](msal-logging-java.md) to help you diagnose and debug issues.
