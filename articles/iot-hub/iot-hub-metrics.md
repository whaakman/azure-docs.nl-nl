---
title: Metrische gegevens gebruiken voor het bewaken van Azure IoT Hub | Microsoft Docs
description: Het gebruik van Azure IoT Hub metrische gegevens om te beoordelen en controleren van de algemene status van uw IoT-hubs.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: b41458f0201c46b99c09d0bfffd219743a36ad50
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186752"
---
# <a name="understand-iot-hub-metrics"></a>Informatie over metrische gegevens van IoT Hub
IoT Hub metrische gegevens geven u meer gegevens over de status van de Azure IoT-resources in uw Azure-abonnement. Metrische gegevens van IoT Hub kunt u de algemene status van de IoT Hub-service en de apparaten die zijn verbonden met het beoordelen. Gebruikersgerichte statistieken zijn belangrijk omdat ze helpen om zien wat er gebeurt met uw IoT hub en help hoofdoorzaak problemen te zonder contact opnemen met ondersteuning van Azure.

Metrische gegevens zijn standaard ingeschakeld. U kunt IoT Hub metrische gegevens van de Azure-portal weergeven.

## <a name="how-to-view-iot-hub-metrics"></a>IoT Hub metrische gegevens weergeven
1. Een IoT-hub maken. U vindt instructies over het maken van een IoT-hub in de [aan de slag] [ lnk-get-started] handleiding.
2. Open de blade van uw IoT-hub. Van daaruit, klikt u op **metrische gegevens**.
   
    ![][1]
