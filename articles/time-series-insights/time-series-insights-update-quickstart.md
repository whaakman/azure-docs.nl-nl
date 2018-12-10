---
title: De demo-omgeving van Azure Time Series Insights (preview) verkennen | Microsoft Docs
description: Inzicht krijgen in de demo-omgeving van Azure Time Series Insights (preview)
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 12/03/2018
ms.openlocfilehash: 76bdc122cab5b4ee87717c9c4df9774ecc3bd8f7
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888771"
---
# <a name="explore-the-azure-time-series-insights-preview-demo-environment"></a>De demo-omgeving van Azure Time Series Insights (preview) verkennen

In deze quickstart wordt uitgelegd hoe u in een gratis demo-omgeving aan de slag kunt met de verkenner van de preview-versie van Azure Time Series Insights (TSI). U leert hoe u in uw webbrowser grote hoeveelheden historische, industriële IoT-gegevens kunt visualiseren en u ontdekt de belangrijkste functies van de verkenner van Azure Time Series Insights (preview).

Azure TSI biedt een end-to-end Platform-as-a-Service-oplossing voor het opnemen, het verwerken, het opslaan en het uitvoeren van query's op zeer contextuele gegevens op IoT-schaal die zijn geoptimaliseerd voor Time Series voor ad-hoc gegevensverkenning en operationele analyse. Time Series Insights is een oplossing op maat voor de unieke vereisten van industriële IoT-implementaties.

De demo-omgeving toont een bedrijf dat elektriciteit opwekt, Contoso, die TSI gebruikt om praktische inzichten in de bedrijfsgegevens te verkrijgen en een korte hoofdoorzaakanalyse uit te voeren. Contoso beheert twee windmolenparken, elk met 10 windmolens, waarbij elke turbine 20 sensoren bevat die elke minuut gegevens aan Azure IoT Hub rapporteren. De sensoren verzamelen informatie over weersomstandigheden, kanteling en draaiing van de wieken, prestaties van de generator, gedrag van de versnellingsbak en veiligheidscontroles.

TSI (preview) wordt gebruikt voor het analyseren van de voortdurend groeiende gegevensset van de afgelopen twee jaar (momenteel 40 GB) om kritieke fouten en langzaam ontstane onderhoudsproblemen beter te begrijpen en te voorspellen.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Verkenner van Time Series Insights nader bekeken in een demo-omgeving

1. Open een browser en ga naar [insights.timeseries.azure.com/preview/samples](https://insights.timeseries.azure.com/preview/samples).  

1. Meld u indien nodig aan bij TSI met de referenties van uw Azure-account.

### <a name="demo-step-one"></a>Demo: stap een

1. Laten we eens kijken naar **windturbine 7 in windmolenpark 1**.  

    * **Actie**: werk het weergavebereik bij tot `1/1/17 20:00 – 3/10/17 20:00 (UTC)` en voeg de sensor `Farm 1 > W7 > Generator > GeneratorSpeed` toe. Geef vervolgens de resulterende waarden weer.

       ![Quickstart een][1]

1. Onlangs **heeft Contoso brand geconstateerd in windturbine 7**. Laten we hier dieper op ingaan. We kunnen zien dat de brandmeldingssensor tijdens de brand is geactiveerd.

    * **Actie**: werk het weergavebereik bij tot `3/9/17 20:00 – 3/10/17 20:00 (UTC)` en voeg de sensor `Safety > FireAlert` toe.

      ![Quickstart twee][2]

1. Laten we kijken wat er nog meer gebeurde rond de tijd van de brand. De waarschuwingen voor oliedruk en activiteit stegen net vóór de brand, maar toen was het te laat om het probleem nog te voorkomen.

    * **Actie**: voeg de sensor `Pitch > HydraulicOilPressure` en de sensor `Pitch > ActiveWarning` toe.

      ![Quickstart drie][3]

1. Als we verder terug kijken, zien we dat er signalen waren die wezen op een aankomende brand. Beide sensoren schommelden. Dus is dit eerder gebeurd?

    * **Actie**: werk het weergavebereik bij tot `2/24/17 20:00 – 3/10/17 20:00 (UTC)`.

      ![Quickstart vier][4]

1. Als we naar de volledige twee jaar aan gegevens kijken, zien we een eerdere brandgebeurtenis met dezelfde voortekenen. Met deze gegevens kunnen we systemen bouwen om dit soort problemen vroegtijdig te ondervangen.

    * **Actie**: werk het weergavebereik bij tot `1/1/16 – 12/31/17` (alle gegevens).

       ![Quickstart vijf][5]

### <a name="demo-step-two"></a>Demo: stap twee

1. Andere problemen zijn subtieler en moeilijker op te sporen. Time Series Insights biedt een scala aan mogelijkheden om te helpen bij het opsporen van lastig vindbare problemen. Hier zien we een onderbreking van de waarschuwingssensor bij `turbine #6` op `6/25`. Maar wat gebeurt er dan eigenlijk?

    * **Actie**: verwijder de huidige sensoren. Werk het weergavebereik vervolgens bij tot `6/1/17 20:00 – 7/1/17 20:00 (UTC)` en voeg de `Farm 1 > W6 > Safety > VoltageActuatorSwitchWarning` toe.

       ![Quickstart zes][6]

1. De waarschuwing geeft aan dat er een probleem is met de spanning die wordt geleverd door de generator. Maar wat is daar de oorzaak van? De algehele stroomuitvoer van de generator ziet er goed uit op basis van een gedetailleerd interval. Maar door de gegevens samen te voegen, zien we een definitieve afname.

    * **Actie**: verwijder de `VoltageActuatorSwitchWarning`, voeg `Generator > ActivePower` toe en werk het interval bij naar `3d`.

       ![Quickstart zeven][7]

1. Als we vooruit navigeren in de gegevensset, zien we dat dit geen tijdelijk probleem is. Het is een constant probleem.

    * **Actie**: breid de tijdsduur naar rechts uit.

       ![Quickstart acht][8]

1. We gaan nog dieper op de gegevens in. We kunnen andere gegevenspunten van de sensor bekijken om de spanning per fase weer te geven. Maar ze zien er allemaal vergelijkbaar uit. We verwijderen een markering om de werkelijke waarden te bekijken. Het lijkt erop dat drie een probleem is met de fase 3-uitvoer.

    * **Actie**: `Add Generator > GridVoltagePhase1, 2, & 3`. Verwijder een markering van het laatste gegevenspunt in het zichtbare gedeelte.

       ![Quickstart acht][8]

1. Als we alle drie op dezelfde schaal bekijken, wordt de afname in fase 3 nog beter duidelijk. Op dit moment kunnen we dit probleem overdragen aan ons onderhoudsteam met een goede lead naar de oorzaak van de waarschuwing.  

    * **Actie**: werk de weergave bij zodat alle sensoren als overlay op dezelfde grafiekschaal worden weergegeven.

       ![Quickstart negen][9]

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw eigen Azure TSI (preview)-omgeving gaan maken:

> [!div class="nextstepaction"]
> [Uw Azure TSI (preview)-omgeving plannen](time-series-insights-update-plan.md)

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