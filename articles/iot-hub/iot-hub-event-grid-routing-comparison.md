---
title: Vergelijk gebeurtenis raster routering voor IoT Hub | Microsoft Docs
description: "IoT Hub biedt een eigen bericht routing-service, maar ook worden geïntegreerd met gebeurtenis raster voor het publiceren van de gebeurtenis. Vergelijk de twee functies."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 5a0a97ccf033b2981ba13be455482146ba212228
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="compare-message-routing-and-event-grid-for-iot-hub"></a>Berichtroutering en gebeurtenis raster vergelijken voor IoT-Hub

Azure IoT Hub biedt de mogelijkheid om te streamen van gegevens van uw verbonden apparaten en die gegevens integreren in uw zakelijke toepassingen. IoT Hub biedt twee methoden voor het integreren van IoT-gebeurtenissen in andere Azure-services of -business-toepassingen. Dit artikel worden de twee functies die deze mogelijkheid te bieden, zodat u kunt kiezen welke optie wordt aanbevolen voor uw scenario.

* **IoT Hub berichtroutering**: deze IoT Hub-functie kunnen gebruikers zich [apparaat-naar-cloud-berichten routeren](iot-hub-devguide-messages-read-custom.md) met service-eindpunten zoals Azure Storage-containers, Event Hubs, Service Bus-wachtrijen en Service Bus-onderwerpen. Routeringsregels de flexibiliteit bieden om routes op basis van een query uitvoeren. Ze ook inschakelen [kritieke waarschuwingen](iot-hub-devguide-messages-d2c.md) die acties through-query's te activeren en kan worden gebaseerd op het berichtkoppen en de hoofdtekst. 
* **IoT Hub-integratie met gebeurtenis raster**: Azure gebeurtenis raster is een volledig beheerde gebeurtenis routing-service die gebruikmaakt van een publiceren-abonneren model. IoT Hub en Event raster samenwerken, zodat [IoT Hub gebeurtenissen integreren met Azure en niet-Azure-services](iot-hub-event-grid.md), in near-realtime. 

## <a name="similarities-and-differences"></a>Overeenkomsten en verschillen

Berichtroutering en gebeurtenis raster waarschuwing configuratie inschakelt, maar er zijn enkele belangrijke verschillen tussen de twee. Raadpleeg de volgende tabel voor meer informatie:

| Functie | Berichtroutering IoT-Hub | IoT Hub-integratie met gebeurtenis raster |
| ------- | --------------- | ---------- |
| **Apparaat-berichten** | Ja, kan berichtroutering worden gebruikt voor telemetrische gegevens. | Nee, gebeurtenis raster kan alleen worden gebruikt voor niet-telemetrie IoT Hub gebeurtenissen. |
| **Gebeurtenistype** | Ja, berichtroutering kan rapporteren dat twin wijzigingen en device lifecycle gebeurtenissen. | Gebeurtenis raster kan Ja, rapporteren wanneer apparaten zijn geregistreerd bij een IoT-Hub en wanneer apparaten zijn verwijderd. |
| **Ordening** | Ja, ordening van gebeurtenissen wordt bijgehouden.  | Nee, de volgorde van gebeurtenissen wordt niet gegarandeerd. | 
| **Maximale berichtgrootte** | 256 KB, device-to-cloud | 64 kB |
| **Filtering** | Rich filtering via SQL-achtige taal biedt ondersteuning voor filteren op berichtkoppen en instanties. Zie voor voorbeelden [IoT Hub-querytaal](iot-hub-devguide-query-language.md). | Filteren op basis van achtervoegsels /-voorvoegsel van apparaat-id's die geschikt is voor hiërarchische services zoals opslag. |
| **Eindpunten** | <ul><li>Event Hub</li> <li>Storage-blob</li> <li>Service Bus-wachtrij</li> <li>Service Bus-onderwerpen</li></ul><br>Betaalde IoT Hub-SKU's (S1, S2 en S3) zijn beperkt tot 10 aangepaste eindpunten. 100 routes kunnen per IoT Hub worden gemaakt. | <ul><li>Azure Functions</li> <li>Azure Automation</li> <li>Event Hub</li> <li>Logic Apps</li> <li>Microsoft Flow</li> <li>De services van derden via webhooks.</li></ul><br>Zie voor de meest recente lijst met eindpunten [gebeurtenis raster gebeurtenis-handlers](../event-grid/overview.md#event-handlers). |
| **Kosten** | Er zijn geen afzonderlijke kosten voor het routeren van berichten. U betaalt alleen inkomend telemetrie in IoT Hub. Als u een bericht naar drie verschillende eindpunten gerouteerd hebt, kunt u wordt bijvoorbeeld voor gefactureerd voor slechts één bericht. | Er zijn geen kosten uit IoT Hub. Gebeurtenis raster biedt de eerst 100.000 bewerkingen per maand gratis en vervolgens $0,60 per miljoen operations daarna. |

