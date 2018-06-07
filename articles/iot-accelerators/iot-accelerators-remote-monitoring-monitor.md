---
title: Geavanceerde bewaking in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe u apparaten met het dashboard externe controle oplossing controleren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 02/22/2018
ms.topic: conceptual
ms.openlocfilehash: 4d2dabd348d7fda4fa7ca3aac9975fd4179400c5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627396"
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Geavanceerde bewaking met behulp van de oplossing voor externe controle uitvoeren

Deze zelfstudie ziet de mogelijkheden van het dashboard externe controle. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

In deze zelfstudie maakt u twee gesimuleerde Contoso vrachtwagen apparaten gebruiken voor informatie over het bewaken van uw apparaten vanuit het dashboard van de accelerator oplossing. Als een Contoso-operator moet u de locatie en het gedrag van uw vrachtwagens in het veld bewaken.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * De apparaten in het dashboard filteren
> * Realtime telemetrie van paginaweergaven
> * Details van de apparaten weergeven
> * Waarschuwingen van uw apparaten weergeven
> * Het systeem KPI's weergeven

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren Remote Monitoring solution accelerator](iot-accelerators-remote-monitoring-deploy.md) zelfstudie.

## <a name="choose-the-devices-to-display"></a>Kiezen welke apparaten om weer te geven

Selecteer welke apparaten weergegeven op de **Dashboard** pagina, filters gebruiken. Om weer te geven alleen de **vrachtwagen** apparaten, kiest u de ingebouwde **vrachtwagens** filter in de vervolgkeuzelijst filter:

![Filter voor vrachtwagens op het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter.png)

Wanneer u een filter toepassen, alleen de apparaten die voldoen aan de filtervoorwaarden weergeven in de kaart op de **Dashboard** pagina:

![Vrachtwagens worden weergegeven op de kaart](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap.png)

Het filter bepaalt ook welke apparaten die u ziet in de **telemetrie** grafiek:

![Vrachtwagen telemetrie wordt weergegeven op het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry.png)

Als u wilt maken, bewerken, en filters verwijderen of kies **filters beheren**.

## <a name="view-real-time-telemetry"></a>Realtime telemetrie van paginaweergaven

De oplossingsverbetering gedetailleerde realtime telemetrische gegevens in de grafiek getekend op de **Dashboard** pagina. De telemetrie-grafiek toont telemetrie-informatie voor de apparaten die zijn geselecteerd door de huidige filter:

![Vrachtwagen telemetrie tekent](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview.png)

Selecteer de telemetrie-waarden om te bekijken, kies het type telemetrie aan de bovenkant van de grafiek:

![Vrachtwagen telemetrie tekent](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry.png)

<!-- 05/01 - this features appears to have been removed
To pause the live telemetry display, choose **Flowing**. To re-enable the live display, choose **Pause**:

![Pause and restart telemetry display](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetrypause.png)-->

## <a name="use-the-map"></a>Gebruik de kaart

De kaart bevat informatie over de gesimuleerde vrachtwagens geselecteerd door de huidige filterset. U kunt inzoomen en pannen van de kaart om locaties in meer of minder details weer te geven. De apparaatpictogrammen op de kaart geven een **waarschuwingen** of **waarschuwingen** die voor het apparaat actief zijn. Een samenvatting van het aantal **waarschuwingen** en **waarschuwingen** wordt weergegeven aan de linkerkant van de kaart.

<!-- 05/01 - cannot select a deice on the map
To view the device details, pan and zoom the map to locate the devices, then click the device on the map. The details include:

* Recent telemetry values
* Methods the device supports
* Device properties

![View device details on the dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail.png)-->

## <a name="view-alerts-from-your-devices"></a>Waarschuwingen van uw apparaten weergeven

De kaart markeert de apparaten in de huidige filterset met **waarschuwingen** en **waarschuwingen**. De **waarschuwingen** Configuratiescherm bevat gedetailleerde informatie over de meest recente waarschuwingen van uw apparaten:

![Systeem-waarschuwingen weergeven op het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms.png)

U kunt de **Dashboard** filter om aan te passen de tijdsduur voor een recente meldingen. Standaard worden in het deelvenster waarschuwingen van het afgelopen uur weergegeven:

![De waarschuwingen filteren op tijd](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>Het systeem KPI's weergeven

De **Dashboard** pagina system KPI's wordt weergegeven:

![Dashboard KPI 's](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis.png)

U kunt de **Dashboard** filter om aan te passen de tijdsduur voor de KPI-aggregatie. Standaard worden in het deelvenster KPI's tijdens het afgelopen uur geaggregeerd weergegeven.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe u de **Dashboard** pagina te filteren en te controleren van de gesimuleerde vrachtwagens ingericht in uw oplossing voor externe controle:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * De apparaten in het dashboard filteren
> * Realtime telemetrie van paginaweergaven
> * Details van de apparaten weergeven
> * Waarschuwingen van uw apparaten weergeven
> * Het systeem KPI's weergeven

U kunt het bewaken van uw apparaten hebt geleerd, de voorgestelde volgende stappen zijn voor meer informatie over hoe:

* [Detecteer problemen met regels op basis van drempelwaarden](iot-accelerators-remote-monitoring-automate.md).
* [Uw apparaten beheren en configureren](iot-accelerators-remote-monitoring-manage.md).
* [Problemen oplossen en het oplossen van problemen met apparaat](iot-accelerators-remote-monitoring-maintain.md).
* [Testen van uw oplossing met gesimuleerde apparaten](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->