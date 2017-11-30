---
title: Azure IoT Hub aangepaste eindpunten begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - routeringsregels gebruiken voor het routeren van apparaat-naar-cloud-berichten met aangepaste eindpunten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2017
ms.author: dobett
ms.openlocfilehash: d1e22a4378caf69d2077d79f78682c4d438dbcd2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Berichtroutes en aangepaste eindpunten gebruikt voor apparaat-naar-cloud-berichten

IoT-Hub kunt u voor het routeren van [apparaat-naar-cloudberichten] [ lnk-device-to-cloud] naar IoT Hub gerichte service-eindpunten op basis van eigenschappen van berichten. Routeringsregels bieden u de flexibiliteit om berichten te verzenden wanneer ze nodig hebben om te gaan zonder dat hiervoor extra services nodig om berichten te verwerken of om aanvullende code te schrijven. Elke regel voor het doorsturen die u configureert heeft de volgende eigenschappen:

| Eigenschap      | Beschrijving |
| ------------- | ----------- |
| **Naam**      | De unieke naam die de regel identificeert. |
| **Bron**    | De oorsprong van de gegevensstroom worden gereageerd. Bijvoorbeeld: apparaattelemetrie. |
| **Voorwaarde** | De queryexpressie voor de routing-regel die is uitgevoerd voor de kopteksten en de hoofdtekst van het bericht en gebruikt om te bepalen of er een overeenkomst voor het eindpunt. Zie voor meer informatie over het maken van een route voorwaarde de [referentie - querytaal voor apparaat horende en taken][lnk-devguide-query-language]. |
| **Eindpunt**  | De naam van het eindpunt waar IoT Hub verzendt berichten die overeenkomen met de voorwaarde. Eindpunten moet in dezelfde regio bevinden als de IoT-hub, anders u mogelijk in rekening gebracht voor de regio-overschrijdende schrijfbewerkingen. |

Een enkel bericht mogelijk overeenkomt met de voorwaarde op meerdere regels voor het doorsturen, geval IoT Hub waarin het bericht naar het eindpunt dat is gekoppeld aan elke overeenkomende regel biedt. IoT Hub deduplicates ook automatisch levering van berichten, dus als een bericht overeenkomt met meerdere regels die hetzelfde doel hebben, wordt alleen geschreven naar deze bestemming eenmaal.

Een IoT-hub is een standaard [ingebouwd eindpunt][lnk-built-in]. U kunt aangepaste eindpunten om berichten te routeren naar maken door andere services in uw abonnement koppelen aan de hub. IoT Hub ondersteunt momenteel Azure Storage-containers, Event Hubs, Service Bus-wachtrijen en Service Bus-onderwerpen als aangepaste eindpunten.

Wanneer u Routering en aangepaste eindpunten, worden berichten alleen geleverd met het ingebouwde eindpunt als ze komen niet met alle regels overeen. Voeg een route dat berichten naar verzendt berichten met het ingebouwde eindpunt ook over een aangepaste eindpunt worden afgeleverd, de **gebeurtenissen** eindpunt.

> [!NOTE]
> IoT Hub biedt alleen ondersteuning voor het schrijven van gegevens naar Azure Storage-containers als blobs.

> [!WARNING]
> Service Bus-wachtrijen en onderwerpen met **sessies** of **detectie van dubbele** ingeschakeld worden niet ondersteund als aangepaste eindpunten.

Zie voor meer informatie over het maken van aangepaste eindpunten van IoT-Hub [IoT-hubeindpunten][lnk-devguide-endpoints].

Zie voor meer informatie over het lezen van het aangepaste eindpunten:

* Lezen van [Azure Storage-containers][lnk-getstarted-storage].
* Lezen van [Event Hubs][lnk-getstarted-eh].
* Lezen van [Service Bus-wachtrijen][lnk-getstarted-queue].
* Lezen van [Service Bus-onderwerpen][lnk-getstarted-topic].

### <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over IoT-hubeindpunten [IoT-hubeindpunten][lnk-devguide-endpoints].

Zie voor meer informatie over de querytaal die u gebruikt voor het definiëren van regels voor het doorsturen [querytaal IoT Hub voor apparaat horende, taken en berichtroutering][lnk-devguide-query-language].

De [proces IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie laat zien hoe u regels voor het doorsturen en aangepaste eindpunten.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
