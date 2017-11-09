---
title: Vooraf geconfigureerde Azure IoT-oplossingen | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde IoT Azure-oplossingen en hun architectuur, met koppelingen naar aanvullende resources.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 76df013e8e5868fcc9f5d95aa523a6a56dea7163
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Wat zijn de vooraf geconfigureerde Azure IoT Suite-oplossingen?

De vooraf geconfigureerde Azure IoT Suite-oplossingen zijn implementaties van algemene IoT-oplossingspatronen die u in Azure kunt implementeren met behulp van uw abonnement. U kunt de vooraf geconfigureerde oplossingen gebruiken:

* Als een beginpunt voor uw eigen IoT-oplossingen.
* Voor meer informatie over algemene patronen bij het ontwerpen en ontwikkelen van IoT-oplossingen.

Elke vooraf geconfigureerde oplossing is een volledige totaalimplementatie die gesimuleerde apparaten gebruikt om telemetrie te genereren.

U kunt de volledige broncode downloaden om de oplossing aan te passen en uit te breiden om te voldoen aan uw specifieke IoT-vereisten.

> [!NOTE]
> Als u een van de vooraf geconfigureerde oplossingen wilt implementeren, gaat u naar [Microsoft Azure IoT Suite][lnk-azureiotsuite]. In het artikel [Get started with the IoT preconfigured solutions][lnk-getstarted-preconfigured] (Aan de slag met de vooraf geconfigureerde IoT-oplossingen) vindt u meer informatie over het implementeren en uitvoeren van een van de oplossingen.

De volgende tabel toont u hoe de oplossingen aan specifieke IoT-functies zijn toegewezen:

| Oplossing | Gegevensopname | Apparaat-id | Apparaatbeheer | Opdracht en controle | Regels en acties | Predictive analytics |
| --- | --- | --- | --- | --- | --- | --- |
| [Externe bewaking][lnk-getstarted-preconfigured] |Ja |Ja |Ja |Ja |Ja |- |
| [Voorspeld onderhoud][lnk-predictive-maintenance] |Ja |Ja |- |Ja |Ja |Ja |
| [Verbonden factory][lnk-getstarted-factory] |Ja |Ja |Ja |Ja |Ja |- |

* *Gegevensopname*: instroom van gegevens op de gewenste schaal in de cloud.
* *Apparaat-id*: beheer unieke apparaat-id's en regel de toegang van apparaten tot de oplossing.
* *Apparaatbeheer*: beheer metagegevens van apparaten en voer bewerkingen uit, zoals het opnieuw opstarten van apparaten en firmware-upgrades.
* *Opdracht en controle*: verzend berichten naar een apparaat vanuit de cloud om het apparaat een actie te laten uitvoeren.
* *Regels en acties*: de back-end van de oplossing gebruikt regels om te reageren op specifieke apparaat-naar-cloudgegevens.
* *Predictive analytics*: de back-end van de oplossing analyseert apparaat-naar-cloudgegevens om te voorspellen wanneer bepaalde acties moeten plaatsvinden. Zo kan de telemetrie van vliegtuigmotoren worden geanalyseerd om te bepalen wanneer motoronderhoud is vereist.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Overzicht van vooraf geconfigureerde oplossing voor externe controle

We hebben ervoor gekozen om in dit artikel de vooraf geconfigureerde oplossing voor externe controle te bespreken omdat dit veel voorkomende ontwerpelementen illustreert die de andere oplossingen ook gebruiken.

Het volgende diagram illustreert de belangrijkste elementen van de oplossing voor externe controle. De volgende secties bevatten meer informatie over deze elementen.

![Architectuur van de vooraf geconfigureerde oplossing voor externe controle][img-remote-monitoring-arch]

## <a name="devices"></a>Apparaten

Wanneer u de vooraf geconfigureerde oplossing voor externe controle implementeert, zijn in de oplossing vooraf vier gesimuleerde apparaten ingericht die een koelapparaat simuleren. Deze gesimuleerde apparaten hebben een ingebouwd temperatuur- en vochtigheidsmodel dat telemetrie verzendt. Deze gesimuleerde apparaten zijn opgenomen om:

- De end-to-end-stroom van gegevens via de oplossing te illustreren.
- Een handige bron van telemetrie te verstrekken.
- Een doel te verstrekken voor methoden of opdrachten als u een back-endontwikkelaar bent die de oplossing gebruikt als uitgangspunt om een aangepaste implementatie te maken.

De gesimuleerde apparaten in de oplossing kunnen reageren op de volgende communicaties van cloud naar apparaat:

