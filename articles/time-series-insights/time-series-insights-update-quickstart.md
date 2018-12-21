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
ms.date: 12/03/2018
ms.openlocfilehash: de5e853db6c6a0e98dea9251cc07b526288574e1
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276823"
---
# <a name="quickstart-explore-the-azure-time-series-insights-preview-demo-environment"></a>Snelstart: De demo-omgeving van Azure Time Series Insights (preview) verkennen

In deze snelstartgids wordt uitgelegd hoe u in een gratis demo-omgeving aan de slag kunt met de verkenner van de preview-versie van Azure Time Series Insights. U leert hoe u in uw webbrowser grote hoeveelheden historische, industriële IoT-gegevens kunt visualiseren en u ontdekt de belangrijkste functies van de verkenner van Time Series Insights (preview).

Time Series Insights biedt een end-to-end-PaaS (Platform as a Service). Het platform wordt gebruikt om voor tijdreeksen geoptimaliseerde, in hoge mate gecontextualiseerde gegevens op IoT-schaal op te nemen, te verwerken, op te slaan en op te vragen. Op deze manier kunnen gegevens geïmproviseerd worden verkend. Het platform biedt ook operationele analyse. Time Series Insights is een oplossing op maat voor de unieke vereisten van industriële IoT-implementaties.

De demo-omgeving bevat informatie over een bedrijf dat elektriciteit opwekt, Contoso. In de omgeving gebruikt u Time Series Insights om bruikbare inzichten te verkrijgen op basis van de Contoso-gegevens en om uit te zoeken wat de hoofdoorzaak is van een probleem. Contoso werkt met twee windturbineparken, elk met 10 turbines. Elke turbine beschikt over 20 sensoren die elke minuut gegevens rapporteren aan Azure IoT Hub. De sensoren verzamelen informatie over weersomstandigheden, kanteling en draaiing van de wieken, prestaties van de generator, gedrag van de tandwielkast en veiligheidscontroles.

U gebruikt Time Series Insights (preview) om de voortdurend groeiende gegevensset van Contoso van de afgelopen twee jaar te analyseren. Op dit moment is de gegevensset 40 GB groot. U kunt hierdoor meer inzicht krijgen in en voorspellingen doen over kritieke storingen en onderhoudsproblemen waarvoor moeilijk een oplossing te vinden is.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="explore-the-time-series-insights-explorer-in-a-demo-environment"></a>Verkenner van Time Series Insights nader bekeken in een demo-omgeving

