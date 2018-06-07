---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Java | Microsoft Docs
description: Aan de slag verzenden naar Event Hubs met behulp van Java
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 05/30/2018
ms.author: sethm
ms.openlocfilehash: 6d3bf0b8ac5c5bdc7bf3deda21e800fe3cc6be2e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626408"
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Gebeurtenissen verzenden naar Azure Event Hubs met Java

Event Hubs is een zeer schaalbaar systeem die kan worden miljoenen gebeurtenissen per seconde opnemen voor het inschakelen van een toepassing te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Zodra verzameld in een event hub, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslagcluster.

Zie voor meer informatie de [overzicht van Event Hubs][Event Hubs overview].

Deze zelfstudie laat zien hoe gebeurtenissen verzenden naar een event hub met behulp van een Java-consoletoepassing. Zie voor het ontvangen van gebeurtenissen met de bibliotheek Java Event Processor Host [in dit artikel](event-hubs-java-get-started-receive-eph.md), of klik op de juiste ontvangende taal in de tabel links van inhoud.

## <a name="prerequisites"></a>Vereisten

Om deze zelfstudie hebt voltooid, moet u de volgende vereisten:

* Een Java-ontwikkelomgeving. Deze zelfstudie wordt gebruikgemaakt van [Eclipse](https://www.eclipse.org/).
* Een actief Azure-account. Als u niet een Azure-abonnement hebt, maakt u een [gratis account][] voordat u begint.

De code in deze zelfstudie is gebaseerd op de [SimpleSend GitHub voorbeeld](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend), die u kunt bekijken om te zien van de volledige werkende toepassing.

## <a name="send-events-to-event-hubs"></a>Gebeurtenissen verzenden naar Event Hubs

De Java-clientbibliotheek voor Event Hubs is beschikbaar voor gebruik in Maven-projecten uit de [Maven centrale opslagplaats](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). U kunt verwijzen naar deze bibliotheek in het bestand Maven-project met behulp van de volgende afhankelijkheid-declaratie. De huidige versie is 1.0.0:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.0</version>
</dependency>
```

Voor verschillende soorten build-omgevingen, kunt u de meest recente uitgebrachte JAR-bestanden uit expliciet verkrijgen de [Maven centrale opslagplaats](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Voor de uitgever van een eenvoudige gebeurtenis en importeer de *com.microsoft.azure.eventhubs* -pakket voor de klassen van Event Hubs-client en de *com.microsoft.azure.servicebus* , zoals het pakket voor hulpprogramma klassen Algemene uitzonderingen die worden gedeeld met de Azure Service Bus messaging-client. 

### <a name="declare-the-send-class"></a>De klasse verzenden declareren

Maak voor het volgende voorbeeld eerst een nieuw Maven-project voor een console/shell-toepassing in uw favoriete Java-ontwikkelomgeving. Naam van de klasse `Send`:     

```java
package com.microsoft.azure.eventhubs.samples.send;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.PartitionSender;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

public class Send {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
```

### <a name="construct-connection-string"></a>Verbindingsreeks

De klasse ConnectionStringBuilder gebruiken om een string-waarde van de verbinding moet worden doorgegeven aan het exemplaar van de client Event Hubs samen te stellen. Vervang de tijdelijke aanduidingen door de waarden die u hebt verkregen tijdens het maken van de naamruimte en event hub:

```java
   final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
      .setNamespaceName("----NamespaceName-----")
      .setEventHubName("----EventHubName-----")
      .setSasKeyName("-----SharedAccessSignatureKeyName-----")
      .setSasKey("---SharedAccessSignatureKey----");
```

### <a name="send-events"></a>Gebeurtenissen verzenden

Een enkelvoudige gebeurtenis maken door het omzetten van een tekenreeks in de UTF-8-byte-codering. Vervolgens maakt u een nieuw exemplaar van de Event Hubs-client uit de verbindingsreeks en verzend het bericht:   

```java 
byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
EventData sendEvent = new EventData(payloadBytes);

final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);
ehClient.sendSync(sendEvent);
    
// close the client at the end of your program
ehClient.closeSync();

``` 

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [De eventprocessorhost gebeurtenissen ontvangen](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs-overzicht][Event Hubs overview]
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[Gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio

