---
title: Informatie over aangepaste eindpunten van Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - routering query's gebruiken voor het routeren van apparaat-naar-cloud-berichten met aangepaste eindpunten.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.author: dobett
ms.openlocfilehash: af0b819c6c60835089c174a1f9f7c3a6215e362c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956954"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Gebruik berichtroutes en aangepaste eindpunten voor apparaat-naar-cloud-berichten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub [berichtroutering](iot-hub-devguide-routing-query-syntax.md) kunnen gebruikers voor het routeren van apparaat-naar-cloud-berichten met gerichte service-eindpunten. Routering biedt ook een mogelijkheid een query uitvoeren op voor het filteren van de gegevens voordat deze naar de eindpunten. Elke routering query die u configureert heeft de volgende eigenschappen:

| Eigenschap      | Beschrijving |
| ------------- | ----------- |
| **Naam**      | De unieke naam ter identificatie van de query. |
| **Bron**    | De oorsprong van de gegevensstroom worden gereageerd. Bijvoorbeeld: telemetrie van apparaten. |
| **voorwaarde** | De query-expressie voor de routering query die wordt uitgevoerd tegen de berichteigenschappen van de toepassing, Systeemeigenschappen, hoofdtekst van het bericht, apparaat apparaatdubbel-tags en apparaatdubbeleigenschappen om te bepalen of er een overeenkomst voor het eindpunt. Zie voor meer informatie over het maken van een query de Zie [message routing query-syntaxis](iot-hub-devguide-routing-query-syntax.md) |
| **Endpoint**  | De naam van het eindpunt waar de IoT Hub-berichten die overeenkomen met de query verzendt. Het is raadzaam dat u een eindpunt in dezelfde regio als uw IoT-hub. |

Een enkel bericht mogelijk overeenkomt met de voorwaarde op meerdere Routering query's, geval IoT Hub waarin het bericht naar het eindpunt dat is gekoppeld aan elke overeenkomende query biedt. IoT Hub ook automatisch beleidinstellingen bezorging van berichten, zodat als een bericht overeenkomt met meerdere query's die hetzelfde doel hebben, ze alleen één keer naar deze bestemming geschreven worden.

## <a name="endpoints-and-routing"></a>Eindpunten en routering

Een IoT-hub is een standaard [ingebouwd eindpunt][lnk-built-in]. U kunt aangepaste eindpunten om berichten te routeren naar maken door andere services in uw abonnement koppelen aan de hub. IoT Hub ondersteunt momenteel Azure Storage-containers, Event Hubs, Service Bus-wachtrijen en Service Bus-onderwerpen als aangepaste eindpunten.

Wanneer u Routering en aangepaste eindpunten, worden alleen berichten met het ingebouwde eindpunt geleverd als ze niet overeenkomen met elke query. Toevoegen een route waarmee berichten worden verzonden om berichten te leveren met het ingebouwde eindpunt evenals garantie voor een aangepast eindpunt, de **gebeurtenissen** eindpunt.

> [!NOTE]
> IoT Hub biedt alleen ondersteuning voor het schrijven van gegevens naar Azure Storage-containers als blobs.

> [!WARNING]
> Service Bus-wachtrijen en onderwerpen met **sessies** of **detectie van dubbele** ingeschakeld worden niet ondersteund als aangepaste eindpunten.

Zie voor meer informatie over het maken van aangepaste eindpunten van IoT-Hub [IoT Hub-eindpunten][lnk-devguide-endpoints].

Zie voor meer informatie over het lezen van het aangepaste eindpunten:

* Lezen van [Azure Storage-containers][lnk-getstarted-storage].
* Lezen van [Eventhubs][lnk-getstarted-eh].
* Lezen van [Service Bus-wachtrijen][lnk-getstarted-queue].
* Lezen van [Service Bus-onderwerpen][lnk-getstarted-topic].

### <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over IoT Hub-eindpunten, [IoT Hub-eindpunten][lnk-devguide-endpoints].
* Zie voor meer informatie over de querytaal die u gebruikt voor het definiëren van routering query's [bericht routering querysyntaxis](iot-hub-devguide-routing-query-syntax.md).
* De [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie leert u hoe u routering query's en aangepaste eindpunten.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