Berichtroutering IoT Hub en Event raster hebt overeenkomsten te, waarvan sommige in de volgende tabel worden beschreven:

| Functie | Berichtroutering IoT-Hub | IoT Hub-integratie met gebeurtenis raster |
| ------- | --------------- | ---------- |
| **Betrouwbaarheid** | Hoog: Levert elk bericht naar het eindpunt ten minste één keer voor elke route. Alle berichten die niet worden bezorgd binnen een uur is verlopen. | Hoog: Levert elk bericht aan de webhook ten minste eenmaal voor elk abonnement. Alle gebeurtenissen die niet worden bezorgd binnen 24 uur is verlopen. | 
| **Schaalbaarheid** | Hoog: Voor optimale ter ondersteuning van miljoenen gelijktijdig verbonden apparaten miljarden berichten verzenden. | Hoog: Geschikt voor routering 10.000.000 gebeurtenissen per seconde per regio. |
| **Latentie** | Laag: Vrijwel in realtime. | Laag: Vrijwel in realtime. |
| **Verzenden naar meerdere eindpunten** | Ja, één bericht verzenden naar meerdere eindpunten. | Ja, één bericht verzenden naar meerdere eindpunten.  | 
| **Beveiliging** | IOT Hub biedt per apparaat-id en terughalen toegangsbeheer. Zie voor meer informatie de [IoT Hub toegangsbeheer](iot-hub-devguide-security.md). | Gebeurtenis raster validatie op drie punten bevat: abonnementen voor gebeurtenissen, gebeurtenis publiceren en levering van de webhook-gebeurtenis. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](../event-grid/security-authentication.md). |

## <a name="how-to-choose"></a>Hoe u kunt kiezen

Berichtroutering IoT Hub en de IoT Hub-integratie met gebeurtenis raster verschillende bewerkingen uitvoeren om te bereikt u hetzelfde resultaat. Ze zowel gegevens uit uw IoT Hub-oplossing en geef dit door op zodat andere services kunnen reageren. Hoe bepaal u welke een om te gebruiken? Gebruik de volgende vragen om u te helpen uw beslissing naast de gegevens uit het vorige gedeelte: 

* **Wat voor soort gegevens verzendt u naar de eindpunten?**

   Gebruik IoT Hub berichtroutering wanneer u telemetriegegevens verzenden naar andere services. Berichtroutering ook kunnen opvragen bericht berichtkoppen en tekst. 

   De integratie van de IoT Hub met gebeurtenis raster werkt met gebeurtenissen die zich in de IoT Hub-service. Deze IoT Hub-gebeurtenissen bevatten apparaat maken en verwijderen. 

* **Wat eindpunten nodig om deze informatie te ontvangen?**

   IoT Hub berichtroutering ondersteunt beperkt eindpunten, maar kunt u de connectors voor het omleiden van de gegevens en gebeurtenissen die moeten worden extra eindpunten maken. Zie de tabel in de vorige sectie voor een volledige lijst van ondersteunde eindpunten. 

   De integratie van de IoT Hub met gebeurtenis raster ondersteunt meer eindpunten. Dit werkt ook met webhooks uit te breiden routering buiten het ecosysteem van de Azure-service en in de derde partij business-toepassingen. 

* **Maakt het als uw gegevens in volgorde aankomen uit?**

   IoT Hub berichtroutering houdt de de volgorde waarin berichten worden verzonden, zodat deze op dezelfde manier binnenkomen.

   Gebeurtenis raster kan niet garanderen dat eindpunten gebeurtenissen in dezelfde volgorde ontvangen dat ze zijn opgetreden. Het schema van de gebeurtenis bevat echter een tijdstempel dat kan worden gebruikt voor het identificeren van de volgorde nadat de gebeurtenissen op het eindpunt plaatsvinden. 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [IoT Hub berichtroutering](iot-hub-devguide-messages-d2c.md) en de [IoT-hubeindpunten](iot-hub-devguide-endpoints.md).

* Meer informatie over [Azure gebeurtenis raster](../event-grid/overview.md)

* De gebeurtenis raster integratie door uitproberen [verzonden e-mailmeldingen over gebeurtenissen van Azure IoT Hub met Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md)