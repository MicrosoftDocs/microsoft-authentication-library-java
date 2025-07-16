---
title: Telemetry configuration
description: "You can register a callback to get telemetry from the authentication flow that you are conducting."
author: Dickson-Mwendia
manager: CelesteDG

ms.author: dmwendia
ms.date: 02/27/2024
ms.reviewer:
ms.service: msal
ms.subservice: msal-java
ms.topic: article
#Customer intent: 
---


# Telemetry configuration

You can register a callback to get telemetry from the authentication flow that you are conducting. To register a callback, you first define a method that receives the telemetry events. Telemetry events are sent back as `List<HashMap<String, String>>` .

```java
public class Telemetry {
    private static List<HashMap<String,String>> eventsReceived = new ArrayList<>();

    public static class MyTelemetryConsumer {

        Consumer<List<HashMap<String, String>>> telemetryConsumer =
                (List<HashMap<String, String>> telemetryEvents) -> {
                    eventsReceived.addAll(telemetryEvents);
                    System.out.println("Received " + telemetryEvents.size() + " events");
                    telemetryEvents.forEach(event -> {
                        System.out.print("Event Name: " + event.get("event_name"));
                        event.entrySet().forEach(entry -> System.out.println("   " + entry));
                    });
                };
     }
}
```

Then register your telemetry consumer with the client application:

```java
PublicClientApplication app = PublicClientApplication.builder(APP_ID)
        .authority(AUTHORITY)
        .telemetryConsumer(new MyTelemetryConsumer().telemetryConsumer)
        .build();
```
