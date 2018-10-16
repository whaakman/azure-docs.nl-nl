---
title: Routering van gebeurtenissen en berichten met Azure digitale dubbels | Microsoft Docs
description: Overzicht van de routering van gebeurtenissen en berichten naar de service-eindpunten met Azure digitale dubbels
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324167"
---
# <a name="routing-events-and-messages"></a>Routering van gebeurtenissen en -berichten

IoT-oplossingen aandragen vaak verschillende krachtige services inclusief opslag, analyse en meer. In dit artikel wordt beschreven hoe u Azure digitale dubbels apps verbinden met Azure analytics, AI en storage-services te verrijken met diepere inzichten en functionaliteiten.

## <a name="route-types"></a>Route-typen

Digitale dubbels biedt twee manieren IoT-gebeurtenissen integreren met andere Azure-services of -business-toepassingen:

* **Routering digitale dubbels gebeurtenissen**: Azure digitale dubbels gebeurtenissen kunnen worden geactiveerd wanneer een object in de wijzigingen ruimtelijke graph als telemetrische gegevens is ontvangen, of wanneer een door de gebruiker gedefinieerde functie maakt een melding op basis van vooraf gedefinieerde voorwaarden. Gebruikers kunnen deze gebeurtenissen te verzenden [Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Service Bus-onderwerpen](https://azure.microsoft.com/services/service-bus/), of [gebeurtenis rasters](https://azure.microsoft.com/services/event-grid/) voor verdere verwerking.

* **Routering apparaattelemetrie**: naast de routering van gebeurtenissen, Azure digitale dubbels kunnen ook niet-gecodeerd apparaat telemetrieberichten routeren naar Event Hubs voor meer inzichten en analyses. Deze typen berichten worden niet verwerkt door digitale dubbels en ze alleen worden doorgestuurd naar de **Event Hub**.

Gebruikers kunnen een of meer uitgaande-eindpunten voor het verzenden van gebeurtenissen of doorsturen van berichten opgeven. Gebeurtenissen en berichten zal worden verzonden naar de eindpunten op basis van deze vooraf gedefinieerde routering voorkeuren. Met andere woorden, kunnen gebruikers bepaalde één eindpunt voor het ontvangen van gebeurtenissen voor graph-bewerking, een ander apparaat telemetriegebeurtenissen ontvangen, enzovoort opgeven.

![Digitale dubbels gebeurtenissen routering][1]

Routering naar **Event Hubs** handhaaft de volgorde in welke telemetrie berichten worden verzonden, zodat ze op het eindpunt in dezelfde volgorde binnenkomen die oorspronkelijk zijn ontvangen. **Event Grid** en **Service Bus** niet garanderen dat eindpunten gebeurtenissen wordt ontvangen in dezelfde volgorde als ze opgetreden. Het gebeurtenissenschema bevat echter een timestamp die kan worden gebruikt voor het identificeren van de volgorde nadat de gebeurtenissen op het eindpunt plaatsvinden.

## <a name="route-implementation"></a>Route-implementatie

De digitale dubbels Azure-service ondersteunt momenteel de volgende **EndpointTypes**:

* **Event hub**: de tekenreeks-eindpunt van de Event Hub-verbinding is.
* **Service Bus**: de tekenreeks-eindpunt van de Service Bus-verbinding is.
* **EventGrid**: de tekenreeks-eindpunt van de Event Grid-verbinding is.

Op dit moment ondersteunt de volgende Azure digitale dubbels **eigenschap EventTypes** die wordt verzonden naar het eindpunt van de gekozen:

* **DeviceMessages**: zijn telemetrieberichten van apparaten van de gebruiker verzonden en doorgestuurd door het systeem.
* **TopologyOperation**: zijn bewerkingen die de grafiek of de metagegevens van de grafiek wijzigen. Bijvoorbeeld, toevoegen of verwijderen van een entiteit, zoals een spatie.
* **SpaceChange**: zijn wijzigingen in een ruimte van de berekende waarde als gevolg van een apparaat telemetrie-bericht.
* **SensorChange**: zijn wijzigingen in van een sensor berekende waarde als gevolg van een apparaat telemetrie-bericht.
* **UdfCustom**: aangepaste meldingen van een door de gebruiker gedefinieerde functie zijn.

> [!IMPORTANT]
> Niet alle **EndpointTypes** ondersteunen alle **eigenschap EventTypes**.
> Zie de onderstaande tabel voor de **eigenschap EventTypes** die zijn toegestaan voor elke **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **EventHub**|     X          |         X         |     X       |      X       |   X       |
| **ServiceBus**|              |         X         |     X       |      X       |   X       |
| **EventGrid**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Zie voor meer informatie over het maken van eindpunten en voorbeelden van het schema van de gebeurtenissen [eindpunten en uitgaand verkeer](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over limieten voor openbare preview, [digitale dubbels Azure preview-limieten](concepts-service-limits.md).

Als u wilt uitproberen een voorbeeld van Azure digitale Twins, lezen [Snelstartgids voor het vinden van beschikbare ruimten](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png