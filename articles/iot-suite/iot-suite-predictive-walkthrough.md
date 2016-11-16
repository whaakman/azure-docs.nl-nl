---
title: Leidraad voor voorspeld onderhoud | Microsoft Docs
description: Een leidraad voor de vooraf geconfigureerde Azure IoT-oplossing voor voorspeld onderhoud.
services: 
suite: iot-suite
documentationcenter: 
author: aguilaaj
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2016
ms.author: araguila
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ba48381f2c16a56e2e3f187017fbdbae09544e77


---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Leidraad voor vooraf geconfigureerde oplossing voor voorspeld onderhoud
## <a name="introduction"></a>Inleiding
De vooraf geconfigureerde IoT-Suite-oplossing voor voorspeld onderhoud is een totaaloplossing voor een bedrijfsscenario die het punt voorspelt waarop naar verwachting een fout optreedt. U kunt deze vooraf geconfigureerde oplossing proactief gebruiken voor activiteiten zoals het optimaliseren van onderhoud. De oplossing combineert belangrijke Azure IoT Suite-services, waaronder een [Azure Machine Learning][lnk_machine_learning]-werkruimte. Deze werkruimte bevat experimenten, gebaseerd op een openbare verzameling voorbeeldgegevens, om de resterende bruikbare levensduur (RUL) van een vliegtuigmotor te voorspellen. De oplossing implementeert het IoT-bedrijfsscenario volledig als een beginpunt zodat u een oplossing kunt plannen en implementeren die voldoet aan uw eigen specifieke zakelijke vereisten.

## <a name="logical-architecture"></a>Logische architectuur
Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![][img-architecture]

De blauwe items zijn Azure-services die zijn ingericht op de locatie die u selecteert wanneer u de vooraf geconfigureerde oplossing inricht. U kunt de vooraf geconfigureerde oplossing inrichten in de regio's VS - oost, Noord-Europa of Oost-Azië.

Sommige resources zijn niet beschikbaar in de regio's waar u de vooraf geconfigureerde oplossing inricht. De oranje items in het diagram vertegenwoordigen de Azure-services die ingericht zijn in de dichtstbijzijnde beschikbare regio (Zuid-centraal VS, EU West of Zuidoost-Azië), afhankelijk van de geselecteerde regio.

Het groene item is een gesimuleerd apparaat dat een vliegtuigmotor vertegenwoordigt. Meer informatie over deze gesimuleerde apparaten vindt u in het volgende gedeelte.