- *Methoden ([directe methoden][lnk-direct-methods])*: een methode voor communicatie in twee richtingen, waarbij van een verbonden apparaat wordt verwacht dat het direct reageert.
- *Opdrachten (berichten van cloud naar apparaat)*: een methode voor communicatie in één richting, waarbij een apparaat de opdracht ophaalt uit een duurzame wachtrij.

Zie [Cloud-to-device communications guidance][lnk-c2d-guidance] (Richtlijnen voor communicatie tussen cloud en apparaat) voor een vergelijking van deze verschillende methoden.

Wanneer een apparaat in de vooraf geconfigureerde oplossing voor het eerst verbinding maakt met IoT Hub, stuurt het apparaat een bericht met apparaatgegevens naar de hub. Dit bericht bevat met een opsomming van de methoden waarop het apparaat kan reageren. In de vooraf geconfigureerde oplossing voor externe controle ondersteunen gesimuleerde apparaten de volgende methoden:

* *InitiateFirmwareUpdate*: deze methode initieert een asynchrone taak op het apparaat om een firmware-update uit te voeren. De asynchrone taak gebruikt gerapporteerde eigenschappen om statusupdates aan het oplossingsdashboard te leveren.
* *Reboot*: deze methode zorgt ervoor dat het gesimuleerde apparaat opnieuw wordt opgestart.
* *FactoryReset*: deze methode zorgt ervoor dat de fabrieksinstellingen op het gesimuleerde apparaat worden teruggezet.

Wanneer een apparaat in de vooraf geconfigureerde oplossing voor het eerst verbinding maakt met IoT Hub, stuurt het apparaat een bericht met apparaatgegevens naar de hub. Dit bericht bevat met een opsomming van de opdrachten waarop het apparaat kan reageren. In de vooraf geconfigureerde oplossing voor externe controle ondersteunen gesimuleerde apparaten de volgende opdrachten:

* *PingDevice*: Het apparaat reageert op deze opdracht met een bevestiging. Deze opdracht is handig om te controleren of het apparaat nog steeds actief is en luistert.
* *StartTelemetry*: hiermee geeft u het apparaat de opdracht om te beginnen met het verzenden van telemetrie.
* *StopTelemetry*: hiermee geeft u het apparaat de opdracht om te stoppen met het verzenden van telemetrie.
* *ChangeSetPointTemperature*: bepaalt de telemetriewaarden van de gesimuleerde temperatuur die het apparaat verzendt. Deze opdracht is handig voor het testen van back-endlogica.
* *DiagnosticTelemetry*: bepaalt of het apparaat de externe temperatuur als telemetrie moet verzenden.
* *ChangeDeviceState*: stelt de eigenschap voor metagegevens van de apparaatstatus in die met het apparaat wordt gerapporteerd. Deze opdracht is handig voor het testen van back-endlogica.

U kunt aan de oplossing meer gesimuleerde apparaten toevoegen die dezelfde telemetrie verzenden en reageren op dezelfde methoden en opdrachten.

Naast het reageren op opdrachten en methoden maakt de oplossing gebruik van [apparaatdubbels][lnk-device-twin]. Apparaten gebruiken apparaatdubbels om eigenschapswaarden aan de back-end van de oplossing te rapporteren. Het oplossingsdashboard maakt gebruik van apparaatdubbels om nieuwe gewenste eigenschapswaarden op apparaten in te stellen. Tijdens het firmware-updateproces meldt het gesimuleerde apparaat bijvoorbeeld de status van de update met behulp van gerapporteerde eigenschappen.

## <a name="iot-hub"></a>IoT Hub

In deze vooraf geconfigureerde oplossing komt de IoT Hub-instantie overeen met de *cloudgateway* in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

IoT Hub ontvangt telemetrie van de apparaten op één eindpunt. IoT Hub onderhoudt ook apparaatspecifieke eindpunten waar met elk apparaat de verzonden opdrachten kunnen worden opgehaald.

IoT Hub stelt de ontvangen telemetrie beschikbaar via het eindpunt voor het lezen van telemetrie aan servicezijde.

Met de IoT Hub-functie voor apparaatbeheer kunt u de apparaateigenschappen beheren vanuit de oplossingsportal en taken plannen die bewerkingen uitvoeren, zoals:

- Apparaten opnieuw opstarten
- Apparaatstatussen wijzigen
- Firmware-updates

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

De vooraf geconfigureerde oplossing maakt gebruik van drie [Azure Stream Analytics][lnk-asa]-jobs (ASA) voor het filteren van de telemetriestroom vanaf de apparaten:

