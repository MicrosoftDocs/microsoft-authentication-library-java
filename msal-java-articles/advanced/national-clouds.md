National clouds (aka Sovereign clouds) are physically isolated instances of Azure. These regions of Azure are designed to make sure that data residency, sovereignty, and compliance requirements are honored within geographical boundaries.

In addition to the public cloud​, Azure Active Directory is deployed in the following National clouds:  

- Azure US Government
- Azure China 21Vianet
- Azure Germany

Note that enabling your application for sovereign clouds requires you to:

- Register your application in a specific portal, depending on the cloud
- Use a specific authority, depending on the cloud in the config file for your application
- In case you want to call the graph, this requires a specific Graph endpoint URL, depending on the cloud.

More details in [Authentication in National Clouds](https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-national-cloud)

For convenience, the list of authentication endpoints for the national clouds can be found in the [AzureCloudEndpoint](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/main/java/com/microsoft/aad/msal4j/AzureCloudEndpoint.java) enum, along with the endpoint for the global/public cloud.