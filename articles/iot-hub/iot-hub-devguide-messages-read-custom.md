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
ms.openlocfilehash: 1b0e99998aaf29c366cce1926f930174686cfee6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414895"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Gebruik berichtroutes en aangepaste eindpunten voor apparaat-naar-cloud-berichten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub [berichtroutering](iot-hub-devguide-routing-query-syntax.md) kunnen gebruikers voor het routeren van apparaat-naar-cloud-berichten met gerichte service-eindpunten. Routering biedt ook een mogelijkheid een query uitvoeren op voor het filteren van de gegevens voordat deze naar de eindpunten. Elke routering query die u configureert heeft de volgende eigenschappen:

| Eigenschap      | Beschrijving |
| ------------- | ----------- |
| **Naam**      | De unieke naam ter identificatie van de query. |
| **Bron**    | De oorsprong van de gegevensstroom worden gereageerd. Bijvoorbeeld: telemetrie van apparaten. |
| **voorwaarde** | De query-expressie voor de routering query die wordt uitgevoerd op de berichteigenschappen van de toepassing, Systeemeigenschappen berichttekst, apparaat apparaatdubbel-tags en apparaatdubbeleigenschappen om te bepalen of er een overeenkomst voor het eindpunt. Zie voor meer informatie over het maken van een query de Zie [message routing query-syntaxis](iot-hub-devguide-routing-query-syntax.md) |
| **Endpoint**  | De naam van het eindpunt waar de IoT Hub-berichten die overeenkomen met de query verzendt. Het is raadzaam dat u een eindpunt in dezelfde regio als uw IoT-hub. |

Een enkel bericht mogelijk overeenkomt met de voorwaarde op meerdere Routering query's, geval IoT Hub waarin het bericht naar het eindpunt dat is gekoppeld aan elke overeenkomende query biedt. IoT Hub ook automatisch beleidinstellingen bezorging van berichten, zodat als een bericht overeenkomt met meerdere query's die hetzelfde doel hebben, ze alleen één keer naar deze bestemming geschreven worden.

## <a name="endpoints-and-routing"></a>Eindpunten en routering

Een IoT-hub is een standaard [ingebouwd eindpunt](iot-hub-devguide-messages-read-builtin.md). U kunt aangepaste eindpunten om berichten te routeren naar maken door andere services in uw abonnement koppelen aan de hub. IoT Hub ondersteunt momenteel Azure Storage-containers, Event Hubs, Service Bus-wachtrijen en Service Bus-onderwerpen als aangepaste eindpunten.

Wanneer u Routering en aangepaste eindpunten, worden alleen berichten met het ingebouwde eindpunt geleverd als ze niet overeenkomen met elke query. Om berichten te leveren met het ingebouwde eindpunt evenals garantie voor een aangepast eindpunt, toevoegen een route waarmee berichten worden verzonden in de ingebouwde **gebeurtenissen** eindpunt.

> [!NOTE]
> * IoT Hub biedt alleen ondersteuning voor het schrijven van gegevens naar Azure Storage-containers als blobs.
> * Service Bus-wachtrijen en onderwerpen met **sessies** of **detectie van dubbele** ingeschakeld worden niet ondersteund als aangepaste eindpunten.

Zie voor meer informatie over het maken van aangepaste eindpunten van IoT-Hub [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md).

Zie voor meer informatie over het lezen van het aangepaste eindpunten:

* Lezen van [Azure Storage-containers](../storage/blobs/storage-blobs-introduction.md).

* Lezen van [Eventhubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).

* Lezen van [Service Bus-wachtrijen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

* Lezen van [Service Bus-onderwerpen](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over IoT Hub-eindpunten, [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md).

* Zie voor meer informatie over de querytaal die u gebruikt voor het definiëren van routering query's [bericht routering querysyntaxis](iot-hub-devguide-routing-query-syntax.md).

* De [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes](tutorial-routing.md) zelfstudie leert u hoe u routering query's en aangepaste eindpunten.