De grijze items vertegenwoordigen onderdelen die mogelijkheden voor *apparaatbeheer* implementeren. In de huidige release van de vooraf geconfigureerde oplossing voor voorspeld onderhoud worden deze resources niet ingericht. Voor meer informatie over het beheer van apparaten leest u het artikel over de [vooraf geconfigureerde oplossing voor externe controle][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Gesimuleerde apparaten
In de vooraf geconfigureerde oplossing vertegenwoordigt een gesimuleerd apparaat een vliegtuigmotor. De oplossing is ingericht met twee motoren die aan één vliegtuig zijn toegewezen. Elke motor verzendt vier typen telemetrie: Sensor 9, Sensor 11, Sensor 14 en Sensor 15 leveren de benodigde gegevens waarmee het Machine Learning-model de resterende levensduur voor de motor berekent. Elk gesimuleerd apparaat verzendt de volgende telemetrieberichten naar IoT Hub:

*Aantal maal gebruikt*. Een cyclus vertegenwoordigt een uitgevoerde vlucht met een variabele lengte van 2 tot 10 uur, waarbij tijdens de vlucht elk half uur telemetriegegevens worden vastgelegd.

*Telemetrie*. Er zijn vier sensoren die motorkenmerken vertegenwoordigen. Deze sensoren worden doorgaans Sensor 9, Sensor 11 Sensor 14 en Sensor 15 genoemd. Deze 4 sensoren vertegenwoordigen voldoende telemetrie om bruikbare resultaten voor de resterende levensduur op te halen uit het Machine Learning-model. Dit model wordt gemaakt op basis van een openbare gegevensset die echte motorsensorgegevens bevat. Zie [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Cortana Intelligence Gallery-sjabloon voor voorspeld onderhoud) voor meer informatie over hoe het model wordt gemaakt op basis van de oorspronkelijke gegevensset.

De gesimuleerde apparaten kunnen de volgende opdrachten verwerken die vanaf een IoT Hub worden verzonden:

| Opdracht | Beschrijving |
| --- | --- |
| StartTelemetry |Bepaalt de status van de simulatie.<br/>Start het verzenden van telemetrie vanaf het apparaat |
| StopTelemetry |Bepaalt de status van de simulatie.<br/>Stopt het verzenden van telemetrie vanaf het apparaat |

IoT Hub biedt een bevestiging van apparaatopdrachten.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-job
**Job: Telemetrie** verwerkt de inkomende telemetriestroom van het apparaat met behulp van twee instructies. De eerste selecteert alle telemetrie van de apparaten en verzendt deze gegevens naar de Blob Storage waar ze in de web-app worden weergegeven. De tweede instructie berekent de gemiddelde sensorwaarden gedurende een sliding window van twee minuten en verzendt deze gegevens via Event Hub naar een **gebeurtenisprocessor**.

## <a name="event-processor"></a>Gebeurtenisprocessor
De **gebeurtenisverwerking** neemt de gemiddelde sensorwaarden voor een voltooide cyclus. Vervolgens worden deze waarden doorgegeven aan een API die het getrainde Machine Learning-model de resterende levensduur voor een motor laat berekenen.

## <a name="azure-machine-learning"></a>Azure Machine Learning
Zie [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Cortana Intelligence Gallery-sjabloon voor voorspeld onderhoud) voor meer informatie over hoe het model wordt gemaakt op basis van de oorspronkelijke gegevensset.

## <a name="lets-start-walking"></a>De eerste stappen
In deze sectie maakt u kennis met de onderdelen van de oplossing, wordt het beoogde gebruiksscenario beschreven en vindt u voorbeelden.

### <a name="predictive-maintenance-dashboard"></a>Dashboard voorspeld onderhoud
Deze pagina in de webtoepassing maakt gebruik van PowerBI JavaScript-besturingselementen (zie de [PowerBI-visuals repository][lnk-powerbi] (Power BI-opslagplaats voor visualisaties)) voor het visualiseren van:

* De uitvoergegevens van de Stream Analytics-jobs in Blob Storage.
* De resterende levensduur en aantal cycli per vliegtuigmotor.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Het gedrag van de cloudoplossing observeren
Navigeer in de Azure-portal naar de resourcegroep met de naam van de oplossing die u hebt gekozen om de ingerichte resources weer te geven.

![][img-resource-group]

Wanneer u de vooraf geconfigureerde oplossing inricht, krijgt u een e-mailbericht met een koppeling naar de Machine Learning-werkruimte. U kunt ook naar de Machine Learning-werkruimte navigeren vanaf de [azureiotsuite.com][lnk-azureiotsuite]-pagina voor de ingerichte oplossing wanneer deze de status **Gereed** heeft.

![][img-machine-learning]

In de oplossingsportal kunt u zien dat het voorbeeld is ingericht met vier gesimuleerde apparaten die twee vliegtuigen met twee motoren per vliegtuig vertegenwoordigen, elk met vier sensoren. Wanneer u voor het eerst naar de oplossingsportal navigeert, wordt de simulatie gestopt.

![][img-simulation-stopped]

Klik op **Simulatie starten** om de simulatie te starten, waarbij u de sensorgeschiedenis, de resterende levensduur, het aantal cycli en de geschiedenis van de resterende levensduur in het dashboard ziet verschijnen.

![][img-simulation-running]

Wanneer de resterende levensduur minder dan 160 is (een willekeurige drempel gekozen ter illustratie), verschijnt in de oplossingsportal een waarschuwingssymbool naast de weergegeven resterende levensduur en wordt de vliegtuigmotor geel gemarkeerd. Zoals u merkt, vertonen de waarden van de resterende levensduur een algemene neerwaartse trend, maar stijgen en dalen die waarden vaak. Dit gedrag wordt veroorzaakt door de verschillende lengten van de cycli en de nauwkeurigheid van het model.

![][img-simulation-warning]

Het duurt ongeveer 35 minuten voordat de volledige simulatie 148 cycli heeft voltooid. Na ongeveer 5 minuten wordt voor het eerst de drempelwaarde van 160 voor de resterende levensduur bereikt. De drempelwaarde voor beide motoren wordt na ongeveer 8 minuten bereikt.

De simulatie wordt uitgevoerd voor de volledige gegevensset voor 148 cycli en bepaalt dan het eindresultaat voor de resterende levensduur en het aantal cycli.

U kunt de simulatie op elk punt stoppen, maar wanneer u op **Simulatie starten** klikt, wordt de simulatie opnieuw vanaf het begin van de gegevensset uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de vooraf geconfigureerde oplossing voor voorspeld onderhoud hebt uitgevoerd, wilt u deze mogelijk wijzigen. Zie [Guidance on customizing preconfigured solutions][lnk-customize] (Instructies voor het aanpassen van vooraf geconfigureerde oplossingen) voor meer informatie.

Het TechNet-blogbericht [IoT Suite - Under The Hood - Predictive Maintenance](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) (IoT Suite - Achter de schermen - Voorspeld onderhoud) biedt aanvullende informatie over de vooraf geconfigureerde oplossing voor voorspeld onderhoud.

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Veelgestelde vragen over IoT Suite][lnk-faq]
* [Compleet nieuwe IoT-beveiliging][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Nov16_HO2-->


