---
title: Diagnosticeren en oplossen van problemen | Microsoft Docs
description: Deze zelfstudie wordt beschreven hoe opsporen en oplossen van problemen in uw omgeving Time Series Insights
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4e10a009eb67706d927ece5692134d802094cdf9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnosticeren en oplossen van problemen in uw omgeving Time Series Insights

## <a name="i-dont-see-my-data"></a>Ik weergegeven mijn gegevens niet
Hier volgen enkele redenen waarom u niet ziet uw gegevens in uw omgeving in de [Azure Time Series Insights-portal](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>De gebeurtenisbron bevat geen gegevens in JSON-indeling
Inzicht van Azure Time Series ondersteunt vandaag JSON-gegevens. Zie voor voorbeelden van de JSON, [JSON ondersteund vormen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a>Nadat u uw gebeurtenisbron geregistreerd, kunt u de sleutel die de vereiste machtiging niet opgeven
* Voor een IoT-hub, moet u de sleutel die is opgeven **service verbinding** machtiging.

   ![IoT hub service connect machtiging](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding, een van de beleidsregels **iothubowner** en **service** zou werken, omdat beide **service verbinding** machtiging.
* Voor een event hub, moet u de sleutel die is opgeven **luisteren** machtiging.

   ![Event hub listen machtiging](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding, een van de beleidsregels **lezen** en **beheren** zou werken, omdat beide **luisteren** machtiging.

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a>De opgegeven consumergroep is niet uitsluitend van toepassing op tijd reeks Insights
Voor een IoT-hub of een event hub, tijdens de registratie moet u de consumergroep die moet worden gebruikt voor het lezen van uw gegevens opgeven. Deze consumergroep moet niet worden gedeeld. Als deze wordt gedeeld, de onderliggende event hub automatisch verbreekt verbinding met een van de lezers willekeurig.

## <a name="i-see-my-data-but-theres-a-lag"></a>Ik mijn gegevens worden weergegeven, maar er is een vertraging
Hier volgen de redenen waarom u gegevens gedeeltelijk mogelijk ziet in uw omgeving in de [Time Series Insights-portal](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Uw omgeving worden beperkt door ophalen
De limiet voor bandbreedteregeling wordt afgedwongen op basis van de omgeving SKU type en capaciteit. Alle bronnen van gebeurtenissen in de omgeving delen deze capaciteit. Als de gegevensbron voor uw IoT-hub of event hub gegevens buiten de grenzen afgedwongen pusht, ziet u bandbreedtebeperking en een vertraging.

Het volgende diagram toont een tijd reeks Insights-omgeving die een SKU S1 en een capaciteit van 3 heeft. Kan het 3 miljoen ingangsgebeurtenissen per dag.

![Huidige omgeving SKU-capaciteit](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Wordt ervan uitgegaan dat deze omgeving berichten van een event hub is opnemen met de ingress-snelheid in het volgende diagram wordt weergegeven:

![Voorbeeld inkomend frequentie voor een event hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Zoals u in het diagram, is de dagelijkse snelheid van inkomende berichten ~ 67,000. Dit recht kan ongeveer aan 46 berichten elke minuut. Als elk gebeurtenisbericht hub samengevoegd tot één keer reeks Insights gebeurtenis, ziet deze omgeving geen beperking. Als elk gebeurtenisbericht hub samengevoegd tot 100 Time Series Insights-gebeurtenissen, moeten klikt u vervolgens 4,600 gebeurtenissen worden ingenomen elke minuut. Een S1 SKU-omgeving met een capaciteit van 3 kunnen alleen 2 100 ingangsgebeurtenissen elke minuut (1 miljoen gebeurtenissen per dag = 700 gebeurtenissen per minuut bij 3 eenheden = 2 100 gebeurtenissen per minuut). Daarom ziet u een vertraging vanwege een beperking. 

Zie voor een grondige kennis van de logica plat werking [JSON ondersteund vormen](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Aanbevolen stappen
Om op te lossen de vertraging, verhoogt u de SKU-capaciteit van uw omgeving. Zie voor meer informatie [schalen van uw omgeving Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>U historische gegevens worden gepusht en veroorzaakt door trage inkomend
Als u een bestaande gebeurtenisbron verbindt, is het waarschijnlijk dat uw IoT-hub of event hub al gegevens daarin. De omgeving begint dus binnenhalen van gegevens vanaf het begin van de gebeurtenisbron bericht bewaarperiode. 

Dit gedrag is de standaardinstelling en kan niet worden overschreven. U kunt benaderen bandbreedtebeperking en duurt het even te lopen op het opnemen van historische gegevens.

#### <a name="recommended-steps"></a>Aanbevolen stappen
U kunt de vertraging oplossen door de volgende stappen uitvoeren:
1. Verhoogt de SKU-capaciteit op de maximaal toegestane waarde (10 in dit geval). Nadat de capaciteit wordt verhoogd, begint het proces inkomend afvangen veel sneller. U kunt visualiseren hoe snel u bent bouwen door middel van de grafiek beschikbaarheid in de [Time Series Insights-portal](https://insights.timeseries.azure.com). Worden in rekening gebracht voor grotere capaciteit.
2. Nadat de vertraging is op de hoogte, verminderen, de SKU-capaciteit terug naar de snelheid van de normale inkomend.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Mijn gebeurtenisbron *tijdstempel eigenschapsnaam* instelling werkt niet
Zorg ervoor dat de naam en waarde aan de volgende regels voldoen:
* De naam van de timestamp-eigenschap is _hoofdlettergevoelig_.
* De waarde van de timestamp-eigenschap die afkomstig is van de gebeurtenisbron, als een JSON-tekenreeks moet de indeling _jjjj-MM-ddTUU. FFFFFFFK_. Een voorbeeld van een dergelijke tekenreeks is ' 2008-04-12T12:53Z '.
