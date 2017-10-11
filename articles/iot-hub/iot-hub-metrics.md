---
title: Metrische gegevens gebruiken voor het bewaken van Azure IoT Hub | Microsoft Docs
description: Het gebruik van Azure IoT Hub metrische gegevens om te beoordelen en de algehele status van uw IoT-hubs.
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e850370faf2d271b4adad1af48c1ead7b316fa67
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="understand-iot-hub-metrics"></a>Inzicht in IoT Hub metrische gegevens
IoT Hub metrische gegevens kunt u betere gegevens over de status van de Azure IoT-resources in uw Azure-abonnement. IoT Hub metrische gegevens kunt u de algemene status van de IoT Hub-service en de apparaten die zijn verbonden met het beoordelen. Gebruikersgerichte statistieken zijn belangrijk omdat ze u zien wat er gebeurt met uw IoT hub help hoofdoorzaak problemen met en helpen zonder contact opnemen met ondersteuning van Azure.

Metrische gegevens zijn standaard ingeschakeld. U kunt IoT Hub metrische gegevens van de Azure-portal bekijken.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub metrische gegevens weergeven
1. Een iothub maken. U vindt instructies over het maken van een IoT-hub in de [aan de slag] [ lnk-get-started] handleiding.
2. Open de blade van uw IoT-hub. Van daaruit, klikt u op **metrische gegevens**.
   
    ![][1]
3. U kunt de metrische gegevens weergeven voor uw iothub en aangepaste weergaven van metrische gegevens over uw maken op de blade metrische gegevens. U kunt de metrische gegevens verzenden naar een Event Hubs-eindpunt of een Azure Storage-account door te klikken op **diagnostische instellingen**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub metrische gegevens en hoe ze te gebruiken
IoT Hub biedt verschillende maatstaven voor een overzicht gegeven van de status van uw hub en het totale aantal verbonden apparaten. U kunt gegevens uit meerdere metrische gegevens te tekenen van een grotere afbeelding van de status van de IoT-hub kunt combineren. De volgende tabel beschrijft de metrische gegevens houdt elke IoT-hub en hoe elke metriek is gekoppeld aan de algehele status van de IoT-hub.

