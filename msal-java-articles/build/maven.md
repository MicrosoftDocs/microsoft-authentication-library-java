---
title: Building with Maven
description: "To be able to build with maven, you need a working installation of Java and Maven."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 01/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: conceptual
---

# Building with Maven

To be able to build with maven, you need a working installation of [Java](https://www.oracle.com/technetwork/java/javase/downloads/index.html) and [Maven](https://maven.apache.org/download.cgi).

Once you have successfully installed Java and Maven, clone the microsoft-authentication-library-for-java repository.

From you shell or command line:

- `$ git clone https://github.com/AzureAD/microsoft-authentication-library-for-java.git`
- `$ cd microsoft-authentication-library-for-java`

Then run:

- `mvn clean`
- `mvn package`

You should now have a "target" directory with `msal4j-x.x.x.jar`.

To install, run:

- `mvn install -DskipITs`

## Run tests

To run the test suite of the project, run the command:

- `mvn test`
