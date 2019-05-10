---
title: 'Snelstart: De demo-omgeving van Azure Time Series Insights (preview) verkennen | Microsoft Docs'
description: Verkrijg inzicht in de demo-omgeving van Azure Time Series Insights (preview).
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: dbdbfc797d37ed38936d6cfd354383d412c6b52d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205826"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Snelstart: De demo-omgeving van Azure Time Series Insights (preview) verkennen

In deze snelstartgids helpt u op weg met de Azure Time Series Insights-Preview. Via de gratis demo, u zult maakt een rondleiding door belangrijke functies die zijn toegevoegd in de Preview van Time Series Insights.

De Preview-demo-omgeving bevat een scenario-bedrijf, Contoso, die twee wind turbine farms werkt, elk met 10 maar. Elke turbine beschikt over 20 sensoren die elke minuut gegevens rapporteren aan Azure IoT Hub. De sensoren informatie verzamelen over de weersomstandigheden, blade verkopen, en yaw positie. Bovendien bewaakt generator prestaties, uitgaande gedrag en de veiligheid.

 U leert hoe u het gebruik van Time Series Insights bruikbare inzichten in gegevens van Contoso kunnen vinden. U moet ook een korte hoofdoorzaak wordt onderzocht om beter te voorspellen kritieke fouten en onderhoud uit te voeren.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Verkenner van Time Series Insights nader bekeken in een demo-omgeving

De Verkenner van Time Series Insights Preview ziet u historische gegevens en analyse van hoofdoorzaken. Aan de slag gaan:

1. Maak een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) als een nog niet is gemaakt.