|Gegevens|Metrische weergavenaam|Eenheid|Samenvoegingstype|Beschrijving|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal voor apparaat-naar-cloud-telemetrieberichten worden verzonden naar uw IoT-hub geprobeerd|
|d2c.telemetry.ingress.Success|Telemetrieberichten|Count|Totaal|Aantal voor apparaat-naar-cloud-telemetrieberichten is verzonden naar uw IoT-hub|
|c2d.Commands.egress.complete.Success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|
|c2d.Commands.egress.Abandon.Success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|
|c2d.Commands.egress.Reject.Success|Opdrachten geweigerd|Count|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|
|devices.totalDevices|Totaal aantal apparaten|Count|Totaal|Aantal apparaten geregistreerd bij uw IoT-hub|
|devices.connectedDevices.allProtocol|Verbonden apparaten|Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|
|d2c.telemetry.egress.Success|Telemetrieberichten geleverd|Count|Totaal|Aantal keren dat berichten zijn geschreven naar eindpunten (totaal)|
|d2c.telemetry.egress.Dropped|Verwijderde berichten|Count|Totaal|Aantal berichten is verwijderd omdat ze komt niet overeen met alle routes en de alternatieve route is uitgeschakeld|
|d2c.telemetry.egress.orphaned|Zwevende berichten|Count|Totaal|Het aantal berichten die niet voldoen aan alle routes met inbegrip van de route voor terugval|
|d2c.telemetry.egress.Invalid|Ongeldige-berichten|Count|Totaal|Het aantal berichten niet worden bezorgd omdat ze niet compatibel met het eindpunt|
|d2c.telemetry.egress.fallback|Berichten die overeenkomt met alternatieve voorwaarde|Count|Totaal|Aantal berichten dat is geschreven naar het eindpunt voor terugval|
|d2c.endpoints.egress.eventHubs|Berichten in de Event Hub-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar de Event Hub-eindpunten|
|d2c.endpoints.Latency.eventHubs|Bericht latentie voor Event Hub-eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een Event Hub-eindpunt, in milliseconden|
|d2c.endpoints.egress.serviceBusQueues|Berichten worden afgeleverd bij Service Bus-wachtrij-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Service Bus-wachtrij-eindpunten|
|d2c.endpoints.Latency.serviceBusQueues|Bericht latentie voor Service Bus-wachtrij-eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een Service Bus-wachtrij eindpunt, in milliseconden|
|d2c.endpoints.egress.serviceBusTopics|Berichten worden afgeleverd bij Service Bus-onderwerp eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Service Bus-onderwerp eindpunten|
|d2c.endpoints.Latency.serviceBusTopics|Bericht latentie voor Service Bus-onderwerp eindpunten|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in een eindpunt Service Bus-onderwerp in milliseconden|
|d2c.endpoints.egress.builtIn.events|Berichten die worden geleverd aan het ingebouwde eindpunt (berichten/gebeurtenissen)|Count|Totaal|Aantal keren dat berichten zijn geschreven naar het ingebouwde eindpunt (berichten/gebeurtenissen)|
|d2c.endpoints.Latency.builtIn.events|Bericht latentie voor het ingebouwde eindpunt (berichten/gebeurtenissen)|milliseconden|Gemiddelde|De gemiddelde latentie tussen inkomend bericht met de iothub en inkomend bericht in het ingebouwde eindpunt (berichten/gebeurtenissen), in milliseconden |
|d2c.Twin.Read.Success|Geslaagde twin leesbewerkingen van apparaten|Count|Totaal|De telling van alle geslaagde apparaat geïnitieerde twin leest.|
|d2c.Twin.Read.failure|Twin leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle apparaat geïnitieerde twin leesbewerkingen mislukt.|
|d2c.Twin.Read.Size|De grootte van de antwoorden twin leesbewerkingen van apparaten|Bytes|Gemiddelde|De gemiddelde min en max van alle geslaagde apparaat geïnitieerde twin leest.|
|d2c.Twin.update.Success|Geslaagde twin updates van apparaten|Count|Totaal|Het aantal updates van alle geslaagde twin apparaat gestart.|
|d2c.Twin.update.failure|Twin updates van apparaten is mislukt|Count|Totaal|De telling van alle apparaat geïnitieerde twin updates mislukt.|
|d2c.Twin.update.Size|Grootte van de updates twin van apparaten|Bytes|Gemiddelde|De gemiddelde min. en max. grootte van alle geslaagde apparaat geïnitieerde twin updates.|
|c2d.Methods.Success|Geslaagde directe methode aanroepen|Count|Totaal|De telling van alle geslaagde directe methodeaanroepen.|
|c2d.Methods.failure|Directe methode aanroepen is mislukt|Count|Totaal|De telling van alle directe methodeaanroepen mislukt.|
|c2d.Methods.requestSize|Aanvraaggrootte van directe methode aanroepen|Bytes|Gemiddelde|De gemiddelde, min en max van alle geslaagde aanvragen voor directe methode.|
|c2d.Methods.responseSize|Reactiegrootte van directe methode aanroepen|Bytes|Gemiddelde|De gemiddelde, min en max van alle geslaagde directe methode antwoorden.|
|c2d.Twin.Read.Success|Geslaagde twin leest uit back-end|Count|Totaal|De telling van alle geslaagde back-end-geïnitieerde twin leest.|
|c2d.Twin.Read.failure|Mislukte twin leest uit back-end|Count|Totaal|De telling van alle kan geen back-end-geïnitieerde twin leest.|
|c2d.Twin.Read.Size|De grootte van de antwoorden twin leesbewerkingen van back-end|Bytes|Gemiddelde|De gemiddelde min en max van alle geslaagde back-end-geïnitieerde twin leest.|
|c2d.Twin.update.Success|Geslaagde twin updates van de back-end|Count|Totaal|De telling van alle geslaagde back-end-geïnitieerde twin updates.|
|c2d.Twin.update.failure|Mislukte twin updates van de back-end|Count|Totaal|De telling van alle back-end-geïnitieerde twin updates mislukt.|
|c2d.Twin.update.Size|Grootte van twin updates van de back-end|Bytes|Gemiddelde|De gemiddelde min. en max. grootte van alle geslaagde back-end-geïnitieerde twin updates.|
|twinQueries.success|Geslaagde twin query 's|Count|Totaal|De telling van alle geslaagde twin query's.|
|twinQueries.failure|Mislukte twin query 's|Count|Totaal|De telling van alle mislukte twin query's.|
|twinQueries.resultSize|Grootte van query's Twin|Bytes|Gemiddelde|De gemiddelde, min en max van de grootte van het resultaat van alle geslaagde twin query's.|
|jobs.createTwinUpdateJob.success|Geslaagde bewerkingen voor het maken van twin taken bijwerken|Count|Totaal|De telling van alle geslaagde twin update taken worden gemaakt.|
|jobs.createTwinUpdateJob.failure|Mislukte bewerkingen voor het maken van twin taken bijwerken|Count|Totaal|De telling van alle mislukte twin update taken worden gemaakt.|
|jobs.createDirectMethodJob.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle geslaagde directe methode aanroepen taken worden gemaakt.|
|jobs.createDirectMethodJob.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle maken van taken voor directe methode-aanroep is mislukt.|
|jobs.listJobs.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|De telling van alle geslaagde aanroepen naar de lijst met taken.|
|jobs.listJobs.failure|Kan niet aanroepen voor een lijst met taken|Count|Totaal|De telling van alle mislukte aanroepen naar de lijst met taken.|
|jobs.cancelJob.success|Geslaagde taakannuleringen|Count|Totaal|De telling van alle geslaagde aanroepen naar een taak annuleren.|
|jobs.cancelJob.failure|Mislukte taakannuleringen|Count|Totaal|De telling van alle mislukte aanroepen voor een taak annuleren.|
|jobs.queryJobs.success|Taak query 's|Count|Totaal|De telling van alle geslaagde aanroepen naar query taken.|
|jobs.queryJobs.failure|Taak voert een query is mislukt|Count|Totaal|De telling van alle mislukte aanroepen naar query taken.|
|jobs.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|
|jobs.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|

## <a name="next-steps"></a>Volgende stappen
Nu u een overzicht van IoT Hub metrische gegevens hebt gezien, volgt u deze koppeling voor meer informatie over het beheren van Azure IoT Hub:

* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [Een apparaat simuleren met Azure IoT rand][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
