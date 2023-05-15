As explained in [scenarios](Scenarios), there are many ways of acquiring a token. Some require user interactions through a web browser. Some don't require any user interactions.
In general the way to acquire a token is different depending on the application type-  public client application (Desktop / Mobile) or a confidential client application (Web App, Web API, daemon application like a windows service)

### Pre-requisites

Before acquiring tokens with MSAL4J:
- Instantiate a [client application](Client-Applications)

### Token acquisition methods

Follow the topics below for detailed explanation with MSAL4J code usage for each token acquisition method.

#### Public client applications:
- Acquire tokens [interactively with system browser](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Acquiring-Token-Interactively)
- Acquire tokens by [authorization code](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Acquiring-Tokens-with-Authorization-codes) after letting the user sign-in through the authorization request URL.
- It's also possible (but not recommended) to get a token with a [username and password](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=java#username--password).
- For applications running on Windows machines and joined to a domain or to Azure AD, it is possible to acquire a token silently, leveraging [Integrated Windows Authentication (IWA)](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=java#integrated-windows-authentication).
- Finally, for applications running on devices which don't have a web browser, it's possible to acquire a token through the [device code flow](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=java#command-line-tool-without-web-browser), which provides the user with a URL and a code. The user goes to a web browser on another device, enters the code and signs-in, and then Azure AD returns back a token to the browser-less device.

#### Confidential client applications:

- Acquire token **as the application itself** using [client credentials](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=java#acquiretokenforclient-api), and not for a user. For example, in apps which process users in batches and not a particular user such as in syncing tools.
- In the case of Web Apps or Web APIs **calling another downstream Web API in the name of the user**, use the [On Behalf Of flow](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki/Service-to-service-calls-on-behalf-of-the-user) to acquire a token based on some User assertion (SAML for instance, or a JWT token).
- **For Web apps in the name of a user**, acquire tokens by [authorization code](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java) after letting the user sign-in through the authorization request URL. This is typically the mechanism used by an application which lets the user sign-in using Open ID Connect, but then wants to access Web APIs for this particular user.

### MSAL4J caches tokens

For both Public client and Confidential client applications, MSAL maintains a token cache, and applications should try to get a token from the cache first before any other means (except in the case of [client credentials](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-daemon-acquire-token?tabs=java#acquiretokenforclient-api), which looks at the cache by itself). Take a look at the [recommended pattern](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-desktop-acquire-token?tabs=java) for token acquisition.

To be able to make use of the cache, the application needs to customize the [token cache serialization](https://docs.microsoft.com/en-us/azure/active-directory/develop/msal-java-token-cache-serialization).
