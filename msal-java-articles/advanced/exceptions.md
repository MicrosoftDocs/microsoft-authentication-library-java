---
title: Exceptions in MSAL Java
description: "When processing exceptions, you can use the exception type itself and the ErrorCode member to distinguish between exceptions."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer: dayodeji
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Exceptions in MSAL Java

When processing exceptions, you can use the exception type itself and the ErrorCode member to distinguish between exceptions. There are three type of exceptions: `MsalClientException`,  `MsalServiceException`, and `MsalInteractionRequiredException`, all which inherit from `MsalException`.

- **MsalClientException** is thrown when an error occurs that is local to the library or device.
- **MsalServiceException** is thrown when the STS service returns an error response or another networking error occurs.
- **MsalInteractionRequiredException** is thrown when UI interaction is required for authentication to succeed.

## MsalServiceException

MsalServiceException exposes HTTP headers returned the requests to the STS. You can access them by `MsalServiceException.headers()`

## MsalInteractionRequiredException

One of common status codes returned from MSAL4J when calling `AcquireTokenSilently()` is `InvalidGrantError`. This status code means that the application should call the authentication library again, but in interactive mode (Using AuthorizationCodeParameters or DeviceCodeParameters for public client applications). This is because additional user interaction is required before an authentication token can be issued.

Most of the time when AcquireTokenSilently fails, it is because the token cache doesn't have tokens matching your request. Access tokens expire in 1 hour, and AcquireTokenSilently will try to fetch a new one based on a refresh token (in OAuth2 terms, this is the "Refresh Token' flow). This flow can also fail for various reasons, for example if a tenant admin configures more stringent login policies.

The interaction aims at having the user do an action. Some of those conditions are easy for users to resolve (for example, accept Terms of Use with a single click), and some can't be resolved with the current configuration (for example, the machine in question needs to connect to a specific corporate network). 

MSAL exposes a `reason` field, which you can read to provide a better user experience, for example to tell the user that their password expired or that they'll need to provide consent to use some resources. The supported values are part of the  InteractionRequiredExceptionReason enum:

|Reason|Meaning|Recommended Handling|
|------|-------|--------------------|
|BasicAction| Condition can be resolved by user interaction during the interactive authentication flow| Call acquireToken with interactive parameters|
|AdditionalAction|Condition can be resolved by additional remedial interaction with the system, outside of the interactive authentication flow.| Call acquireToken with interactive parameters to show a message that explains the remedial action. Calling application may choose to hide flows that require additional_action if the user is unlikely to complete the remedial action.|
|MessageOnly|Condition can't be resolved at this time. Launching interactive authentication flow will show a message explaining the condition.| Call acquireToken with interactive parameters to show a message that explains the condition. acquireTokenCall will return UserCanceled error after the user reads the message and closes the window. Calling application may choose to hide flows that result in message_only if the user is unlikely to benefit from the message.|
|ConsentRequired|User consent is missing, or has been revoked.|Call all acquireToken with interactive parameters for user to give consent.|
|UserPasswordExpired|User's password has expired.|Call acquireToken with interactive parameter so user can reset password|
|ConsentRequired|User consent is missing, or has been revoked| Call acquireToken with interactive parameters so user can reset password|
|None| 	No further details are provided. Condition may be resolved by user interaction during the interactive authentication flow. | Call acquireToken with interactive parameters|

## Code Example

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
