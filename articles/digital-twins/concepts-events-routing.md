---
title: Routering gebeurtenissen en berichten met Azure digitale dubbels | Microsoft Docs
description: Overzicht van de routering van gebeurtenissen en berichten naar de service-eindpunten met Azure digitale dubbels
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: a1a31ec7ee0d1daea9f178e51dc860279d3787ec
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615334"
---
# <a name="routing-events-and-messages"></a>Routering van gebeurtenissen en berichten

IoT-oplossingen aandragen vaak verschillende krachtige services met opslag, analyse en meer. In dit artikel wordt beschreven hoe u Azure digitale dubbels apps verbinden met Azure analytics, AI en storage-services zodat ze diepere inzichten en functionaliteiten.

## <a name="route-types"></a>Route-typen  

Azure van digitale dubbels biedt twee manieren IoT-gebeurtenissen integreren met andere Azure-services of -business-toepassingen:

* **Routering Azure digitale dubbels gebeurtenissen**: een object in de ruimtelijke grafiek die wijzigingen, telemetrische gegevens die worden ontvangen, of een gebruiker gedefinieerde functie waarmee een melding op basis van vooraf gedefinieerde voorwaarden Azure digitale dubbels gebeurtenissen kunt activeren. Gebruikers kunnen deze gebeurtenissen te verzenden [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure Service Bus-onderwerpen](https://azure.microsoft.com/services/service-bus/), of [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) voor verdere verwerking.

* **Routering apparaattelemetrie**: naast de routering van gebeurtenissen, Azure digitale dubbels kunnen ook niet-gecodeerd apparaat telemetrieberichten routeren naar Event Hubs voor meer inzichten en analyses. Deze typen berichten worden niet verwerkt door Azure digitale dubbels. En ze alleen bent doorgestuurd naar de event hub.

Gebruikers kunnen een of meer uitgaande-eindpunten voor het verzenden van gebeurtenissen of doorsturen van berichten opgeven. Gebeurtenissen en berichten zal worden verzonden naar de eindpunten op basis van deze vooraf gedefinieerde routering voorkeuren. Met andere woorden, kunnen gebruikers een bepaalde eindpunt voor het ontvangen van gebeurtenissen voor graph-bewerking, een ander apparaat telemetriegebeurtenissen ontvangen, enzovoort opgeven.

![Azure digitale dubbels gebeurtenissen routering][1]

Routering naar Event Hubs, houdt de volgorde waarin berichten over telemetrie wordt verzonden. Ze dus binnenkomen op het eindpunt in dezelfde volgorde die oorspronkelijk zijn ontvangen. Event Grid en Service Bus garanderen niet dat eindpunten gebeurtenissen ontvangen in dezelfde volgorde als ze opgetreden. Het gebeurtenissenschema bevat echter een timestamp die kan worden gebruikt voor het identificeren van de volgorde nadat de gebeurtenissen op het eindpunt plaatsvinden.

## <a name="route-implementation"></a>Route-implementatie

De digitale dubbels Azure-service ondersteunt momenteel de volgende **EndpointTypes**:

* **Event hub** eindpunt tekenreeks voor de Event Hubs is.
* **Service Bus** is de tekenreeks-eindpunt van de Service Bus-verbinding.
* **EventGrid** eindpunt tekenreeks voor de Event Grid is.

Op dit moment ondersteunt de volgende Azure digitale dubbels **eigenschap EventTypes** die wordt verzonden naar het eindpunt van de gekozen:

* **DeviceMessages** zijn telemetrieberichten van apparaten van de gebruiker verzonden en doorgestuurd door het systeem.
* **TopologyOperation** is een bewerking waarmee de grafiek of de metagegevens van de grafiek wordt gewijzigd. Een voorbeeld is toevoegen of verwijderen van een entiteit, zoals een spatie.
* **SpaceChange** is een wijziging in de berekende waarde van een adresruimte die het resultaat is van een apparaat telemetrie-bericht.
* **SensorChange** is een wijziging in de berekende waarde van een sensor die het resultaat is van een apparaat telemetrie-bericht.
* **UdfCustom** is een aangepaste melding van een door de gebruiker gedefinieerde functie.

> [!IMPORTANT]  
> Niet alle **EndpointTypes** ondersteunen alle **eigenschap EventTypes**.
> Zie de volgende tabel voor de **eigenschap EventTypes** die zijn toegestaan voor elke **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| EventHub|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Zie voor meer informatie over het maken van eindpunten en voorbeelden van het schema van de gebeurtenissen [uitgaand verkeer en eindpunten](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Azure digitale dubbels preview limieten, [Servicelimieten voor openbare preview-versie](concepts-service-limits.md).
- Als u wilt uitproberen een voorbeeld van Azure digitale Twins, Zie de [Snelstartgids voor het vinden van beschikbare ruimten](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png
