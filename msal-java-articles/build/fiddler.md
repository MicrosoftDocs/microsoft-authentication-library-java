---
title: Using Fiddler with MSAL Java
description: "You can use the MSAL4J with a proxy such as Fiddler to debug requests and responses."
author: Dickson-Mwendia
manager: CelesteDG
ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer: 
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
#Customer intent: 
---

# Using Fiddler with MSAL Java

You can use the MSAL4J with a proxy such as [Fiddler](https://www.telerik.com/fiddler) to debug requests and responses.

## 1. Set up a keystore file

- From Fiddler, export FiddlerRoot.cer to desktop via Tools -> Options -> HTTPS -> Actions button
- Open Command Prompt as an admin
- Run JDK's keytool to create a keystore

`<JDK_HOME>\bin\keytool.exe -import -file C:\Users\<username>\Desktop\FiddlerRoot.cer^
  -keystore FiddlerKeystore -alias Fiddler`
- Enter a password for your keystore. Make sure to remember this as you will need it later on.

## 2a. Set up IntelliJ

- In IntelliJ, open Run/Debug configurations
- Create a new debug configuration called “Fiddler Trace”
- Add the following VM Options:

`-DproxySet=true
 -DproxyHost=127.0.0.1
-DproxyPort=8888
-Djavax.net.ssl.trustStorePassword="yourpassword"
-Djavax.net.ssl.trustStore="path\to\keystore\FiddlerKeystore"`

### 2b. Set up Eclipse

- In Eclipse, open Run -> Run Configurations
- Select the Run Configuration you want to use
- Select the Arguments tab
- Add the following arguments:

`-DproxySet=true
-DproxyHost=127.0.0.1
-DproxyPort=8888
-Djavax.net.ssl.trustStore="path\to\keystore\FiddlerKeystore"
-Djavax.net.ssl.trustStorePassword="yourpassword"`
