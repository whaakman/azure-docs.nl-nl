---
title: Event Grid, routering voor IoT Hub vergelijken | Microsoft Docs
description: IoT Hub biedt een eigen bericht routing-service, maar kan ook worden geïntegreerd met Event Grid voor het publiceren van de gebeurtenis. Vergelijk de twee functies.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: e7d5ab3ebdf29fbf699220a3b214176ec4862739
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672794"
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>Berichtroutering en Event Grid vergelijken voor IoT-Hub

Azure IoT Hub biedt de mogelijkheid voor het streamen van gegevens van uw verbonden apparaten en het integreren van die gegevens in uw zakelijke toepassingen. IoT Hub biedt twee methoden voor het integreren van IoT-gebeurtenissen in andere Azure-services of -business-toepassingen. Dit artikel worden de twee functies die deze mogelijkheid te bieden, zodat u kunt kiezen welke optie wordt aanbevolen voor uw scenario.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

**[IoT Hub-berichtroutering](iot-hub-devguide-messages-d2c.md)**: Deze IoT Hub-functie kunnen gebruikers voor het routeren van apparaat-naar-cloud-berichten naar de service-eindpunten, zoals Azure Storage-containers, Event Hubs, Service Bus-wachtrijen en Service Bus-onderwerpen. Routering biedt ook een mogelijkheid een query uitvoeren op voor het filteren van de gegevens voordat deze naar de eindpunten. Naast het apparaat telemetriegegevens, kunt u ook verzenden [niet telemetriegebeurtenissen](iot-hub-devguide-messages-d2c.md#non-telemetry-events) die kunnen worden gebruikt voor het activeren van acties. 

**Integratie van IoT-Hub met Event Grid**: Azure Event Grid is een volledig beheerde service voor gebeurtenisroutering die gebruikmaakt van een publish-subscribe-model. IoT Hub en Event Grid samenwerken om [IoT Hub-gebeurtenissen integreren met Azure en niet-Azure-services](iot-hub-event-grid.md), in near-real-time. 

## <a name="differences"></a>Verschillen

Berichtroutering en Event Grid waarschuwing configuratie inschakelt, maar er zijn enkele belangrijke verschillen tussen de twee. Raadpleeg de volgende tabel voor meer informatie:

| Functie | IoT Hub-berichtroutering | Integratie van IoT-Hub met Event Grid |
| ------- | --------------- | ---------- |
| **Apparaat-berichten** | Ja, kan berichtroutering worden gebruikt voor telemetrische gegevens. | Nee, Event Grid kunnen alleen worden gebruikt voor niet-telemetrie IoT Hub-gebeurtenissen. |
| **Apparaatgebeurtenissen** | Ja, routering van berichten kan rapporteren dat dubbele wijzigingen en levenscyclusgebeurtenissen voor het apparaat. | Ja, Event Grid kunt rapporteren wanneer apparaten zijn gemaakt, verwijderd, verbonden en IoT Hub is verbroken |
| **Bestellen** | Ja, ordening van gebeurtenissen wordt bijgehouden.  | Nee, de volgorde van gebeurtenissen is niet noodzakelijkerwijs. | 
| **Maximale berichtgrootte** | 256 KB, apparaat-naar-cloud | 64 kB |
| **Filteren** | Geavanceerde op Eigenschappen van de toepassing berichten filteren, twin bericht Systeemeigenschappen, hoofdtekst van het bericht apparaat apparaatdubbel-tags en apparaat eigenschappen. Zie voor voorbeelden van [bericht routering querysyntaxis](iot-hub-devguide-routing-query-syntax.md). | Filteren op basis van achtervoegsel/voorvoegsel van apparaat-id's die geschikt is voor hiërarchische services zoals opslag. |
| **Eindpunten** | <ul><li>Event Hubs</li> <li>Azure Blob Storage</li> <li>Service Bus-wachtrij</li> <li>Service Bus-onderwerpen</li></ul><br>Betaalde IoT Hub-SKU's (S1, S2 en S3) zijn beperkt tot 10 aangepaste eindpunten. 100 routes kunnen per IoT Hub worden gemaakt. | <ul><li>Azure Functions</li> <li>Azure Automation</li> <li>Event Hubs</li> <li>Logic Apps</li> <li>Storage Blob</li> <li>Aangepaste onderwerpen</li> <li>De services van derden via WebHooks</li></ul><br>Zie voor de meest recente lijst met eindpunten, [Event Grid-gebeurtenis-handlers](../event-grid/overview.md#event-handlers). |
| **Kosten** | Er is geen aparte toeslag voor het routeren van berichten. Alleen inkomend verkeer van telemetrie naar IoT Hub wordt in rekening gebracht. Bijvoorbeeld, als u een bericht doorgestuurd naar drie verschillende eindpunten hebt, u worden in rekening gebracht voor slechts één bericht. | Er zijn geen kosten van IoT Hub. Event Grid biedt de eerste 100.000 bewerkingen per maand gratis, en vervolgens $0,60 per miljoen bewerkingen daarna. |

## <a name="similarities"></a>Overeenkomsten

Berichtroutering in IoT Hub en Event Grid hebt overeenkomsten te, waarvan sommige in de volgende tabel worden beschreven:

| Functie | IoT Hub-berichtroutering | Integratie van IoT-Hub met Event Grid |
| ------- | --------------- | ---------- |
| **Betrouwbaarheid** | Hoog: Voorziet in elk bericht naar het eindpunt ten minste één keer voor elke route. Alle berichten die niet worden bezorgd binnen een uur is verlopen. | Hoog: Elk bericht biedt naar de webhook ten minste eenmaal voor elk abonnement. Alle gebeurtenissen die niet worden bezorgd binnen 24 uur is verlopen. | 
| **Schaalbaarheid** | Hoog: Geoptimaliseerd naar miljoenen gelijktijdig verbonden apparaten miljarden berichten verzenden. | Hoog: Geschikt voor routering 10.000.000 gebeurtenissen per seconde per regio. |
| **Latentie** | Laag: Near-real-time. | Laag: Near-real-time. |
| **Verzenden naar meerdere eindpunten** | Ja, een enkel bericht verzenden naar meerdere eindpunten. | Ja, een enkel bericht verzenden naar meerdere eindpunten.  | 
| **Beveiliging** | IOT Hub biedt per apparaat-id en terughalen access control. Zie voor meer informatie de [IoT Hub-toegangsbeheer](iot-hub-devguide-security.md). | Event Grid biedt validatie op de drie punten: gebeurtenisabonnementen gebeurtenispublicatie en webhook-gebeurtenisverzending. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Hoe u kunt kiezen

IoT Hub-berichtroutering en de IoT Hub-integratie met Event Grid verschillende bewerkingen uitvoeren om te bereikt u hetzelfde resultaat. Ze zowel informatie uit uw IoT Hub-oplossing en doorgeven op, zodat andere services kunnen reageren. Hoe bepaalt u welke computer te gebruiken? Houd rekening met de volgende vragen als richtlijn voor uw beslissing: 

* **Wat voor soort gegevens verzendt u naar de eindpunten?**

   Gebruik IoT Hub-berichtroutering wanneer er telemetrische gegevens verzenden naar andere services. Berichtroutering ook kunnen toepassings- en eigenschappen van berichten, hoofdtekst van het bericht apparaat apparaatdubbel-tags en apparaatdubbeleigenschappen uitvoeren van query's.

   De IoT Hub-integratie met Event Grid werkt met gebeurtenissen die in de IoT Hub-service plaatsvinden. Deze IoT Hub-gebeurtenissen zijn onder andere apparaat gemaakt, verwijderd, verbonden en de verbinding verbroken. 

* **Welke eindpunten nodig om deze informatie te ontvangen?**

   IoT Hub-berichtroutering ondersteunt alleen eindpunten, maar u kunt connectors voor het omleiden van de gegevens en gebeurtenissen naar extra eindpunten maken. Zie voor een volledige lijst van ondersteunde eindpunten, de tabel in de vorige sectie. 

   De IoT Hub-integratie met Event Grid ondersteunt meer eindpunten. Het werkt ook met webhooks om uit te breiden routering buiten het ecosysteem van Azure-service en in toepassingen van derden. 

* **Maakt het uit of uw gegevens in volgorde binnenkomen?**

   IoT Hub-berichtroutering wordt gehandhaafd, de volgorde waarin berichten worden verzonden, zodat ze op dezelfde manier binnenkomen.

   Event Grid is geen garantie dat eindpunten gebeurtenissen ontvangen in dezelfde volgorde als ze opgetreden. Het gebeurtenissenschema bevat echter een timestamp die kan worden gebruikt voor het identificeren van de volgorde nadat de gebeurtenissen op het eindpunt plaatsvinden. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [IoT Hub-berichtroutering](iot-hub-devguide-messages-d2c.md) en de [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md).
* Meer informatie over [Azure Event Grid](../event-grid/overview.md).
* Zie voor meer informatie over het maken van berichtroutes, de [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes](../iot-hub/tutorial-routing.md) zelfstudie.
* Probeer uit de Event Grid-integratie door [e-mailmeldingen verzenden over Azure IoT Hub-gebeurtenissen met Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md).