---
title: Uw IoT-apparaten bewaken met een Azure-oplossing | Microsoft Docs
description: In deze zelfstudie leert u hoe u uw IoT-apparaten kunt bewaken met behulp van de oplossingsversneller voor externe bewaking.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5f42ed0fa5362959e5619f2d550ca1ae3711ed65
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097458"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Zelfstudie: Uw IoT-apparaten bewaken

In deze zelfstudie gebruikt u de oplossingsversneller voor externe bewaking om de verbonden IoT-apparaten te bewaken. Gebruik het dashboard van de oplossing om telemetrie, apparaatgegevens, waarschuwingen en KPI's weer te geven.

Om u te laten kennismaken met deze bewakingsfuncties, wordt in de zelfstudie gebruikgemaakt van twee simulatieapparaten voor vrachtwagens ('Trucks' in het voorbeeld hieronder). De trucks worden beheerd door een organisatie met de naam Contoso en zijn verbonden met de oplossingsversneller voor externe bewaking. Als Contoso-operator moet u de locatie en het gedrag van uw trucks in het veld bewaken.

In deze zelfstudie doet u het volgende:

>[!div class="checklist"]
> * De apparaten filteren in het dashboard
> * Telemetrie in realtime weergeven
> * Apparaatdetails weergeven
> * Meldingen van uw apparaten weergeven
> * De systeem-KPI's weergeven

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u een geïmplementeerd exemplaar van de oplossingsversneller voor externe bewaking in uw Azure-abonnement nodig.

Als u de oplossingsverbetering voor externe controle nog niet hebt geïmplementeerd, voltooit u eerst de snelstart [Een cloudoplossing voor externe controle implementeren](quickstart-remote-monitoring-deploy.md).

## <a name="choose-the-devices-to-display"></a>Kies de weer te geven apparaten

Gebruik filters om te selecteren welke verbonden apparaten worden weergegeven op de pagina **Dashboard**. Om alleen de **Truck**-apparaten weer te geven, kiest u het ingebouwde **Trucks**-filter in de vervolgkeuzelijst met filters:

[![Filteren op trucks in het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Wanneer u een filter toepast, worden alleen de apparaten die voldoen aan de filtervoorwaarden weergegeven op de kaart op de pagina **Dashboard**:

[![Alleen trucks worden weergegeven op de kaart](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

Het filter bepaalt ook welke apparaten u in de grafiek **Telemetrie** te zien krijgt:

[![Telemetrie van trucks wordt weergegeven op het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-expanded.png#lightbox)

Als u filters wilt maken, bewerken en verwijderen, kiest u **Apparaatgroepen beheren**.

## <a name="view-real-time-telemetry"></a>Telemetrie in realtime weergeven

De oplossingsversneller plot in realtime telemetrie in de grafiek op de pagina **Dashboard**. Boven in de telemetriegrafiek ziet u de beschikbare telemetrietypen voor de apparaten die met het huidige filter zijn geselecteerd:

[![Telemetrietypen voor trucks](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Als u temperatuurtelemetrie wilt weergeven, klikt u op **Temperatuur**:

[![Temperatuurtelemetrie voor trucks geplot](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="use-the-map"></a>Gebruik van de kaart

De kaart bevat informatie over de gesimuleerde trucks die met het huidige filter zijn geselecteerd. U kunt de kaart inzoomen en pannen om locaties met meer of minder details weer te geven. De kleur van een apparaatpictogram op de kaart geeft aan of er voor het apparaat **Meldingen** of **Waarschuwingen** actief zijn. Een samenvatting van het aantal **Meldingen** en **Waarschuwingen** wordt links van de kaart weergegeven.

Als u de apparaatdetails wilt bekijken, zoomt en pant u de kaart om naar het apparaat te zoeken en selecteert u het apparaat op de kaart. Klik vervolgens op het apparaatlabel om het deelvenster **Apparaatdetails** te openen. De apparaatdetails omvatten:

* Recente telemetriewaarden
* Methoden die door het apparaat worden ondersteund
* Apparaateigenschappen

[![Apparaatdetails weergeven in het dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Waarschuwingen weergeven

Het deelvenster **Waarschuwingen** geeft gedetailleerde informatie over de meest recente meldingen van uw apparaten weer:

[![Apparaatmeldingen in het dashboard bekijken](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

U kunt een filter gebruiken om de tijdsduur voor recente meldingen aan te passen. Standaard worden in het deelvenster waarschuwingen van het afgelopen uur weergegeven:

[![De meldingen filteren op tijd](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-expanded.png#lightbox)

## <a name="view-the-system-kpis"></a>De systeem-KPI's weergeven

De pagina **Dashboard** geeft systeem-KPI's weer die zijn berekend door de oplossingsversneller in het deelvenster **Analyse**:

[![Dashboard-KPI's](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

Het dashboard geeft drie KPI's weer voor de meldingen die door de huidige filters voor apparaten en tijdsduur zijn geselecteerd:

* Het aantal actieve meldingen voor de regels die de meeste meldingen hebben geactiveerd.
* Het deel van de meldingen op basis van apparaattype.
* Het percentage meldingen dat uit kritieke meldingen bestaat.

Dezelfde filters die de tijdsduur instellen voor meldingen en regelen welke apparaten worden weergegeven, bepalen hoe de KPI's worden samengevoegd. Standaard worden in het deelvenster KPI's weergegeven die in de loop van het afgelopen uur zijn samengevoegd.

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent verder te gaan naar de volgende zelfstudie, laat u de oplossingsversneller voor externe bewaking geïmplementeerd. Als u de kosten wilt verminderen voor het uitvoeren van de oplossingsversneller terwijl u deze niet gebruikt, kunt u de gesimuleerde apparaten in het deelvenster Instellingen stopzetten:

[![Telemetrie onderbreken](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-expanded.png#lightbox)

Wanneer u bent klaar om te beginnen met de volgende zelfstudie, kunt u de gesimuleerde apparaten opnieuw opstarten.

Als u de oplossingsversneller niet meer nodig hebt, verwijdert u deze van de pagina [Ingerichte oplossingen](https://www.azureiotsolutions.com/Accelerators#dashboard):

![Oplossing verwijderen](media/iot-accelerators-remote-monitoring-monitor/deletesolution.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de pagina **Dashboard** in de oplossingsversneller voor externe bewaking gebruikt om de gesimuleerde trucks te filteren en bewaken. Voor informatie over het gebruik van de oplossingsversneller voor het detecteren van problemen met uw verbonden apparaten, gaat u verder met de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Problemen detecteren met apparaten die zijn verbonden met uw bewakingsoplossing](iot-accelerators-remote-monitoring-automate.md)