1. Ga in uw browser naar de [Contoso Wind Farm-omgeving](https://insights.timeseries.azure.com/preview/samples).  

1. Meld u waar nodig aan bij Time Series Insights. Gebruik hiervoor de referenties van uw Azure-account.

### <a name="demo-step-1"></a>Demo: stap 1

1. Laten we eens kijken naar **W7** in **Contoso-fabriek 1**.  

    * **Actie**: Werk het weergavebereik bij naar **01-01-2017 20:00 tot 10-03-2017 20:00 uur (UTC)**, voeg de sensor**Contoso Plant 1** > **W7** > **Generator System** > **GeneratorSpeed** toe en geef dan de resulterende waarden weer.

       ![Quickstart een][1]

1. Onlangs heeft Contoso brand geconstateerd in windturbine **W7**. Laten we hier dieper op ingaan. We kunnen zien dat de brandmeldingssensor tijdens de brand is geactiveerd.

    * **Actie**: Werk het weergavebereik bij naar **09-03-2017 20:00 tot 10-03-2017 20:00 uur (UTC)** en voeg de sensor **Safety System** > **FireAlert** toe.

      ![Quickstart twee][2]

1. Laten we kijken wat er nog meer gebeurde rond de tijd van de brand. De waarschuwingen voor oliedruk en activiteit stegen net vóór de brand, maar toen was het te laat om het probleem nog te voorkomen.

    * **Actie**: Voeg de sensor **Pitch System** > **HydraulicOilPressure** en de sensor **Pitch System** > **ActiveWarning** toe.

      ![Quickstart drie][3]

1. Als we verder terug kijken, zien we dat er signalen waren die wezen op een aankomende brand. Beide sensoren schommelden. Is dit al eerder gebeurd?

    * **Actie**: Werk het weergavebereik bij naar **24-02-2017 tot 10-03-2017**.

      ![Quickstart vier][4]

1. Als we naar de volledige twee jaar aan gegevens kijken, zien we een eerdere brandgebeurtenis met dezelfde voortekenen. Met deze gegevens kunnen we systemen bouwen om dit soort problemen vroegtijdig te ondervangen.

    * **Actie**: Werk het weergavebereik bij naar **01-01-2016 tot 31-12-2017** (alle gegevens).

       ![Quickstart vijf][5]

### <a name="demo-step-2"></a>Demo: stap twee

1. Andere problemen zijn subtieler en moeilijker op te sporen. Time Series Insights biedt een scala aan mogelijkheden om te helpen bij het opsporen van lastig vindbare problemen. Hier zien we een onderbreking van de waarschuwingssensor bij **W6** op **25-06**. Maar wat gebeurt er dan eigenlijk?

    * **Actie**: Verwijder de huidige sensoren, werk het weergavebereik bij naar **01-06-2017 20:00 tot 01-07-2017 20:00 uur (UTC)** en voeg de sensor **Contoso Plant 1** > **W6** > **Safety System** > **VoltageActuatorSwitchWarning**.

       ![Quickstart zes][6]

1. De waarschuwing geeft aan dat er een probleem is met de spanning die wordt geleverd door de generator. Maar wat is daar de oorzaak van? De algehele stroomuitvoer van de generator ziet er goed uit op basis van een gedetailleerd interval. Maar door de gegevens samen te voegen, zien we een definitieve afname.

    * **Actie**: Verwijder de sensor **VoltageActuatorSwitchWarning** voeg de sensor **Generator System** > **ActivePower** toe en werk het interval bij naar **3d**.

       ![Quickstart zeven][7]

1. Als we vooruit navigeren in de gegevensset, zien we dat dit geen tijdelijk probleem is. Het treedt voortdurend op.

    * **Actie**: Breid de tijdsduur naar rechts uit.

       ![Quickstart acht][8]

1. We gaan nog dieper op de gegevens in. We kunnen andere gegevenspunten van de sensor toevoegen om de spanning per fase weer te geven. De gegevenspunten zien er echter allemaal vergelijkbaar uit. We verwijderen een markering om de werkelijke waarden te bekijken. Het lijkt erop dat er een probleem is met de fase 3-uitvoer.

    * **Actie**: Voeg de sensoren **Generator System** > **GridVoltagePhase1**, **GridVoltagePhase2** en **GridVoltagePhase3** toe. Verwijder een markering van het laatste gegevenspunt in het zichtbare gedeelte.

       ![Quickstart acht][8]

1. Als we alle drie de gegevenspunten op dezelfde schaal bekijken, wordt de afname in fase 3 nog beter duidelijk. Op dit moment kunnen we het probleem overdragen aan ons onderhoudsteam met een goede lead naar de oorzaak van de waarschuwing.  

    * **Actie**: Werk de weergave bij zodat alle sensoren als overlay op dezelfde grafiekschaal worden weergegeven.

       ![Quickstart negen][9]

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw eigen Time Series Insights-omgeving (preview) gaan maken:

> [!div class="nextstepaction"]
> [Een Time Series Insights-omgeving (preview) plannen](time-series-insights-update-plan.md)

<!-- Images -->
[1]: media/v2-update-quickstart/quickstart-one.png
[2]: media/v2-update-quickstart/quickstart-two.png
[3]: media/v2-update-quickstart/quickstart-three.png
[4]: media/v2-update-quickstart/quickstart-four.png
[5]: media/v2-update-quickstart/quickstart-five.png
[6]: media/v2-update-quickstart/quickstart-six.png
[7]: media/v2-update-quickstart/quickstart-seven.png
[8]: media/v2-update-quickstart/quickstart-eight.png
[9]: media/v2-update-quickstart/quickstart-nine.png
