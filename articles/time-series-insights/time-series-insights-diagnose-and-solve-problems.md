---
title: Diagnosticeren en oplossen van problemen in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe diagnosticeren en oplossen van veelvoorkomende problemen in uw omgeving Azure Time Series Insights optreden kunnen oplossen.
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/15/2017
ms.openlocfilehash: 757d37183ad334aca462af59bad261cfa686299e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnosticeren en oplossen van problemen in uw omgeving Time Series Insights

## <a name="problem-1-no-data-is-shown"></a>1 probleem: Er zijn geen gegevens worden weergegeven.
Er zijn enkele veelvoorkomende redenen waarom u ziet mogelijk ook geen uw gegevens in de [Azure Time Series Insights Explorer](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Mogelijke oorzaak A: gebeurtenisgegevens bron is niet in JSON-indeling
Inzicht van Azure Time Series ondersteunt alleen JSON-gegevens. Zie voor voorbeelden van de JSON, [JSON ondersteund vormen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Mogelijke oorzaak B: gebeurtenis bronsleutel ontbreekt een vereiste machtiging
* Voor een IoT-Hub, moet u de sleutel die is opgeven **service verbinding** machtiging.

   ![Service IoT Hub connect machtiging](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding, een van de beleidsregels **iothubowner** en **service** zou werken, omdat beide **service verbinding** machtiging.
   
* Voor een event hub, moet u de sleutel die is opgeven **luisteren** machtiging.

   ![Event hub listen machtiging](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding, een van de beleidsregels **lezen** en **beheren** zou werken, omdat beide **luisteren** machtiging.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>De consumergroep opgegeven zich niet uitsluitend van toepassing op tijd reeks Insights C: oorzaak
Tijdens de registratie van am IoT-Hub of een event hub, kunt u de consumergroep die moet worden gebruikt voor het lezen van de gegevens opgeven. Deze consumergroep moet **niet** worden gedeeld. Als de consumergroep wordt gedeeld, de onderliggende event hub automatisch verbreekt verbinding met een van de lezers willekeurig. Geef een unieke consumergroep voor tijd reeks inzichten te lezen.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Probleem 2: Sommige gegevens wordt weergegeven, maar sommige ontbreekt
Wanneer u gegevens gedeeltelijk kunt zien, maar de gegevens is achtergebleven achter, zijn er verschillende mogelijkheden te overwegen:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Mogelijke oorzaak A: uw omgeving worden beperkt door ophalen
De limiet voor bandbreedteregeling wordt afgedwongen op basis van de omgeving SKU type en capaciteit. Alle bronnen van gebeurtenissen in de omgeving delen deze capaciteit. Als de gegevensbron voor uw IoT-Hub of event hub gegevens buiten de grenzen afgedwongen pusht, ziet u bandbreedtebeperking en een vertraging.

Het volgende diagram toont een tijd reeks Insights-omgeving die een SKU S1 en een capaciteit van 3 heeft. Kan het 3 miljoen ingangsgebeurtenissen per dag.

![Huidige omgeving SKU-capaciteit](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Stel bijvoorbeeld dat deze omgeving van berichten van een event hub opnemen. Houd rekening met de ingress-snelheid in het volgende diagram wordt weergegeven:

![Voorbeeld inkomend frequentie voor een event hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Zoals u in het diagram, is de dagelijkse snelheid van inkomende berichten ~ 67,000. Dit recht kan ongeveer aan 46 berichten elke minuut. Als elk gebeurtenisbericht hub samengevoegd tot één keer reeks Insights gebeurtenis, ziet deze omgeving geen beperking. Als elk gebeurtenisbericht hub samengevoegd tot 100 Time Series Insights-gebeurtenissen, moeten klikt u vervolgens 4,600 gebeurtenissen worden ingenomen elke minuut. Een S1 SKU-omgeving met een capaciteit van 3 kunnen alleen 2 100 ingangsgebeurtenissen elke minuut (1 miljoen gebeurtenissen per dag = 700 gebeurtenissen per minuut bij 3 eenheden = 2 100 gebeurtenissen per minuut). Daarom ziet u een vertraging vanwege een beperking. 

Zie voor een grondige kennis van de logica plat werking [JSON ondersteund vormen](time-series-insights-send-events.md#supported-json-shapes).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Stappen voor overmatige beperking aanbevolen oplossing
Om op te lossen de vertraging, verhoogt u de SKU-capaciteit van uw omgeving. Zie voor meer informatie [schalen van uw omgeving Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Mogelijke oorzaak B: initiële opname van historische gegevens wordt veroorzaakt door trage inkomend
Als u een bestaande gebeurtenisbron zijn verbonden, is het waarschijnlijk dat uw IoT-Hub of event hub al gegevens daarin. De omgeving start binnenhalen van gegevens vanaf het begin van de gebeurtenisbron bericht bewaarperiode.

Dit gedrag is de standaardinstelling en kan niet worden overschreven. U kunt benaderen bandbreedtebeperking en duurt het even te lopen op het opnemen van historische gegevens.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Aanbevolen Oplossingsstappen van grote eerste opname
U kunt de vertraging oplossen door de volgende stappen uitvoeren:
1. Verhoogt de SKU-capaciteit op de maximaal toegestane waarde (10 in dit geval). Nadat de capaciteit wordt verhoogd, begint het proces inkomend afvangen veel sneller. U kunt visualiseren hoe snel u bent bouwen door middel van de grafiek beschikbaarheid in de [Time Series Insights explorer](https://insights.timeseries.azure.com). Worden in rekening gebracht voor grotere capaciteit.
2. Nadat de vertraging is op de hoogte, verminderen, de SKU-capaciteit terug naar de snelheid van de normale inkomend.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Probleem 3: Mijn gebeurtenisbron van *tijdstempel eigenschapsnaam* instelling werkt niet
Zorg ervoor dat de naam en waarde aan de volgende regels voldoen:
* De naam van de timestamp-eigenschap is _hoofdlettergevoelig_.
* De waarde van de timestamp-eigenschap die afkomstig is van de gebeurtenisbron, als een JSON-tekenreeks moet de indeling _jjjj-MM-ddTUU. FFFFFFFK_. Een voorbeeld van een dergelijke tekenreeks is ' 2008-04-12T12:53Z '.

## <a name="next-steps"></a>Volgende stappen
- Voor aanvullende ondersteuning door een gesprek te starten op de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- U kunt ook [ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor ondersteuning.
