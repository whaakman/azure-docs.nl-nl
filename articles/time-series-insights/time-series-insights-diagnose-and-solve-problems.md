---
title: Vaststellen en oplossen van problemen in Azure Time Series Insights | Microsoft Docs
description: Dit artikel wordt beschreven hoe u vaststellen, oplossen en oplossen van veelvoorkomende problemen die in uw Azure Time Series Insights-omgeving optreden.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.openlocfilehash: ef06e7b1abd66a2204ef982943fe24354bd7f122
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837440"
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Vaststellen en oplossen van problemen in uw Time Series Insights-omgeving

Dit artikel worden enkele problemen die kunnen worden weergegeven in uw Time Series Insights-omgeving. Het biedt mogelijke oorzaken en oplossingen voor het omzetten van.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>In deze video behandelen we veelvoorkomende problemen van Time Series Insights-klanten en oplossingen.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-1-no-data-is-shown"></a>1 probleem: Er zijn geen gegevens worden weergegeven.
Er zijn enkele veelvoorkomende redenen waarom u ziet mogelijk niet de gegevens in de [Azure Time Series Insights Explorer](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Mogelijke oorzaak A: gebeurtenis brongegevens bevinden zich niet in JSON-indeling
Azure Time Series Insights ondersteunt alleen JSON-gegevens. Zie voor voorbeelden van JSON, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Mogelijke oorzaak B: gebeurtenis bronsleutel ontbreekt een vereiste machtiging
* Voor een IoT-Hub, moet u de sleutel die is opgeven **service verbinding maken met** machtiging.

   ![IoT Hub-service ontbreekt de connect machtiging](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding is een van de beleidsregels **iothubowner** en **service** zou moeten werken, omdat beide **service verbinding maken met** machtiging.

* Voor een event hub, moet u de sleutel die is opgeven **luisteren** machtiging.

   ![Event hub listen machtiging](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Zoals wordt weergegeven in de voorgaande afbeelding is een van de beleidsregels **lezen** en **beheren** zou moeten werken, omdat beide **luisteren** machtiging.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>De consumentengroep opgegeven is niet exclusief voor Time Series Insights C: oorzaak
Tijdens de registratie van uur IoT-Hub of een event hub, moet u de consumentengroep die moet worden gebruikt voor het lezen van de gegevens opgeven. Deze consumergroep moet **niet** worden gedeeld. Als de consumergroep wordt gedeeld, verbroken de onderliggende event hub automatisch een van de lezers willekeurig. Geef een unieke consumergroep voor Time Series Insights om uit te lezen.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Probleem 2: Sommige gegevens worden weergegeven, maar sommige ontbreekt
Wanneer u gegevens gedeeltelijk kunt zien, maar de gegevens is achtergebleven achter, zijn er verschillende mogelijkheden om te overwegen:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>Mogelijke oorzaak A: uw omgeving is ophalen beperkt
Dit is een veelvoorkomend probleem wanneer omgevingen zijn ingericht na het maken van een gebeurtenisbron aan gegevens.  Azure IoT-Hubs en gebeurtenissen Hubs opslaan van gegevens van zeven dagen.  TSI wordt altijd gestart vanuit de oudste gebeurtenis FIFO-principe (), binnen de gebeurtenisbron.  Dus als u vijf miljoen gebeurtenissen in een gebeurtenisbron hebt wanneer u verbinding met een S1-, één eenheid TSI-omgeving maakt, leest TSI ongeveer 1 miljoen gebeurtenissen per dag.  Dit lijkt lijkt het alsof TSI vijf dagen met een latentie van op het eerste gezicht ondervindt.  Wat er werkelijk gebeurt, is dat de omgeving wordt beperkt.  Als u oude gebeurtenissen in de gebeurtenisbron hebt, kunt u twee manieren benaderen:

- Limieten voor het bewaren van uw gebeurtenisbron om te verwijderen van oude gebeurtenissen die u niet wilt weergegeven in de TSI wijzigen
- Richt een groter omgeving (voor wat betreft het aantal eenheden) voor een betere doorvoer van oude gebeurtenissen.  Met behulp van het bovenstaande voorbeeld als u die dezelfde S1-omgeving naar vijf eenheden verhoogd gedurende één dag, moet de omgeving bijwerken naar nu binnen een dag.  Als uw productie-onveranderlijke gebeurtenis 1 miljoen of minder gebeurtenissen per dag is, kunt u de capaciteit van de gebeurtenis omlaag naar één eenheid beperken nadat deze is bijgewerkt.  

De beperkingslimiet wordt afgedwongen op basis van de SKU-type van de omgeving en capaciteit. Alle bronnen van gebeurtenissen in de omgeving delen deze capaciteit. Als de bron van de gebeurtenis voor uw IoT-Hub of event hub is pushen van gegevens buiten de grenzen afgedwongen, ziet u de beperking en een vertraging.

Het volgende diagram toont een Time Series Insights-omgeving met een SKU van S1 en een capaciteit van 3. Kan het binnenkomende 3 miljoen gebeurtenissen per dag.

![Huidige omgeving SKU-capaciteit](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Bijvoorbeeld, wordt ervan uitgegaan dat berichten van een event hub kan in deze omgeving opnemen is. Houd rekening met het inkomend tarief weergegeven in het volgende diagram:

![Voorbeeld van de inkomende tarief voor een event hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Zoals wordt weergegeven in het diagram, wordt de dagelijks tarief dat inkomend ~ 67,000 berichten. Dit tarief wordt omgezet in ongeveer 46 berichten per minuut. Als elk event hub-bericht is samengevoegd tot één Time Series Insights-gebeurtenis, wordt in deze omgeving ziet er geen beperking. Als de event hub-bericht is samengevoegd tot 100 Time Series Insights-gebeurtenissen, moeten klikt u vervolgens 4,600 gebeurtenissen worden opgenomen elke minuut. Een S1 SKU-omgeving met een capaciteit van 3 kan alleen 2100 ingangsgebeurtenissen per minuut (1 miljoen gebeurtenissen per dag = 700 gebeurtenissen per minuut op 3 eenheden = 2100 gebeurtenissen per minuut). Daarom ziet u een vertraging vanwege een beperking. 

Zie voor een grondig inzicht in hoe logische afvlakken werkt, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Aanbevolen stappen voor overmatige beperking
Om op te lossen de vertraging, vergroot u de SKU-capaciteit van uw omgeving. Zie voor meer informatie, [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Mogelijke oorzaak B: initiële opname van historische gegevens wordt veroorzaakt door trage inkomend
Als u een bestaande gebeurtenisbron verbindt, is het waarschijnlijk uw IoT-Hub of event hub in het al gegevens bevat. De omgeving wordt gestart binnenhalen van gegevens vanaf het begin van de bewaarperiode van de bron van de gebeurtenis-bericht.

Dit gedrag is de standaardinstelling en kan niet worden overschreven. U kunt betrekken beperking en het duurt even voor meer informatie over het ophalen van historische gegevens.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Aanbevolen stappen van grote eerste gegevensopname
U kunt de vertraging oplossen, moet u de volgende stappen uitvoeren:
1. Verhoog de SKU-capaciteit voor de maximaal toegestane waarde (10 in dit geval). Nadat de capaciteit wordt verhoogd, begint het proces voor inkomend verkeer afvangen veel sneller. Kunt u visualiseren hoe snel u bent bouwen via de van beschikbaarheidsgrafiek in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com). U betaalt voor de grotere capaciteit.
2. Nadat de vertraging is bijgewerkt, verlaagt u de SKU-capaciteit terug naar de snelheid van de normale inkomend verkeer.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Probleem 3: De bron van gebeurtenis van *naam van de eigenschap timestamp* instelling werkt niet
Zorg ervoor dat de naam en waarde aan de volgende regels voldoet:
* Naam van de tijdstempeleigenschap is _hoofdlettergevoelig_.
* De waarde van de timestamp eigenschap die afkomstig van de bron van gebeurtenis, als een JSON-tekenreeks, moet de indeling hebben _jjjj-MM-ddTUU. FFFFFFFK_. Een voorbeeld van een tekenreeks is die is ' 2008-04-12T12:53Z '.

De eenvoudigste manier om ervoor te zorgen dat uw *naam van de eigenschap timestamp* wordt vastgelegd en correct werkt, is met de TSI-Verkenner.  In de TSI-Verkenner, een bepaalde tijd nadat u hebt opgegeven met behulp van de grafiek en selecteer de *naam van de eigenschap timestamp*.  Met de rechtermuisknop op de selectie en kies de *gebeurtenissen onderzoeken* optie.  De eerste kolom header moet uw *naam van de eigenschap timestamp* en er zijn een *($ts)* naast het woord *tijdstempel*, in plaats van:
- *(abc)* , die aangeeft dat het TSI lezen van de gegevenswaarden als tekenreeksen
- *Pictogram Agenda*, die aangeeft dat het TSI leest de gegevenswaarde als *datum/tijd*
- *#*, de gegevenswaarden die aangeeft dat het TSI wordt gelezen als een geheel getal zijn


## <a name="next-steps"></a>Volgende stappen
- Voor verdere ondersteuning, een gesprek starten op de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- U kunt ook [ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor ondersteuning.
