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
ms.openlocfilehash: b1c9854d794c88c28c82a4b6e40c81a29552223a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984058"
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

|Gegevens|De naam van de metrische gegevens weergeven|Eenheid|Aggregatietype|Beschrijving|Dimensies|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetrie-bericht verzenden pogingen|Count|Totaal|Aantal telemetrieberichten dat apparaat-naar-cloud heeft geprobeerd om te worden verzonden naar uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.ingress.Success|Berichten over telemetrie verzonden|Count|Totaal|Aantal telemetrieberichten dat is verzonden naar uw IoT hub apparaat-naar-cloud|Er zijn geen dimensies|
|c2d.Commands.egress.complete.Success|Opdrachten voltooid|Count|Totaal|Aantal opdrachten voor cloud-naar-apparaat is voltooid door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.abandon.success|Opdrachten afgebroken|Count|Totaal|Aantal cloud-naar-apparaatopdrachten afgebroken door het apparaat|Er zijn geen dimensies|
|c2d.commands.egress.reject.success|Opdrachten geweigerd|Count|Totaal|Aantal cloud-naar-apparaatopdrachten geweigerd door het apparaat|Er zijn geen dimensies|
|devices.totalDevices|Totaal aantal apparaten (afgeschaft)|Count|Totaal|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|devices.connectedDevices.allProtocol|Verbonden apparaten (afgeschaft) |Count|Totaal|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|d2c.telemetry.egress.success|Routering: telemetrieberichten geleverd|Count|Totaal|Het aantal keren dat berichten zijn bezorgd bij alle eindpunten met behulp van IoT Hub-routering. Als een bericht wordt doorgestuurd naar meerdere eindpunten, wordt deze waarde verhoogd met één voor elke geslaagde levering. Als een bericht wordt bezorgd in het hetzelfde eindpunt meerdere keren, wordt deze waarde verhoogd met één voor elke geslaagde levering.|Er zijn geen dimensies|
|d2c.telemetry.egress.dropped|Routering: telemetrieberichten verwijderd |Count|Totaal|Het aantal keren dat berichten door de IoT-Hub routering vanwege andere eindpunten zijn verwijderd. Deze waarde is, telt niet berichten die in de alternatieve route, zoals verwijderde berichten worden er niet bezorgd.|Er zijn geen dimensies|
|d2c.telemetry.egress.orphaned|Routering: telemetrieberichten zwevende |Count|Totaal|Het aantal keren dat berichten door de IoT Hub routering zwevende omdat ze niet overeenkomen met alle regels voor doorsturen (met inbegrip van de alternatieve regel). |Er zijn geen dimensies|
|d2c.telemetry.egress.Invalid|Routering: telemetrieberichten niet compatibel|Count|Totaal|Het aantal keren dat berichten worden verzonden vanwege compatibiliteitsproblemen met het eindpunt van de IoT-Hub routering kan niet. Deze waarde bevat geen nieuwe pogingen.|Er zijn geen dimensies|
|d2c.telemetry.egress.fallback|Routering: berichten worden afgeleverd bij terugval|Count|Totaal|Het aantal keren dat het IoT-Hub routering berichten naar het eindpunt dat is gekoppeld aan de alternatieve route geleverd.|Er zijn geen dimensies|
|d2c.endpoints.egress.eventHubs|Routering: berichten die naar Event Hub worden geleverd|Count|Totaal|Het aantal keren routering is IoT-Hub die berichten worden geleverd aan Event Hub-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.latency.eventHubs|Routering: bericht latentie voor Event Hub|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en bericht inkomend verkeer naar een Event Hub-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusQueues|Routering: berichten worden afgeleverd bij Service Bus-wachtrij|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten geleverd aan Service Bus-wachtrij-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusQueues|Routering: bericht latentie voor Service Bus-wachtrij|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-wachtrij-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.serviceBusTopics|Routering: berichten worden afgeleverd bij Service Bus-onderwerp|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan Service Bus-onderwerp eindpunten geleverd.|Er zijn geen dimensies|
|d2c.endpoints.latency.serviceBusTopics|Routering: bericht latentie voor Service Bus-onderwerp|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een Service Bus-onderwerp-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.builtIn.events|Routering: berichten worden afgeleverd bij berichten/gebeurtenissen|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan het ingebouwde eindpunt (berichten/gebeurtenissen) geleverd.|Er zijn geen dimensies|
|d2c.endpoints.latency.builtIn.events|Routering: bericht latentie voor berichten/gebeurtenissen|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer naar het eindpunt van de ingebouwde (berichten/gebeurtenissen).|Er zijn geen dimensies|
|d2c.endpoints.egress.Storage|Routering: berichten die naar de opslag worden geleverd|Count|Totaal|Het aantal keren dat het IoT Hub is routering berichten aan opslag eindpunten geleverd.|Er zijn geen dimensies|
|d2c.endpoints.Latency.Storage|Routering: bericht latentie voor opslag|Milliseconden|Gemiddeld|De gemiddelde latentie (in milliseconden) tussen bericht inkomend verkeer naar IoT Hub en telemetrie bericht inkomend verkeer in een storage-eindpunt.|Er zijn geen dimensies|
|d2c.endpoints.egress.Storage.bytes|Routering: gegevens geleverd aan opslag|Bytes|Totaal|De hoeveelheid gegevens (bytes) routering IoT-Hub die worden geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|d2c.endpoints.egress.Storage.blobs|Routering: blobs die worden geleverd aan opslag|Count|Totaal|Het aantal keren dat het IoT-Hub routering blobs geleverd aan opslag-eindpunten.|Er zijn geen dimensies|
|d2c.twin.read.success|Geslaagde dubbele leesbewerkingen van apparaten|Count|Totaal|Het aantal voltooide dubbele apparaat geïnitieerde leesbewerkingen.|Er zijn geen dimensies|
|d2c.Twin.Read.failure|Dubbele leesbewerkingen van apparaten is mislukt|Count|Totaal|De telling van alle dubbele apparaat geïnitieerde leesbewerkingen mislukt.|Er zijn geen dimensies|
|d2c.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde apparaat geïnitieerde twin leesbewerkingen.|Er zijn geen dimensies|
|d2c.Twin.update.Success|Geslaagde apparaatdubbel werkt bij van apparaten|Count|Totaal|Het aantal voltooide apparaat geïnitieerde apparaatdubbel werkt.|Er zijn geen dimensies|
|d2c.Twin.update.failure|Apparaatdubbel werkt bij van apparaten is mislukt|Count|Totaal|De telling van alle mislukte dubbel apparaat geïnitieerde updates.|Er zijn geen dimensies|
|d2c.Twin.update.Size|Grootte van apparaatdubbel werkt bij van apparaten|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde apparaat geïnitieerde twin updates.|Er zijn geen dimensies|
|c2d.Methods.Success|Geslaagde rechtstreekse methodeaanroepen|Count|Totaal|Het aantal voltooide rechtstreekse methodeaanroepen.|Er zijn geen dimensies|
|c2d.Methods.failure|Rechtstreekse methodeaanroepen is mislukt|Count|Totaal|De telling van alle mislukte aanroepen van de directe methode.|Er zijn geen dimensies|
|c2d.Methods.requestSize|Grootte van de aanvraag van een rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde aanvragen voor directe methode.|Er zijn geen dimensies|
|c2d.Methods.responseSize|Reactiegrootte van rechtstreekse methodeaanroepen|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van alle geslaagde antwoorden in de directe methode.|Er zijn geen dimensies|
|c2d.twin.read.success|Geslaagde dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart dubbele leesbewerkingen.|Er zijn geen dimensies|
|c2d.twin.read.failure|Mislukte dubbele leesbewerkingen van back-end|Count|Totaal|De telling van alle back-end-gestart dubbele leesbewerkingen mislukt.|Er zijn geen dimensies|
|c2d.twin.read.size|Reactiegrootte van dubbele leesbewerkingen van back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale van alle geslaagde back-end-gestart twin leesbewerkingen.|Er zijn geen dimensies|
|c2d.Twin.update.Success|Geslaagde apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle geslaagde back-end-gestart apparaatdubbel werkt.|Er zijn geen dimensies|
|c2d.Twin.update.failure|Mislukte apparaatdubbel werkt bij vanaf back-end|Count|Totaal|De telling van alle mislukte back-end-gestart dubbele updates.|Er zijn geen dimensies|
|c2d.Twin.update.Size|Grootte van apparaatdubbel werkt bij vanaf back-end|Bytes|Gemiddeld|De gemiddelde, de minimale en het maximale grootte van alle geslaagde back-end-gestart twin updates.|Er zijn geen dimensies|
|twinQueries.success|Geslaagde apparaatdubbel-query 's|Count|Totaal|De telling van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries.failure|Mislukte apparaatdubbel-query 's|Count|Totaal|De telling van alle mislukte apparaatdubbel-query's.|Er zijn geen dimensies|
|twinQueries.resultSize|Grootte van apparaatdubbel-query 's|Bytes|Gemiddeld|De gemiddelde, minimale en maximale van de grootte van het resultaat van alle geslaagde apparaatdubbel-query's.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.success|Geslaagde bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle is gemaakt van dubbele update taken.|Er zijn geen dimensies|
|jobs.createTwinUpdateJob.failure|Mislukte bewerkingen voor het maken van dubbele taken bijwerken|Count|Totaal|De telling van alle mislukte aanmaak van dubbele taken bijwerken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.success|Geslaagde bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle is gemaakt van een rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs.createDirectMethodJob.failure|Mislukte bewerkingen voor het maken van de methode aanroepen van taken|Count|Totaal|De telling van alle mislukte aanmaak van rechtstreekse methode aanroepen van taken.|Er zijn geen dimensies|
|jobs.listJobs.success|Geslaagde aanroepen naar de lijst met taken|Count|Totaal|Het aantal geslaagde aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.listJobs.failure|Mislukte aanroepen naar de lijst met taken|Count|Totaal|Het aantal mislukte aanroepen naar de lijst met taken.|Er zijn geen dimensies|
|jobs.cancelJob.success|Geslaagde taakannuleringen|Count|Totaal|Het aantal geslaagde aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs.cancelJob.failure|Mislukte taakannuleringen|Count|Totaal|Het aantal mislukte aanroepen naar een taak annuleren.|Er zijn geen dimensies|
|jobs.queryJobs.success|Taak query 's|Count|Totaal|Het aantal geslaagde aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.queryJobs.failure|Taak query's is mislukt|Count|Totaal|Het aantal mislukte aanroepen naar query taken.|Er zijn geen dimensies|
|jobs.completed|Voltooide taken|Count|Totaal|De telling van alle voltooide taken.|Er zijn geen dimensies|
|jobs.failed|Mislukte taken|Count|Totaal|De telling van alle mislukte taken.|Er zijn geen dimensies|
|d2c.telemetry.ingress.sendThrottle|Aantal beperkingsfouten|Count|Totaal|Aantal beperkingsfouten vanwege apparaat doorvoer beperkt|Er zijn geen dimensies|
|dailyMessageQuotaUsed|Totaal aantal berichten dat is gebruikt|Count|Gemiddeld|Het aantal totaal aantal berichten momenteel gebruikt. Dit is een cumulatieve waarde die is ingesteld op nul om 00:00 UTC elke dag.|Er zijn geen dimensies|
|deviceDataUsage|Totaal aantal devicedata gebruik (verouderd)|Bytes|Totaal|Bytes overgebracht naar en van alle apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|deviceDataUsageV2|Totaal aantal apparaat gegevensgebruik (preview)|Bytes|Totaal|Bytes overgebracht naar en van alle apparaten die zijn verbonden met IotHub|Er zijn geen dimensies|
|totalDeviceCount|Totaal aantal apparaten (preview)|Count|Gemiddeld|Aantal apparaten die zijn geregistreerd met uw IoT hub|Er zijn geen dimensies|
|connectedDeviceCount|Verbonden apparaten (preview)|Count|Gemiddeld|Aantal apparaten dat is verbonden met uw IoT-hub|Er zijn geen dimensies|
|Configuraties|Configuratie van metrische gegevens|Count|Totaal|Metrische gegevens voor configuratiebewerkingen|Er zijn geen dimensies|

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