* *De job DeviceInfo* voert gegevens uit naar een Event Hub die apparaatregistratiespecifieke berichten doorstuurt naar het apparaatregister van de oplossing. Dit apparaatregister is een Azure Cosmos DB-database. Deze berichten worden verzonden wanneer een apparaat de eerste keer verbinding maakt of reageert op een opdracht voor het **wijzigen van de apparaatstatus**.
* *De job Telemetry* verzendt alle onbewerkte telemetrie naar Azure Blob Storage voor koude opslag en berekent telemetrieaggregaties die op het dashboard van de oplossing worden weergegeven.
* *De job Rules* filtert de telemetriestroom op waarden die groter zijn dan regeldrempelwaarden en voert de gegevens uit naar een Event Hub. Wanneer een regel wordt gestart, wordt deze gebeurtenis in het portaldashboard van de oplossing weergegeven als een nieuwe rij in de alarmhistorietabel. Deze regels kunnen ook een actie activeren op basis van de instellingen die zijn gedefinieerd in de weergaven **Regels** en **Acties** van de oplossingsportal.

In deze vooraf geconfigureerde oplossing maken de ASA-jobs deel uit van de **back-end van de IoT-oplossing** in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Gebeurtenisprocessor

In deze vooraf geconfigureerde oplossing maakt de gebeurtenisprocessor deel uit van de **back-end van de IoT-oplossing** in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

De ASA-jobs **DeviceInfo** en **Rules** verzenden hun uitvoer naar Event Hubs voor levering aan andere back-endservices. De oplossing maakt gebruik van een [EventProcessorHost][lnk-event-processor]-instantie, die in een [WebJob][lnk-web-job] wordt uitgevoerd, om de berichten van deze Event Hubs te lezen. De **EventProcessorHost** gebruikt:
- De gegevens in **DeviceInfo** om de apparaatgegevens in de Azure Cosmos DB-database bij te werken.
- De gegevens in **Regels** om de logische app aan te roepen en de weergave Waarschuwingen in de portal van de oplossing bij te werken.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Register voor apparaat-id's, apparaatdubbel en Cosmos DB

Elke IoT Hub bevat een [register voor apparaat-id's][lnk-identity-registry] waarin apparaatsleutels worden opgeslagen. IoT Hub gebruikt deze informatie om apparaten te verifiëren; een apparaat moet zijn geregistreerd en over een geldige sleutel beschikken voordat het verbinding kan maken met de hub.

Een [apparaatdubbel][lnk-device-twin] is een JSON-document dat wordt beheerd door de IoT Hub. Een apparaatdubbel voor een apparaat bevat:

- Gerapporteerde eigenschappen die door het apparaat naar de hub zijn verzonden. U kunt deze eigenschappen in de oplossingsportal bekijken.
- Gewenste eigenschappen die u naar het apparaat wilt verzenden. U kunt deze eigenschappen in de oplossingsportal instellen.
- Tags die alleen in de apparaatdubbel bestaan en niet op het apparaat. U kunt deze tags gebruiken om lijsten van apparaten in de oplossingsportal te filteren.

Deze oplossing gebruikt apparaatdubbels om metagegevens van apparaten te beheren. De oplossing gebruikt ook een Cosmos DB-database. Hierin worden aanvullende oplossingsspecifieke apparaatgegevens opgeslagen, zoals de opdrachten die door elk apparaat worden ondersteund en de opdrachtgeschiedenis.

De oplossing moet ook de gegevens in het register voor apparaat-id's gesynchroniseerd houden met de inhoud van de Cosmos DB-database. **EventProcessorHost** gebruikt de gegevens uit de Stream Analytics-job **DeviceInfo** om de synchronisatie te beheren.

## <a name="solution-portal"></a>Oplossingsportal

![oplossingsportal][img-dashboard]

De oplossingsportal is een webgebaseerde gebruikersinterface die naar de cloud wordt geïmplementeerd als onderdeel van de vooraf geconfigureerde oplossing. Hiermee kunt u:

* De telemetrie- en waarschuwingsgeschiedenis weergeven in een dashboard.
* Nieuwe apparaten inrichten.
* Apparaten beheren en controleren.
* Opdrachten naar specifieke apparaten verzenden.
* Methoden aanroepen op specifieke apparaten.
* Regels en acties beheren.
* Taken plannen die op een of meer apparaten moeten worden uitgevoerd.

In deze vooraf geconfigureerde oplossing maakt de oplossingsportal deel uit van de **back-end van de IoT-oplossing** en van de **verwerkings- en bedrijfsconnectiviteit** in de typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Volgende stappen

Zie [Microsoft Azure IoT services: Reference Architecture][lnk-refarch] (Microsoft Azure IoT-services: referentiearchitectuur) voor meer informatie over IoT-oplossingsarchitecturen.

Nu u weet wat een vooraf geconfigureerde oplossing is, kunt u aan de slag met het implementeren van de vooraf geconfigureerde *externe controle* als oplossing: [Aan de slag met de vooraf geconfigureerde oplossingen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-v1-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-v1-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md
