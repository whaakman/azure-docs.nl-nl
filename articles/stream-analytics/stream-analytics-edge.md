---
title: Azure Stream Analytics op IoT rand (preview)
description: Edge-taken maken in Azure Stream Analytics en deze implementeren naar apparaten gestart Azure IoT rand.
keywords: gegevensstroom, iot, rand
services: stream-analytics
documentationcenter: 
author: jseb225
manager: jhubbard
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/16/2017
ms.author: jeanb
ms.openlocfilehash: f1ff8d6f64a04ab03c8170fd2b6a7c881227da2e
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stream-analytics-on-iot-edge-preview"></a>Azure Stream Analytics op IoT rand (preview)

> [!IMPORTANT]
> Deze functionaliteit is in preview. We raden niet gebruiken in productie.
 
Azure Stream Analytics (ASA) op de rand IoT machtigt ontwikkelaars near-realtime analytische intelligence dichter op IoT-apparaten implementeren, zodat de volledige meerwaarde van apparaat gegenereerde gegevens kan worden ontgrendeld. Ontworpen voor lage latentie, tolerantie, bandbreedte en naleving efficiënt worden gebruikt, kunnen ondernemingen nu implementeren besturingselement logica dicht bij de industriële bewerkingen en een aanvulling vormen op Big Data-analyses uitgevoerd in de cloud.  
Azure Stream Analytics op IoT-rand wordt uitgevoerd in de [Azure IoT rand](https://azure.microsoft.com/campaigns/iot-edge/) framework. Nadat de taak is gemaakt in ASA, deploym en beheren van de ASA-jobs met IoT Hub.
Deze functie is een Preview-versie, als u vragen of feedback hebt kunt u [deze enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) contact opnemen met het productteam. 

## <a name="scenarios"></a>Scenario's
![Diagram op hoog niveau](media/stream-analytics-edge/ASAedge_highlevel.png)

* **Lage latentie opdracht en controle**: bijvoorbeeld veiligheid productiesystemen moet reageren op de operationele gegevens met een zeer lage latentie. Met ASA over IoT-zijde, kunt u sensor, gegevens in near-realtime en opdrachten uitgeven wanneer u detecteren afwijkingen is vereist om een machine stoppen of trigger waarschuwingen te analyseren.
*   **De verbinding met de cloud beperkt**: missie kritieke systemen zoals externe analysemodel apparatuur, verbonden vaartuigen of drijvende analyseren hoeft te analyseren en reageren op gegevens, zelfs wanneer de connectiviteit van de cloud wordt onderbroken. Uw streaming logica wordt uitgevoerd onafhankelijk van de netwerkverbinding met ASA, en u kunt kiezen wat u verzenden naar de cloud voor verdere verwerking of opslag.
* **Beperkte bandbreedte**: de hoeveelheid gegevens die wordt geproduceerd door jet-engines of verbonden auto's kunnen zo groot dat gegevens moeten worden gefilterd of vooraf verwerken voordat deze naar de cloud verzonden. ASA gebruikt, kunt u filteren en samenvoegen van de gegevens die moeten worden verzonden naar de cloud.
* **Naleving**: naleving van regelgeving kan vereisen sommige gegevens lokaal worden geanonimiseerde of geaggregeerd voordat ze worden verzonden naar de cloud.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Edge-taken in Azure Stream Analytics
### <a name="what-is-an-edge-job"></a>Wat is er een taak 'edge'?

ASA rand taken worden uitgevoerd als modules binnen [Azure IoT rand runtime](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works). Deze zijn samengesteld uit twee delen:
1.  Een cloud-onderdeel dat verantwoordelijk is voor de taakdefinitie: gebruikers invoer, uitvoer, query en andere instellingen (volgorde van gebeurtenissen, enz.) definiëren in de cloud.
2.  De ASA op de rand van de IoT-module die wordt lokaal uitgevoerd. Het bevat de engine ASA complexe verwerking en de taakdefinitie ontvangt van de cloud. 

ASA maakt gebruik van IoT Hub rand taken op apparaten implementeren. Meer informatie over [IoT rand implementatie ziet u hier](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

![Edge-taak](media/stream-analytics-edge/ASAedge_job.png)


### <a name="installation-instructions"></a>Installatie-instructies
De stappen op hoog niveau worden in de volgende tabel beschreven. Meer details zijn opgegeven in de volgende secties.
|      |Stap   | Plaats     | Opmerkingen   |
| ---   | ---   | ---       |  ---      |
| 1   | **Een ASA edge-taak maken**   | Azure Portal      |  Maak een nieuwe taak, selecteer **rand** als **hostomgeving**. <br> Deze taken zijn gemaakt/beheerd vanuit de cloud en uitvoeren op uw eigen IoT Edge-apparaten.     |
| 2   | **Een opslagcontainer maken**   | Azure Portal       | Storage-containers worden gebruikt voor het opslaan van de taakdefinitie van de waar ze toegankelijk zijn voor uw IoT-apparaten. <br>  U kunt een bestaande opslagcontainer hergebruiken.     |
| 3   | **Uw omgeving zijde van IoT op uw apparaten instellen**   | Appara(a)t(en)      | Instructies voor het [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) of [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).          |
| 4   | **ASA implementeren op de rand van de IoT-apparaten**   | Azure Portal      |  De taakdefinitie ASA wordt geëxporteerd naar de storage-container die eerder hebt gemaakt.       |
U kunt volgen [deze stapsgewijze zelfstudie](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) voor het implementeren van uw eerste baan ASA op IoT rand. Aan de hand van de volgende video moet u inzicht in het proces is een Stream Analytics-taak uitgevoerd op een apparaat van de rand IoT:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]



#### <a name="create-an-asa-edge-job"></a>Een Edge ASA-taak maken
1. Maak een nieuwe 'Stream Analytics-taak' vanuit de Azure-portal. [Directe koppeling te maken van een nieuwe ASA-taak hier](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

> [!Note]
> U kunt de Edge-taken maken in alle regio's wordt ondersteund door ASA, **behalve in 'VS-West 2' regio**.
> Deze beperking wordt binnenkort verwijderd.

2. Selecteer in het scherm voor het maken van **rand** als **hostomgeving** (Zie de volgende afbeelding) ![maken van de taak](media/stream-analytics-edge/ASAEdge_create.png)
3. Taakdefinitie
    1. **Definieer invoer Stream(s)**. Een of meer invoer stromen voor uw project definiëren.
    2. Definieer de referentiegegevens (optioneel).
    3. **Definieer uitvoer Stream(s)**. Een of meerdere uitgangen stromen voor uw project definiëren. 
    4. **Definieer query**. Definieer de ASA-query in de cloud met behulp van de inline-editor. De compiler wordt automatisch gecontroleerd of de syntaxis voor ASA rand ingeschakeld. U kunt ook uw query testen door voorbeeldgegevens te uploaden. 
4. Optionele instellingen instellen
    1. **Gebeurtenis ordening**. U kunt de volgorde out-beleid configureren in de portal. Documentatie is beschikbaar [hier](https://msdn.microsoft.com/library/azure/mt674682.aspx?f=255&MSPPError=-2147217396).
    2. **Landinstelling**. Stel de deur-indeling.


#### <a name="create-a-storage-container"></a>Een opslagcontainer maken
Storage-container is vereist voor het exporteren van de query ASA gecompileerd en de taakconfiguratie van de. Wordt gebruikt voor het configureren van de installatiekopie van het ASA Docker met uw specifieke query. 
1. Ga als volgt [deze instructies](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) een opslagaccount maken vanuit de Azure-portal. U kunt alle standaardopties voor dit account gebruiken met ASA houden.
2. Maak een blob storage-container in de zojuist gemaakte opslagaccount:
    1. Klik op 'Blobs', klikt u vervolgens '+ Container'. 
    2. Voer een naam en het behouden van de container als "Vertrouwelijk"


> [!Note]
> Wanneer een implementatie wordt gemaakt, exporteert ASA de taakdefinitie naar een opslagcontainer. Deze taakdefinitie blijven hetzelfde tijdens de duur van een implementatie. Als gevolg hiervan, als u bijwerken van een taak uitgevoerd op de rand wilt, moet u de taak in ASA bewerken en maak vervolgens een nieuwe implementatie van IoT-Hub.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>Uw omgeving zijde van IoT op uw apparaten instellen
Taken van de rand kunnen worden geïmplementeerd op apparaten met Azure IoT rand.
Hiervoor moet u als volgt te werk:
- Een Iot-Hub maken
- Docker- en IoT rand runtime installeren op uw randapparaten;
- Stel uw apparaten als 'Rand van de IoT-apparaten' in IoT-Hub.

Deze stappen worden beschreven in de rand van de IoT-documentatie voor [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) of [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux).  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>Implementatie ASA op de rand van de IoT-apparaten
##### <a name="add-asa-to-your-deployment"></a>ASA toevoegen aan uw implementatie
- In de Azure portal openen IoT Hub, navigeer naar IoT rand Explorer en open de blade van het apparaat.
- Selecteer **modules ingesteld**, selecteer daarna **importeren Azure IoT rand servicemodule**.
- Selecteer het abonnement en de rand van de ASA-taak die u hebt gemaakt. Selecteer vervolgens uw storage-account. Klik op opslaan.
![ASA-module in uw implementatie toevoegen](media/stream-analytics-edge/set_module.png)


> [!Note]
> Tijdens deze stap ASA vraagt om toegang tot de geselecteerde storage-container en maakt vervolgens een map met de naam 'EdgeJobs'. Voor elke implementatie wordt een nieuwe submap gemaakt in de map 'EdgeJobs'.
> ASA maakt om uw werk op de edge-apparaten implementeert, een shared access signature (SAS) voor het definitiebestand voor de taak. De SAS-sleutel worden veilig verzonden naar de rand van de IoT-apparaten met apparaat twin. De vervaldatum van deze sleutel is drie jaar na de dag van het maken ervan.


Zie voor meer informatie over IoT rand implementaties [deze pagina](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).


##### <a name="configure-routes"></a>Routes configureren
IoT-rand biedt een manier declaratief om berichten te routeren tussen modules en tussen modules en IoT Hub. De volledige syntaxis wordt beschreven [hier](https://docs.microsoft.com/azure/iot-edge/module-composition).
Namen van de invoer en uitvoer gemaakt in de ASA-taak kunnen worden gebruikt als eindpunten voor routering.  

###### <a name="example"></a>Voorbeeld
```
{
"routes": {                                              
    "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
    "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream", 
    "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")" 
}
}   

```
Dit voorbeeld toont de routes voor het scenario dat wordt beschreven in de volgende afbeelding. Deze bevat een edge-taak genaamd '**ASA**', met een invoer met de naam '**temperatuur**'en een uitvoer met de naam'**waarschuwing**'.
![Voorbeeld van Routering](media/stream-analytics-edge/RoutingExample.png)

In dit voorbeeld definieert de volgende routes:
- Elk bericht uit de **tempSensor** wordt verzonden naar de module met de naam **ASA** naar de invoer met de naam **temperatuur**,
- Alle uitvoerwaarden van **ASA** module met de IoT-Hub die is gekoppeld aan dit apparaat (upstream$), worden verzonden
- Alle uitvoerwaarden van **ASA** module worden verzonden naar de **besturingselement** eindpunt van de **tempSensor**.


## <a name="technical-information"></a>Technische informatie
### <a name="current-limitations-for-edge-jobs-compared-to-cloud-jobs"></a>Huidige beperkingen voor rand taken vergeleken met cloud-taken
Het doel is om pariteit tussen de rand van taken en in de cloud van taken. De meeste van de functies van onze SQL-querytaal worden al ondersteund.
Echter de volgende functies zijn nog niet ondersteund voor rand taken:
* Gebruiker gedefinieerde functies (UDF) en de gebruiker gedefinieerde aggregaties (UDA).
* Azure ML-functies.
* Met behulp van meer dan 14 statistische functies in één stap.
* AVRO-indeling voor invoer/uitvoer. Op dit moment worden alleen CSV en JSON ondersteund.
* Compressie van JSON-invoer.
* De volgende SQL-operators:
    * AnomalyDetection
    * Georuimtelijke operators:
        * CreatePoint
        * CreatePolygon
        * CreateLineString
        * ST_DISTANCE
        * ST_WITHIN
        * ST_OVERLAPS
        * ST_INTERSECTS
    * PARTITIONEREN
    * GetMetadataPropertyValue


### <a name="runtime-and-hardware-requirements"></a>Runtime-en hardwarevereisten
Als u wilt ASA op IoT rand uitvoeren, moet u de apparaten die kunnen worden uitgevoerd [Azure IoT rand](https://azure.microsoft.com/campaigns/iot-edge/). 

ASA en gebruiken van Azure IoT rand **Docker** containers naar een draagbaar bieden die wordt uitgevoerd op meerdere host-OS (Windows, Linux).

ASA op IoT-rand wordt beschikbaar gesteld als Windows- en Linux-installatiekopieën, uitgevoerd op zowel x86 64- of ARM-architecturen. 


### <a name="input-and-output"></a>Invoer en uitvoer
#### <a name="input-and-output-streams"></a>Invoer- en uitvoerstromen
Rand van de ASA-jobs krijgen invoer en uitvoer van andere modules dat wordt uitgevoerd op de IoT Edge-apparaten. Voor verbinding van en naar specifieke modules, kunt u de configuratie van de routering instellen tijdens de implementatie. Meer informatie wordt beschreven op [de rand van de IoT-module samenstelling documentatie](https://docs.microsoft.com/azure/iot-edge/module-composition).

Voor de invoer en uitvoer worden CSV en JSON-indelingen ondersteund.

Voor elk invoer en uitvoerstroom die u in uw taak ASA maakt, wordt een bijbehorende eindpunt op uw geïmplementeerde module gemaakt. Deze eindpunten kunnen worden gebruikt in de routes van uw implementatie.



##### <a name="reference-data"></a>Referentiegegevens
Referentiegegevens (ook wel bekend als een opzoektabel) is een beperkte verzameling die statisch of vertraging wijzigen in aard. Wordt gebruikt om een zoekopdracht of correleren met de gegevensstroom. Om ervoor te gebruiken van referentiegegevens in uw Azure Stream Analytics-taak, gebruikt u doorgaans een [verwijzing gegevens Join](https://msdn.microsoft.com/library/azure/dn949258.aspx) in uw Query. Zie voor meer informatie de [ASA-documentatie over referentiegegevens](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-use-reference-data).

Als u wilt gebruiken referentiegegevens voor ASA over Iot-zijde, moet u als volgt te werk: 
1. Een nieuwe invoer voor uw project maken
2. Kies **referentiegegevens** als de **brontype**.
3. Stel het bestandspad. Het bestandspad moet een **absolute** bestandspad op het apparaat ![verwijzen naar het maken van gegevens](media/stream-analytics-edge/ReferenceData.png)
4. Schakel **gedeelde stations** in de Docker-configuratie en zorg ervoor dat het station is ingeschakeld voordat u de implementatie start.

Zie voor meer informatie [Docker-documentatie voor Windows hier](https://docs.docker.com/docker-for-windows/#shared-drives).

> [!Note]
> Op dit moment wordt alleen lokale referentiegegevens ondersteund.




## <a name="license-and-third-party-notices"></a>Licenties en mededelingen van derden
* [Azure Stream Analytics op IoT rand preview licentie](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Kennisgeving van derden voor Azure Stream Analytics op IoT rand preview](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure Iot-zijde](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [ASA op de rand van de IoT-zelfstudie](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Feedback verzenden naar het team met behulp van deze enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
