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
ms.date: 08/09/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 597043b17993ebddc9cf730ddce849e1d6ff3bc9


---
# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Wat zijn de vooraf geconfigureerde Azure IoT Suite-oplossingen?
De vooraf geconfigureerde Azure IoT Suite-oplossingen zijn implementaties van algemene IoT-oplossingspatronen die u in Azure kunt implementeren met behulp van uw abonnement. U kunt de vooraf geconfigureerde oplossingen gebruiken:

* Als een beginpunt voor uw eigen IoT-oplossingen.
* Voor meer informatie over algemene patronen bij het ontwerpen en ontwikkelen van IoT-oplossingen.

Elke vooraf geconfigureerde oplossing is een volledige totaalimplementatie die gesimuleerde apparaten gebruikt om telemetrie te genereren.

U kunt niet alleen de oplossingen in Azure implementeren en uitvoeren, maar ook de volledige broncode downloaden en vervolgens de oplossing aanpassen en uitbreiden, zodat deze aan uw specifieke IoT-vereisten voldoet.

> [!NOTE]
> Als u een van de vooraf geconfigureerde oplossingen wilt implementeren, gaat u naar [Microsoft Azure IoT Suite][lnk-azureiotsuite]. In het artikel [Get started with the IoT preconfigured solutions][lnk-getstarted-preconfigured] (Aan de slag met de vooraf geconfigureerde IoT-oplossingen) vindt u meer informatie over het implementeren en uitvoeren van een van de oplossingen.
> 
> 

De volgende tabel toont u hoe de oplossingen aan specifieke IoT-functies zijn toegewezen:

| Oplossing | Gegevensopname | Apparaat-id | Opdracht en controle | Regels en acties | Predictive analytics |
| --- | --- | --- | --- | --- | --- |
| [Externe controle][lnk-getstarted-preconfigured] |Ja |Ja |Ja |Ja |- |
| [Voorspeld onderhoud][lnk-predictive-maintenance] |Ja |Ja |Ja |Ja |Ja |

* *Gegevensopname*: instroom van gegevens op de gewenste schaal in de cloud.
* *Apparaat-id*: beheer unieke id's van elk aangesloten apparaat.
* *Opdracht en controle*: verzend berichten naar een apparaat vanuit de cloud om het apparaat een actie te laten uitvoeren.
* *Regels en acties*: de back-end van de oplossing gebruikt regels om te reageren op specifieke apparaat-naar-cloudgegevens.
* *Predictive analytics*: de back-end van de oplossing analyseert apparaat-naar-cloudgegevens om te voorspellen wanneer bepaalde acties moeten plaatsvinden. Zo kan de telemetrie van vliegtuigmotoren worden geanalyseerd om te bepalen wanneer motoronderhoud is vereist.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Overzicht van vooraf geconfigureerde oplossing voor externe controle
We hebben ervoor gekozen om in dit artikel de vooraf geconfigureerde oplossing voor externe controle te bespreken omdat dit veel voorkomende ontwerpelementen illustreert die de andere oplossingen ook gebruiken.

Het volgende diagram illustreert de belangrijkste elementen van de oplossing voor externe controle. De volgende secties bevatten meer informatie over deze elementen.

![Architectuur van de vooraf geconfigureerde oplossing voor externe controle][img-remote-monitoring-arch]

## <a name="devices"></a>Apparaten
Wanneer u de vooraf geconfigureerde oplossing voor externe controle implementeert, zijn in de oplossing vooraf vier gesimuleerde apparaten ingericht die een koelapparaat simuleren. Deze gesimuleerde apparaten hebben een ingebouwd temperatuur- en vochtigheidsmodel dat telemetrie verzendt. Deze gesimuleerde apparaten zijn opgenomen ter illustratie van de end-to-endstroom van gegevens binnen de oplossing en als handige bron van telemetrie en als een doel voor opdrachten voor back-endontwikkelaars die de oplossing gebruiken als een beginpunt voor een aangepaste implementatie.

Wanneer in de vooraf geconfigureerde oplossing voor externe controle een apparaat voor het eerst verbinding maakt met IoT Hub, inventariseert het informatiebericht van het apparaat dat naar IoT Hub wordt verzonden de lijst met opdrachten waarop het apparaat kan reageren. In de vooraf geconfigureerde oplossing voor externe controle zijn de opdrachten: 

* *PingDevice*: Het apparaat reageert op deze opdracht met een bevestiging. Dit is handig om te controleren of het apparaat nog steeds actief is en luistert.
* *StartTelemetry*: hiermee geeft u het apparaat de opdracht om te beginnen met het verzenden van telemetrie.
* *StopTelemetry*: hiermee geeft u het apparaat de opdracht om te stoppen met het verzenden van telemetrie.
* *ChangeSetPointTemperature*: bepaalt de telemetriewaarden van de gesimuleerde temperatuur die het apparaat verzendt. Dit is nuttig voor het testen van back-endlogica.
* *DiagnosticTelemetry*: bepaalt of het apparaat de externe temperatuur als telemetrie moet verzenden.
* *ChangeDeviceState*: Stelt de eigenschap voor metagegeven van de apparaatstatus in die het apparaat rapporteert. Dit is nuttig voor het testen van back-endlogica.

