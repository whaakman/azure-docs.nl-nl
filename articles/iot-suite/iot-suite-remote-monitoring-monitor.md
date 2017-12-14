---
title: Geavanceerde bewaking in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe u apparaten met het dashboard externe controle oplossing controleren.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 2e6d965d9177a61f974b319a1bd2155c9132533f
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Geavanceerde bewaking met behulp van de oplossing voor externe controle uitvoeren

Deze zelfstudie ziet de mogelijkheden van het dashboard voor externe controle. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

In deze zelfstudie maakt u twee gesimuleerde Contoso vrachtwagen apparaten gebruiken voor informatie over het bewaken van uw apparaten vanuit het dashboard van de vooraf geconfigureerde oplossing. Als een Contoso-operator moet u de locatie en het gedrag van uw vrachtwagens in het veld bewaken.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * De apparaten in het dashboard filteren
> * Realtime telemetrie van paginaweergaven
> * Details van de apparaten weergeven
> * Alarmen van uw apparaten weergeven
> * Het systeem KPI's weergeven

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-deploy.md) zelfstudie.

## <a name="choose-the-devices-to-display"></a>Kiezen welke apparaten om weer te geven

Selecteer welke apparaten weergegeven op de **Dashboard** pagina, filters gebruiken. Om weer te geven alleen de **vrachtwagen** apparaten, kiest u de ingebouwde **vrachtwagens** filter in de vervolgkeuzelijst filter:

![Filter voor vrachtwagens op het dashboard](media/iot-suite-remote-monitoring-monitor/dashboardtruckfilter.png)

Wanneer u een filter toepassen, alleen de apparaten die voldoen aan de filtervoorwaarden weergeven in de kaart op de **Dashboard** pagina:

![Vrachtwagens worden weergegeven op de kaart](media/iot-suite-remote-monitoring-monitor/dashboardtruckmap.png)

Het filter bepaalt ook welke apparaten die u ziet in de **telemetrie** grafiek:

![Vrachtwagen telemetrie wordt weergegeven op het dashboard](media/iot-suite-remote-monitoring-monitor/dashboardtelemetry.png)

Als u wilt maken, bewerken, en filters verwijderen of kies **filters beheren**.

## <a name="view-real-time-telemetry"></a>Realtime telemetrie van paginaweergaven

De vooraf geconfigureerde oplossing gedetailleerde realtime telemetrische gegevens in de grafiek getekend op de **Dashboard** pagina. De telemetrie-grafiek toont telemetrie-informatie voor de apparaten die zijn geselecteerd door de huidige filter:

![Vrachtwagen telemetrie tekent](media/iot-suite-remote-monitoring-monitor/dashboardtelemetryview.png)

Selecteer de telemetrie-waarden om te bekijken, kies het type telemetrie aan de bovenkant van de grafiek:

![Vrachtwagen telemetrie tekent](media/iot-suite-remote-monitoring-monitor/dashboardselecttelemetry.png)

Kies de weergave live telemetrie onderbreken, **Flowing**. Als u wilt de live weergave opnieuw inschakelen, kiest u **onderbreken**:

![Onderbreken en hervatten van telemetrie weergeven](media/iot-suite-remote-monitoring-monitor/dashboardtelemetrypause.png)

## <a name="use-the-map"></a>Gebruik de kaart

De kaart bevat informatie over de gesimuleerde vrachtwagens geselecteerd door de huidige filterset. U kunt inzoomen en pannen van de kaart om locaties in meer of minder details weer te geven. De apparaatpictogrammen op de kaart geven een **alarmen** of **waarschuwingen** die voor het apparaat actief zijn. Een samenvatting van het aantal **alarmen** en **waarschuwingen** wordt weergegeven aan de linkerkant van de kaart.

Het om Apparaatdetails te bekijken, Pannen en zoomen van de kaart om te zoeken van de apparaten en klik vervolgens op het apparaat op de kaart. De volgende gegevens bevatten:

* Recente telemetrie waarden
* Methoden die worden ondersteund door het apparaat
* Apparaateigenschappen

![Details van de weergave-apparaat op het dashboard](media/iot-suite-remote-monitoring-monitor/dashboarddevicedetail.png)

## <a name="view-alarms-from-your-devices"></a>Alarmen van uw apparaten weergeven

De kaart markeert de apparaten in de huidige filterset met **alarmen** en **waarschuwingen**. De **System alarmen** Configuratiescherm bevat gedetailleerde informatie over de meest recente alarmen van uw apparaten:

![Weergave system alarmen op het dashboard](media/iot-suite-remote-monitoring-monitor/dashboardsystemalarms.png)

U kunt de **System alarmen** filter de tijdsduur voor een recente alarmen aanpassen. Standaard worden in het deelvenster alarmen vanuit het afgelopen uur weergegeven:

![De alarmen filteren op tijd](media/iot-suite-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>Het systeem KPI's weergeven

De **Dashboard** pagina system KPI's wordt weergegeven:

![De alarmen filteren op tijd](media/iot-suite-remote-monitoring-monitor/dashboardkpis.png)

U kunt de **System KPI** filter om aan te passen de tijdsduur voor de KPI-aggregatie. Standaard worden in het deelvenster KPI's tijdens het afgelopen uur geaggregeerd weergegeven.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe u de **Dashboard** pagina te filteren en te controleren van de gesimuleerde vrachtwagens ingericht in uw oplossing voor externe controle:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * De apparaten in het dashboard filteren
> * Realtime telemetrie van paginaweergaven
> * Details van de apparaten weergeven
> * Alarmen van uw apparaten weergeven
> * Het systeem KPI's weergeven

U kunt het bewaken van uw apparaten hebt geleerd, de voorgestelde volgende stappen zijn voor meer informatie over hoe:

* [Detecteer problemen met regels op basis van drempelwaarden](./iot-suite-remote-monitoring-automate.md).
* [Uw apparaten beheren en configureren](./iot-suite-remote-monitoring-manage.md).
* [Problemen oplossen en het oplossen van problemen met apparaat](./iot-suite-remote-monitoring-maintain.md).
* [Testen van uw oplossing met gesimuleerde apparaten](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->