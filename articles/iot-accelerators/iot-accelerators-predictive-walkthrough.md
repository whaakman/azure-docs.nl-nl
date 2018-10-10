---
title: Overzicht van de oplossingsversneller Voorspeld onderhoud - Azure | Microsoft Docs
description: Een overzicht van de oplossingsversneller voor Predictief onderhoud van Azure IoT.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 822e02d42be8ce516901190af4be579d13ac841d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888319"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Overzicht van de oplossingsversneller Voorspeld onderhoud

De oplossingsversneller Voorspeld onderhoud is een totaaloplossing voor een bedrijfsscenario die het punt voorspelt waarop naar verwachting een fout optreedt. U kunt deze oplossingsversneller proactief gebruiken voor activiteiten zoals het optimaliseren van onderhoud. In de oplossing worden belangrijke oplossingsversnellers van Azure IoT gecombineerd, zoals IoT Hub, Stream Analytics en een [Azure Machine Learning][lnk-machine-learning]-werkruimte. Deze werkruimte bevat een model, gebaseerd op een openbare verzameling voorbeeldgegevens, om de resterende bruikbare levensduur (RUL) van een vliegtuigmotor te voorspellen. De oplossing implementeert het IoT-bedrijfsscenario volledig als een beginpunt zodat u een oplossing kunt plannen en implementeren die voldoet aan uw eigen specifieke zakelijke vereisten.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de oplossingsversneller:

![][img-architecture]

De blauwe items zijn Azure-services die zijn ingericht in de regio waar u de oplossingsversneller hebt geïmplementeerd. De lijst met regio's waar u de oplossingsversneller kunt implementeren, wordt weergegeven op de pagina [Inrichting][lnk-azureiotsuite].

Het groene item is een gesimuleerd apparaat dat een vliegtuigmotor vertegenwoordigt. Meer informatie over deze gesimuleerde apparaten vindt u in het gedeelte [Gesimuleerde apparaten](#simulated-devices).

De grijze items vertegenwoordigen onderdelen waarmee mogelijkheden voor *apparaatbeheer* worden geïmplementeerd. In de huidige release van de oplossingsversneller Voorspeld onderhoud worden deze resources niet ingericht. Raadpleeg voor meer informatie over het beheer van apparaten, de [oplossingsverbetering voor externe controle][lnk-remote-monitoring].

## <a name="azure-resources"></a>Azure-resources

Navigeer in Azure Portal naar de resourcegroep met de naam van de oplossing die u hebt gekozen om de ingerichte resources weer te geven.

![Accelerator-resources][img-resource-group]

Wanneer u de oplossingsversneller inricht, krijgt u een e-mailbericht met een koppeling naar de Machine Learning-werkruimte. U kunt ook navigeren naar de Machine Learning-werkruimte van de [Microsoft Azure IoT-oplossingsversnellers] [ lnk-azureiotsuite] pagina voor de ingerichte oplossing. Op deze pagina is een tegel beschikbaar wanneer de oplossing de status **Gereed** heeft.

![Machine learning-model][img-machine-learning]

## <a name="simulated-devices"></a>Gesimuleerde apparaten

In de oplossingsversneller vertegenwoordigt een gesimuleerd apparaat een vliegtuigmotor. De oplossing is ingericht met twee motoren die aan één vliegtuig zijn toegewezen. Elke motor verzendt vier typen telemetrie: Sensor 9, Sensor 11, Sensor 14 en Sensor 15 leveren de benodigde gegevens waarmee het Machine Learning-model de RUL voor de motor berekent. Elk gesimuleerd apparaat verzendt de volgende telemetrieberichten naar IoT Hub:

*Aantal maal gebruikt*. Een cyclus vertegenwoordigt een voltooide vlucht met een duur van tussen de twee en de tien uur. Tijdens de vlucht worden elk half uur telemetriegegevens vastgelegd.

*Telemetrie*. Er zijn vier sensoren die motorkenmerken vertegenwoordigen. Deze sensoren worden doorgaans Sensor 9, Sensor 11 Sensor 14 en Sensor 15 genoemd. Deze vier sensoren leveren voldoende telemetrie om nuttige resultaten te verkrijgen van het RUL-model. Het model dat wordt gebruikt in de oplossingsversneller, wordt gemaakt op basis van een openbare gegevensset die echte motorsensorgegevens bevat. Zie [Cortana Intelligence Gallery Predictive Maintenance Template][lnk-cortana-analytics] (Cortana Intelligence Gallery-sjabloon voor voorspellend onderhoud) voor meer informatie over hoe het model wordt gemaakt op basis van de oorspronkelijke gegevensset.

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
Het Machine Learning-onderdeel maakt gebruikt van een model dat is afgeleid van gegevens die zijn verzameld bij echte vliegtuigmotoren. U kunt naar de Machine Learning-werkruimte navigeren vanaf de tegel van de oplossing op de pagina [azureiotsuite.com][lnk-azureiotsuite]. De tegel is beschikbaar wanneer de oplossing de status **Gereed** heeft.

De Azure Machine Learning-model is beschikbaar als een sjabloon om deze mogelijkheden werken op basis van telemetriegegevens die zijn verzameld via IoT-oplossing accelerators services weer te geven. Microsoft heeft ontwikkeld een [regressiemodel] [ lnk_regression_model] van een vliegtuigmotor op basis van de openbaar beschikbare gegevens<sup>\[1\]</sup>, en stapsgewijze instructies over het gebruik van het model.

De Azure IoT-oplossingsversneller Voorspeld onderhoud maakt gebruik van het regressiemodel dat op basis van deze sjabloon is gemaakt. Het model is geïmplementeerd in uw Azure-abonnement en is toegankelijk via een automatisch gegenereerde API. De oplossing omvat een subset met de testgegevens voor 4 (van in totaal 100) motoren en de 4 (van in totaal 21) gegevensstromen van sensoren. Deze gegevens leveren een accuraat resultaat op van het getrainde model.

*\[1\] A. Saxena en K. Goebel (2008). 'Turbofan Engine Degradation Simulation Data Set', NASA Ames Prognostics Data Repository (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*

## <a name="next-steps"></a>Volgende stappen
Nu u de belangrijke onderdelen van de oplossingsversneller Voorspeld onderhoud hebt gezien, wilt u deze misschien aanpassen.

U kunt ook enkele van de andere functies en mogelijkheden van de IoT-oplossingsversnellers bekijken:

* [Veelgestelde vragen over IoT-oplossingsversnellers][lnk-faq]
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