U kunt aan de oplossing meer gesimuleerde apparaten toevoegen die dezelfde telemetrie verzenden en reageren op dezelfde opdrachten. 

## <a name="iot-hub"></a>IoT Hub
In deze vooraf geconfigureerde oplossing komt de IoT Hub-instantie overeen met de *cloudgateway* in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

IoT Hub ontvangt telemetrie van de apparaten op één eindpunt. IoT Hub onderhoudt ook apparaatspecifieke eindpunten waar elk apparaat de opdrachten kan ophalen die ernaar worden verzonden.

IoT Hub stelt de ontvangen telemetrie beschikbaar via het eindpunt voor het lezen van telemetrie aan servicezijde.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
De vooraf geconfigureerde oplossing maakt gebruik van drie [Azure Stream Analytics][lnk-asa] (ASA)-jobs voor het filteren van de telemetriestroom vanaf de apparaten:

* *De job DeviceInfo* voert gegevens uit naar een Event Hub die apparaatregistratiespecifieke berichten, die worden verzonden wanneer een apparaat voor het eerst verbinding maakt of als reactie op een opdracht **ChangeDeviceState**, doorstuurt naar het apparaatregister van de oplossing (een DocumentDB-database). 
* *De job Telemetry* verzendt alle onbewerkte telemetrie naar Azure Blob Storage voor koude opslag en berekent telemetrieaggregaties die op het dashboard van de oplossing worden weergegeven.
* *De job Rules* filtert de telemetriestroom op waarden die groter zijn dan regeldrempelwaarden en voert de gegevens uit naar een Event Hub. Wanneer een regel wordt gestart, wordt deze gebeurtenis in de dashboardweergave van de oplossingsportal weergegeven als een nieuwe rij in de tabel met de geschiedenis van waarschuwingen en wordt een actie gestart op basis van de instellingen die in de portal van de oplossing zijn gedefinieerd in de weergaven Regels en Acties.

In deze vooraf geconfigureerde oplossing maken de ASA-jobs deel uit van de **back-end van de IoT-oplossing** in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Gebeurtenisprocessor
In deze vooraf geconfigureerde oplossing maakt de gebeurtenisprocessor deel uit van de **back-end van de IoT-oplossing** in een typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

De ASA-jobs **DeviceInfo** en **Rules** verzenden hun uitvoer naar Event Hubs voor levering aan andere back-endservices. De oplossing maakt gebruik van een [EventPocessorHost][lnk-event-processor]-instantie, die in een [WebJob][lnk-web-job] wordt uitgevoerd, om de berichten van deze Event Hubs te lezen. **EventProcessorHost** gebruikt de **DeviceInfo**-gegevens om de apparaatgegevens in de DocumentDB-database bij te werken en gebruikt de **Rules**-gegevens om de logische app aan te roepen en de waarschuwingsweergave in de portal van de oplossing bij te werken.

## <a name="device-identity-registry-and-documentdb"></a>Register voor apparaat-id's en DocumentDB
Elke IoT Hub bevat een [register voor apparaat-id's][lnk-identity-registry] waarin apparaatsleutels worden opgeslagen. IoT Hub gebruikt deze informatie om apparaten te verifiëren; een apparaat moet zijn geregistreerd en over een geldige sleutel beschikken voordat het verbinding kan maken met de hub.

Deze oplossing bevat aanvullende informatie over apparaten zoals hun status, de opdrachten die ze ondersteunen, alsook andere metagegevens. De oplossing maakt gebruik van een DocumentDB-database voor het opslaan van deze oplossingsspecifieke apparaatgegevens. De oplossingsportal haalt gegevens uit deze DocumentDB-database op om deze weer te geven en te bewerken.

De oplossing moet ook de gegevens in het register voor apparaat-id's gesynchroniseerd houden met de inhoud van de DocumentDB-database. **EventProcessorHost** gebruikt de gegevens uit de Stream Analytics-job **DeviceInfo** om de synchronisatie te beheren.

## <a name="solution-portal"></a>Oplossingsportal
![Dashboard van de oplossing][img-dashboard]

De oplossingsportal is een webgebaseerde gebruikersinterface die naar de cloud wordt geïmplementeerd als onderdeel van de vooraf geconfigureerde oplossing. Hiermee kunt u:

* De telemetrie- en waarschuwingsgeschiedenis weergeven in een dashboard.
* Nieuwe apparaten inrichten.
* Apparaten beheren en controleren.
* Opdrachten naar specifieke apparaten verzenden.
* Regels en acties beheren.

In deze vooraf geconfigureerde oplossing maakt de oplossingsportal deel uit van de **back-end van de IoT-oplossing** en van de **verwerkings- en bedrijfsconnectiviteit** in de typische [IoT-oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Volgende stappen
Zie [Microsoft Azure IoT services: Reference Architecture][lnk-refarch] (Microsoft Azure IoT-services: referentiearchitectuur) voor meer informatie over IoT-oplossingsarchitecturen.

Nu u weet wat een vooraf geconfigureerde oplossing is, kunt u aan de slag met het implementeren van de vooraf geconfigureerde *externe controle* als oplossing: [Aan de slag met de vooraf geconfigureerde oplossingen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md



<!--HONumber=Nov16_HO2-->


