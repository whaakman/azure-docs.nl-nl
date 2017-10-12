---
title: Leidraad voor voorspeld onderhoud | Microsoft Docs
description: Een leidraad voor de vooraf geconfigureerde Azure IoT-oplossing voor voorspeld onderhoud.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: a68a8fdc3976ade0d1036d5ed58c8b2eb6d32a5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Leidraad voor vooraf geconfigureerde oplossing voor voorspeld onderhoud

De vooraf geconfigureerde oplossing voor predictief onderhoud is een totaaloplossing voor een bedrijfsscenario die het punt voorspelt waarop naar verwachting een fout optreedt. U kunt deze vooraf geconfigureerde oplossing proactief gebruiken voor activiteiten zoals het optimaliseren van onderhoud. In de oplossing worden belangrijke Azure IoT Suite-services gecombineerd, zoals IoT Hub, Stream Analytics en een [Azure Machine Learning][lnk-machine-learning]-werkruimte. Deze werkruimte bevat een model, gebaseerd op een openbare verzameling voorbeeldgegevens, om de resterende bruikbare levensduur (RUL) van een vliegtuigmotor te voorspellen. De oplossing implementeert het IoT-bedrijfsscenario volledig als een beginpunt zodat u een oplossing kunt plannen en implementeren die voldoet aan uw eigen specifieke zakelijke vereisten.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![][img-architecture]

De blauwe items zijn Azure-services die zijn ingericht in de regio waar u de vooraf geconfigureerde oplossing hebt geïmplementeerd. De lijst met regio's waar u de vooraf geconfigureerde oplossing kunt implementeren, wordt weergegeven op de pagina [Inrichting][lnk-azureiotsuite].

Het groene item is een gesimuleerd apparaat dat een vliegtuigmotor vertegenwoordigt. Meer informatie over deze gesimuleerde apparaten vindt u in het volgende gedeelte.

De grijze items vertegenwoordigen onderdelen waarmee mogelijkheden voor *apparaatbeheer* worden geïmplementeerd. In de huidige release van de vooraf geconfigureerde oplossing voor voorspeld onderhoud worden deze resources niet ingericht. Raadpleeg de [vooraf geconfigureerde oplossing voor externe controle][lnk-remote-monitoring] voor meer informatie over apparaatbeheer.

## <a name="simulated-devices"></a>Gesimuleerde apparaten

In de vooraf geconfigureerde oplossing vertegenwoordigt een gesimuleerd apparaat een vliegtuigmotor. De oplossing is ingericht met twee motoren die aan één vliegtuig zijn toegewezen. Elke motor verzendt vier typen telemetrie: Sensor 9, Sensor 11, Sensor 14 en Sensor 15 leveren de benodigde gegevens waarmee het Machine Learning-model de RUL voor de motor berekent. Elk gesimuleerd apparaat verzendt de volgende telemetrieberichten naar IoT Hub:

*Aantal maal gebruikt*. Een cyclus vertegenwoordigt een voltooide vlucht met een duur van tussen de twee en de tien uur. Tijdens de vlucht worden elk half uur telemetriegegevens vastgelegd.

*Telemetrie*. Er zijn vier sensoren die motorkenmerken vertegenwoordigen. Deze sensoren worden doorgaans Sensor 9, Sensor 11 Sensor 14 en Sensor 15 genoemd. Deze vier sensoren leveren voldoende telemetrie om nuttige resultaten te verkrijgen van het RUL-model. Het model dat wordt gebruikt in de vooraf geconfigureerde oplossing, wordt gemaakt op basis van een openbare gegevensset die echte motorsensorgegevens bevat. Zie [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Cortana Intelligence Gallery-sjabloon voor voorspellend onderhoud) voor meer informatie over hoe het model wordt gemaakt op basis van de oorspronkelijke gegevensset.

De gesimuleerde apparaten kunnen de volgende opdrachten verwerken die zijn verzonden vanaf de IoT Hub in de oplossing:

| Opdracht | Beschrijving |
| --- | --- |
| StartTelemetry |Bepaalt de status van de simulatie.<br/>Start het verzenden van telemetrie vanaf het apparaat |
| StopTelemetry |Bepaalt de status van de simulatie.<br/>Stopt het verzenden van telemetrie vanaf het apparaat |

IoT Hub biedt een bevestiging van apparaatopdrachten.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-job

**Job: Telemetrie** verwerkt de inkomende telemetriestroom van het apparaat met behulp van twee instructies:

* De eerste selecteert alle telemetrie van de apparaten en verzendt deze gegevens naar Blob Storage. Hier worden ze weergegeven in de webtoepassing.
* De tweede instructie berekent de gemiddelde sensorwaarden gedurende een sliding window van twee minuten en verzendt deze gegevens via Event Hub naar een **gebeurtenisprocessor**.

## <a name="event-processor"></a>Gebeurtenisprocessor
De **gebeurtenisprocessorhost** wordt uitgevoerd in een Azure-webtaak. De **gebeurtenisverwerking** neemt de gemiddelde sensorwaarden voor een voltooide cyclus. Vervolgens worden deze waarden doorgegeven aan een API die het getrainde model de RUL voor een motor laat berekenen. De API wordt weergegeven met een Machine Learning-werkruimte die is ingericht als onderdeel van de oplossing.

## <a name="machine-learning"></a>Machine Learning
Het Machine Learning-onderdeel maakt gebruikt van een model dat is afgeleid van gegevens die zijn verzameld bij echte vliegtuigmotoren. U kunt naar de Machine Learning-werkruimte navigeren vanaf de tegel op de [azureiotsuite.com][lnk-azureiotsuite]-pagina voor de vooraf ingerichte oplossing. De tegel is beschikbaar wanneer de oplossing de status **Gereed** heeft.


## <a name="next-steps"></a>Volgende stappen
Nu u de belangrijke onderdelen van de oplossing voor voorspeld onderhoud hebt gezien, wilt u deze misschien aanpassen. Zie [Guidance on Customizing Preconfigured Solutions][lnk-customize] (Handleiding voor het aanpassen van vooraf geconfigureerde oplossingen).

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Veelgestelde vragen over IoT Suite][lnk-faq]
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/