1. Navigeer naar de [Contoso Wind Farm demo](https://insights.timeseries.azure.com/preview/samples) omgeving.  

1. Als u wordt gevraagd, moet u zich aanmelden bij de Time Series Insights-Verkenner met de referenties van uw Azure-account.

## <a name="work-with-historical-data"></a>Werken met historische gegevens

1. Bekijk windturbine **W7** in **Contoso-fabriek 1**.  

    * Bijwerken van het weergavebereik tot **1/1/17 20:00 tot en met 3/10/17 20:00 uur (UTC)**.
    * Selecteer de **Contoso-fabriek 1** > **W7** > **Generator System** > **GeneratorSpeed** de sensor. Bekijk de resulterende waarden.

      [![W7 in Contoso-fabriek 1](media/v2-update-quickstart/quickstart-one.png)](media/v2-update-quickstart/quickstart-one.png#lightbox)

1. Onlangs heeft Contoso brand geconstateerd in windturbine **W7**. Adviezen variëren over wat de oorzaak in de buurt van de brand was. Bij dichter bij controle zien we dat de waarschuwing fire-sensor tijdens de brand is geactiveerd.

    * Bijwerken van het weergavebereik tot **3/9/17 20:00 tot en met 3/10/17 20:00 uur (UTC)**.
    * Selecteer de **veiligheid System** > **FireAlert** sensor.

      [![Contoso brand in windturbine W7 gevonden](media/v2-update-quickstart/quickstart-two.png)](media/v2-update-quickstart/quickstart-two.png#lightbox)

1. Bekijk andere gebeurtenissen die rond dezelfde tijd van de brand om te begrijpen wat er is gebeurd. Olie druk te verlichten en actieve waarschuwingen verrijkt net vóór de brand.

    * Selecteer de **inspiratie System** > **HydraulicOilPressure** sensor.
    * Selecteer de **inspiratie System** > **ActiveWarning** sensor.

      [![Andere gebeurtenissen die rond dezelfde tijd controleren](media/v2-update-quickstart/quickstart-three.png)](media/v2-update-quickstart/quickstart-three.png#lightbox)

1. De olie druk te verlichten en actieve waarschuwing sensoren verrijkt precies vóór de brand. Vouw de weergegeven tijd serie als u wilt zien van de andere zich aanwezig aanloop naar de brand. Beide sensoren schommelde consistent na verloop van tijd een permanente en eng patroon waarmee wordt aangegeven.

    * Bijwerken van het weergavebereik tot **2/24/17 20:00 tot en met 3/10/17 20:00 uur (UTC)**.

      [![Oliedruk in de- en actieve waarschuwing sensoren ook verrijkt](media/v2-update-quickstart/quickstart-four.png)](media/v2-update-quickstart/quickstart-four.png#lightbox)

1. Onderzoek van twee jaar van historische gegevens blijkt dat een andere fire-gebeurtenis met de dezelfde sensor fluctuaties.

    * Bijwerken van het weergavebereik tot **1/1/16 tot en met 31-12/17** (alle gegevens).

      [![Historische patronen zoeken](media/v2-update-quickstart/quickstart-five.png)](media/v2-update-quickstart/quickstart-five.png#lightbox)

Met behulp van Azure Time Series Insights en onze telemetrie sensor, hebben we een problematische en lange termijn trend verborgen in onze historische gegevens gedetecteerd. Met deze nieuwe inzichten kunt we het volgende doen:

> [!div class="checklist"]
> * Leg uit wat er in feite is opgetreden.
> * Los het probleem.
> * Superieure waarschuwingsmeldingen systemen in locatie plaatsen.

## <a name="root-cause-analysis"></a>Hoofdoorzaakanalyses

1. Sommige scenario's voor geavanceerde analyse om subtiele of er aanwijzingen in de gegevens bloot te nodig hebben. Selecteer de Zwart gat **W6** op datum **6/25**

    * Bijwerken van het weergavebereik tot **6/1/17 20:00 tot en met 7/1/17 20:00 uur (UTC)**.
    * Selecteer vervolgens de **Contoso-fabriek 1** > **W6** > **veiligheid System** > **VoltageActuatorSwitchWarning**  sensor.

      [![Bijwerken van het weergavebereik, en selecteer W6](media/v2-update-quickstart/quickstart-six.png)](media/v2-update-quickstart/quickstart-six.png#lightbox)

1. De waarschuwing geeft aan dat er een probleem is met de spanning die wordt geleverd door de generator. De algemene power-uitvoer van de generator wordt uitgevoerd in de normale parameters onze Huidig interval opgegeven. Door onze interval, een ander patroon ontstaat: Er is een goede inleverbibliotheek.

    * Verwijder de **VoltageActuatorSwitchWarning** sensor.
    * Selecteer de **Generator System** > **ActivePower** sensor.
    * Het interval om te werken **3d**.

      [![Het interval bijwerken naar 3d](media/v2-update-quickstart/quickstart-seven.png)](media/v2-update-quickstart/quickstart-seven.png#lightbox)

1. Door het tijdsbereik vergroot, kunnen we bepalen of het probleem is gestopt of of blijft.

    * Breid de tijdsspanne tot 60 dagen.

      [![De tijdsduur tot 60 dagen verlengen](media/v2-update-quickstart/quickstart-eight.png)](media/v2-update-quickstart/quickstart-eight.png#lightbox)

1. Andere gegevenspunten sensor kunnen worden toegevoegd aan het leveren van uitmuntende context. Meer sensoren te kunnen weergeven, de uitgebreidere ons een beeld vormen van het probleem is. We verwijderen een markering om de werkelijke waarden te bekijken. 

    * Selecteer de **Generator System** > **GridVoltagePhase1**, **GridVoltagePhase2**, en **GridVoltagePhase3** sensoren .
    * Verwijder een markering van het laatste gegevenspunt in het zichtbare gedeelte.

      [![Een markering verwijderen](media/v2-update-quickstart/quickstart-nine.png)](media/v2-update-quickstart/quickstart-nine.png#lightbox)

    De drie spanningssensors werkt neemt en binnen het normale parameters. Het ziet eruit als de **GridVoltagePhase3** sensor is het overmatig.

1. Met maximaal contextuele gegevens toegevoegd, verschijnt de fase 3-inleverbibliotheek nog meer als het probleem. We zijn nu gereed om te verwijzen van het probleem naar ons onderhoudsteam met een goede lead op de oorzaak van de waarschuwing.  

    * De weergave voor alle overlay bijwerken **Generator System** sensoren op dezelfde grafiek schaal.

       [![De weergave zodanig dat alles bijwerken](media/v2-update-quickstart/quickstart-ten.png)](media/v2-update-quickstart/quickstart-ten.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw eigen Time Series Insights-omgeving (preview) gaan maken:

> [!div class="nextstepaction"]
> [Een Time Series Insights-omgeving (preview) plannen](time-series-insights-update-plan.md)

Meer informatie over de demo en de bijbehorende functies gaan:

> [!div class="nextstepaction"]
> [De Verkenner van Time Series Insights Preview](time-series-insights-update-explorer.md)
