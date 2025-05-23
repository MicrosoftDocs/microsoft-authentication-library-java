---
title: Client credentials in MSAL Java
description: "MSAL Java supports two types of client credentials - application secrets and certificates."
#Customer intent: 
---

# Client credentials in MSAL Java

There are three types of client secrets in MSAL4J:

- Application Secrets
- Certificates
- Client assertions

## Client Credentials with application secret in MSAL4J

During the registration of a the confidential client application with Microsoft Entra ID, a client secret is generated (a kind of application password). When the client wants to acquire a token in its own name it will:

- Create `IClientCredential` using the `ClientCredentialFactory`, passing in the client secret, which should be a string.

```java
String CLIENT_SECRET; 
IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET)
```

## Client Credentials with certificate

In this case, when the application is registered with Microsoft Entra ID, it uploads the public key of a certificate. When it wants to acquire a token, the client application will

- Create `IClientCredential` using the `ClientCredentialFactory`, passing in either both the public and private keys, or a InputStream of the pkcs12

```java
PrivateKey privateKey;  
X509Certificate publicKey;  
IClientCredential credential = ClientCredentialFactory.createFromCertificate(privateKey, publicKey)
```

or

```java
InputStream inputStream;  
String password;  
IClientCredential credential = ClientCredentialFactory.create(inputStream, password)
```

- You would then create a confidential client application and pass in the client credential. 

```java
ConfidentialClientApplication app =
                    ConfidentialClientApplication.builder(
                        CLIENT_ID,
                        credential)
                .build();
```