3. U kunt de metrische gegevens weergeven voor uw IoT-hub en aangepaste weergaven van uw metrische gegevens maken op de blade metrische gegevens. U kunt uw metrische gegevens verzenden naar een Event Hubs-eindpunt of een Azure Storage-account door te klikken op **diagnostische instellingen**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-to-use-them"></a>IoT Hub metrische gegevens en hoe ze te gebruiken
IoT Hub biedt verschillende metrische gegevens zodat u een overzicht van de status van uw hub en het totale aantal verbonden apparaten. U kunt gegevens uit meerdere metrische gegevens tekenen van een grotere beeld van de status van de IoT-hub kunt combineren. De volgende tabel beschrijft de metrische gegevens die elke IoT-hub worden bijgehouden, en hoe alle gegevens zich verhoudt tot de algemene status van de IoT-hub.

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Type samenvoeging geselecteerd|Beschrijving|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal telemetrieberichten dat apparaat-naar-cloud heeft geprobeerd om te worden verzonden naar uw IoT-hub|
|d2c.telemetry.ingress.Success|Berichten over telemetrie verzonden|Count|Totaal|Aantal telemetrieberichten dat is verzonden naar uw IoT hub apparaat-naar-cloud|
|c2d.Commands.egress.complete.Success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|
|c2d.commands.egress.abandon.success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|
|c2d.commands.egress.reject.success|Opdrachten geweigerd|Count|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|
|devices.totalDevices|Totaal aantal apparaten|Count|Totaal|Aantal apparaten die zijn geregistreerd met uw IoT hub|
|devices.connectedDevices.allProtocol|Verbonden apparaten|Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|
|d2c.telemetry.egress.success|Berichten over telemetrie die worden geleverd|Count|Totaal|Aantal keren dat berichten zijn geschreven naar eindpunten (totaal)|
|d2c.telemetry.egress.dropped|Verwijderde berichten|Count|Totaal|Aantal berichten verwijderd omdat ze komt niet overeen met alle routes en de alternatieve route is uitgeschakeld|
|d2c.telemetry.egress.orphaned|Zwevende berichten|Count|Totaal|Het aantal berichten niet overeenkomen met routes met inbegrip van de alternatieve route|
|d2c.telemetry.egress.Invalid|Ongeldige berichten|Count|Totaal|Het aantal berichten niet worden bezorgd omdat ze niet compatibel met het eindpunt|
|d2c.telemetry.egress.fallback|Berichten die overeenkomt met alternatieve voorwaarde|Count|Totaal|Aantal berichten dat is geschreven naar het eindpunt van de terugval|
|d2c.endpoints.egress.eventHubs|Berichten die worden geleverd aan Event Hub-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Event Hub-eindpunten|
|d2c.endpoints.latency.eventHubs|Bericht latentie voor Event Hub-eindpunten|Milliseconden|Gemiddeld|De gemiddelde latentie tussen bericht inkomend verkeer naar de IoT hub en bericht inkomend verkeer naar een Event Hub-eindpunt, in milliseconden|
|d2c.endpoints.egress.serviceBusQueues|Berichten die worden geleverd aan Service Bus-wachtrij-eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar Service Bus-wachtrij-eindpunten|
|d2c.endpoints.latency.serviceBusQueues|Latentie van het bericht voor eindpunten van de Service Bus-wachtrij|Milliseconden|Gemiddeld|De gemiddelde latentie tussen bericht inkomend verkeer naar de IoT hub en bericht inkomend verkeer in een Service Bus-wachtrij-eindpunt, in milliseconden|
|d2c.endpoints.egress.serviceBusTopics|Berichten die worden geleverd aan Service Bus-onderwerp eindpunten|Count|Totaal|Aantal keren dat berichten zijn geschreven naar de eindpunten van de Service Bus-onderwerp|
|d2c.endpoints.latency.serviceBusTopics|Latentie van het bericht voor eindpunten van de Service Bus-onderwerp|Milliseconden|Gemiddeld|De gemiddelde latentie tussen bericht inkomend verkeer naar de IoT hub en bericht inkomend verkeer naar een eindpunt van de Service Bus-onderwerp, in milliseconden|
|d2c.endpoints.egress.builtIn.events|Berichten die worden geleverd aan het ingebouwde eindpunt (berichten/gebeurtenissen)|Count|Totaal|Aantal keren dat berichten zijn geschreven naar het eindpunt van de ingebouwde (berichten/gebeurtenissen)|
|d2c.endpoints.latency.builtIn.events|Bericht latentie voor het ingebouwde eindpunt (berichten/gebeurtenissen)|Milliseconden|Gemiddeld|De gemiddelde latentie tussen bericht inkomend verkeer naar de IoT hub en bericht inkomend verkeer in het geïntegreerde eindpunt (berichten/gebeurtenissen), in milliseconden |
|d2c.twin.read.success|Geslaagde dubbele leesbewerkingen van apparaten|Count|Totaal|Het aantal voltooide dubbele apparaat geïnitieerde leesbewerkingen.|
|d2c.Twin.Read.failure|Dubbele leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle dubbele apparaat geïnitieerde leesbewerkingen mislukt.|
|d2c.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde apparaat geïnitieerde twin leesbewerkingen.|
|d2c.Twin.update.Success|Geslaagde apparaatdubbel werkt bij van apparaten|Count|Totaal|Het aantal voltooide apparaat geïnitieerde apparaatdubbel werkt.|
|d2c.Twin.update.failure|Apparaatdubbel werkt bij van apparaten is mislukt|Count|Totaal|De telling van alle mislukte dubbel apparaat geïnitieerde updates.|
|d2c.Twin.update.Size|Grootte van apparaatdubbel werkt bij van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde apparaat geïnitieerde twin updates.|
|c2d.Methods.Success|Geslaagde rechtstreekse methodeaanroepen|Count|Totaal|Het aantal voltooide rechtstreekse methodeaanroepen.|
|c2d.Methods.failure|Rechtstreekse methodeaanroepen is mislukt|Count|Totaal|De telling van alle mislukte aanroepen van de directe methode.|
|c2d.Methods.requestSize|Grootte van de aanvraag van een rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde aanvragen voor directe methode.|
|c2d.Methods.responseSize|Reactiegrootte van rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde antwoorden in de directe methode.|
|c2d.twin.read.success|Geslaagde dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart dubbele leesbewerkingen.|
|c2d.twin.read.failure|Mislukte dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle back-end-gestart dubbele leesbewerkingen mislukt.|
|c2d.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde back-end-gestart twin leesbewerkingen.|
|c2d.Twin.update.Success|Geslaagde apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart apparaatdubbel werkt.|
|c2d.Twin.update.failure|Mislukte apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle mislukte back-end-gestart dubbele updates.|
|c2d.Twin.update.Size|Grootte van apparaatdubbel werkt bij vanaf back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde back-end-gestart twin updates.|
|twinQueries.success|Geslaagde apparaatdubbel-query 's|Count|Totaal|De telling van alle geslaagde apparaatdubbel-query's.|
|twinQueries.failure|Mislukte apparaatdubbel-query 's|Count|Totaal|De telling van alle mislukte apparaatdubbel-query's.|
|twinQueries.resultSize|Grootte van apparaatdubbel-query 's|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van de grootte van het resultaat van alle geslaagde apparaatdubbel-query's.|
|jobs.createTwinUpdateJob.success|Geslaagde bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle is gemaakt van dubbele update taken.|
|jobs.createTwinUpdateJob.failure|Mislukte bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle mislukte aanmaak van dubbele taken bijwerken.|
|jobs.createDirectMethodJob.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle is gemaakt van een rechtstreekse methode aanroepen van taken.|
|jobs.createDirectMethodJob.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle mislukte aanmaak van rechtstreekse methode aanroepen van taken.|
|jobs.listJobs.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|Het aantal geslaagde aanroepen naar de lijst met taken.|
|jobs.listJobs.failure|Mislukte aanroepen naar de lijst met taken|Count|Totaal|Het aantal mislukte aanroepen naar de lijst met taken.|
|jobs.cancelJob.success|Geslaagde taakannuleringen|Count|Totaal|Het aantal geslaagde aanroepen naar een taak annuleren.|
|jobs.cancelJob.failure|Mislukte taakannuleringen|Count|Totaal|Het aantal mislukte aanroepen naar een taak annuleren.|
|jobs.queryJobs.success|Taak query 's|Count|Totaal|Het aantal geslaagde aanroepen naar query taken.|
|jobs.queryJobs.failure|Taak query's is mislukt|Count|Totaal|Het aantal mislukte aanroepen naar query taken.|
|jobs.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|
|jobs.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|

## <a name="next-steps"></a>Volgende stappen
Nu u een overzicht van metrische gegevens van IoT Hub hebt gezien, volgt u deze koppeling voor meer informatie over het beheren van Azure IoT Hub:

* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
