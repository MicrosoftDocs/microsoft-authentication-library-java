---
title: Using the acquired token to call a protected API
description: "How to use a token acquired with MSAL4J to call a secure API."
---

## Using the token

Getting a token is not a goal per se. It's a necessary step to call a protected API. The token needs to be used to access a Web API. The way to do it is by setting the Authorization header to be "Bearer", followed by a space, followed by the access token.

## Using the access token to call a protected Web API

Note that the code below shows how to call directly the web API with an HttpURLConnection. You can also use libraries which will only require the access token (DocumentDb for instance) and will take care of the headers details. In practice the code might change depending on the libraries you want to use.

```java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
 ```
