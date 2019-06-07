---
title: Overzicht van Azure IoT Hub bericht enrichments
description: Overzicht van bericht enrichments voor Azure IoT Hub-berichten
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: 13e35ab93fc37541548785c6355489eaf3a3efc2
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754560"
---
# <a name="message-enrichments-for-device-to-cloud-iot-hub-messages-preview"></a>Bericht enrichments voor IoT Hub apparaat-naar-cloud-berichten (preview)

*Bericht enrichments* is het vermogen van de IoT-Hub kunt u *stempel* berichten met aanvullende informatie voordat de berichten worden verzonden naar het opgegeven eindpunt. Eén reden voor het gebruik van bericht enrichments is om op te nemen van gegevens die kunnen worden gebruikt voor het vereenvoudigen van de downstream-verwerkingen. Bijvoorbeeld, het verrijkende telemetrieberichten van apparaten met een dubbele apparaattag belasting van klanten om te maken van de apparaatdubbel die API-voor deze informatie aanroepen verminderen.

![Bericht enrichments stroom](./media/iot-hub-message-enrichments-overview/message-enrichments-flow.png)

Een bericht verrijking heeft drie belangrijke elementen:

* De naam van verrijking of sleutel

* Een waarde

* Een of meer [eindpunten](iot-hub-devguide-endpoints.md) voor verrijking moet worden toegepast.

De sleutel kan een willekeurige tekenreeks zijn.

De waarde kan een van de volgende voorbeelden zijn:

* Een statische tekenreeks. Dynamische waarden zoals voorwaarden, logische, bewerkingen en -functies zijn niet toegestaan. Bijvoorbeeld, als u een SaaS-toepassing die wordt gebruikt door meerdere klanten ontwikkelt, kunt u wijst een id toe aan elke klant en deze id in de toepassing beschikbaar te maken. Wanneer de toepassing wordt uitgevoerd, wordt IoT Hub tijdstempel het apparaat berichten over telemetrie van de klant-id, waardoor het mogelijk voor het verwerken van de berichten anders voor elke klant.

* Gegevens van de dubbele apparaten, zoals het pad ervan. Voorbeelden zijn *$twin.tags.field* en *$twin.tags.latitude*.

* De naam van de IoT-hub te verzenden. Deze waarde is *$iothubname*.

## <a name="applying-enrichments"></a>Enrichments toepassen

De berichten kunnen afkomstig zijn van een gegevensbron wordt ondersteund door [IoT Hub-berichtroutering](iot-hub-devguide-messages-d2c.md), met inbegrip van de volgende voorbeelden:

* Telemetrie van apparaten, zoals temperatuur of druk te verlichten
* Device twin wijzigingsmeldingen--wijzigingen in de apparaatdubbel
* apparaat-levenscyclus van gebeurtenissen, zoals wanneer het apparaat is gemaakt of verwijderd

U kunt enrichments toevoegen aan de berichten die naar het ingebouwde eindpunt van een IoT-Hub of berichten die worden doorgestuurd naar aangepaste eindpunten, zoals Azure Blob storage, een Service Bus-wachtrij of een Service Bus-onderwerp.

U kunt ook enrichments toevoegen op berichten die worden gepubliceerd naar Event Grid door te selecteren van het eindpunt als Event Grid. Zie voor meer informatie, [Iot-Hub en Event Grid](iot-hub-event-grid.md).

Enrichments worden toegepast per eindpunt. Als u vijf enrichments om te worden geautoriseerd voor een bepaald eindpunt opgeeft, worden alle berichten die naar dit eindpunt met de dezelfde vijf enrichments stempel.

Zie voor meer informatie over voor het uitproberen van bericht enrichments, de [bericht enrichments zelfstudie](tutorial-message-enrichments.md)

## <a name="limitations"></a>Beperkingen

* U kunt maximaal 10 enrichments per IoT Hub voor deze hubs toevoegen in de standard- of basic-laag. Voor IoT-Hubs in de gratis laag, kunt u maximaal 2 enrichments toevoegen.

* In sommige gevallen, als u bij het toepassen van een verrijking met een waarde ingesteld op een label of de eigenschap op het dubbele apparaat zal de waarde worden geautoriseerd als een string-waarde. Bijvoorbeeld, als een verrijking-waarde is ingesteld op $twin.tags.field, worden de berichten worden factureringslabel voor de tekenreeks "$twin.tags.field" in plaats van de waarde van dat veld van het dubbele. Dit gebeurt in de volgende gevallen:

   * Uw IoT-Hub is in de laag basis. Basic-laag IoT-hubs bieden geen ondersteuning voor dubbele apparaten.

   * Uw IoT-Hub is in de laag standaard, maar is geen dubbele apparaat op het apparaat dat het bericht te verzenden.

   * Uw IoT-Hub is in de laag standaard, maar het apparaatdubbel pad gebruikt voor de waarde van de verrijking bestaat niet. Bijvoorbeeld, als de waarde verrijking is ingesteld op $twin.tags.location en het dubbele apparaat heeft geen een locatie-eigenschap onder tags, het bericht is een factureringslabel voor de tekenreeks "$twin.tags.location". 

* Updates voor een apparaatdubbel kunnen worden weergegeven in de overeenkomstige waarde voor verrijking tot vijf minuten duren.

* De totale berichtgrootte, inclusief de enrichments kan niet groter zijn dan 256 KB. Als een grootte van het bericht groter is dan 256 KB, wordt het bericht verwijderen in de IoT-Hub. U kunt [metrische gegevens van IoT-Hub](iot-hub-metrics.md) om te bepalen en fouten opsporen wanneer berichten worden verwijderd. U kunt bijvoorbeeld d2c.telemetry.egress.invalid bewaken.

## <a name="pricing"></a>Prijzen

Bericht enrichments zijn beschikbaar voor zonder extra kosten. Op dit moment in rekening gebracht wanneer u een bericht naar een IoT-Hub verzendt. U betaalt alleen één keer voor dit bericht, zelfs als het bericht naar meerdere eindpunten gaat.

## <a name="availability"></a>Beschikbaarheid

Deze mogelijkheid is beschikbaar in preview en is beschikbaar in alle regio's behalve VS-Oost, VS-West, West-Europa, [Azure Government](/azure/azure-government/documentation-government-welcome), [Azure China 21Vianet](/azure/china), en [Azure Duitsland](https://azure.microsoft.com/global-infrastructure/germany/).

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie over het routeren van berichten naar een IoT-Hub:

* [Gebruik IoT Hub-berichtroutering apparaat-naar-cloud-berichten te verzenden naar verschillende eindpunten](iot-hub-devguide-messages-d2c.md)

* [Zelfstudie: IoT Hub-routering](tutorial-routing.md)