---
title: Problemen vaststellen, oplossen en oplossen van problemen in Azure Time Series Insights | Microsoft Docs
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
ms.custom: seodec18
ms.openlocfilehash: 36ea2b8d3649fbda5a5cd6cc5f2cd05cdc095902
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555809"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Vaststellen en oplossen van problemen in uw Time Series Insights-omgeving

In dit artikel worden enkele problemen beschreven die in uw Azure Time Series Insights-omgeving optreden. Het artikel biedt mogelijke oorzaken en oplossingen voor het omzetten van.

## <a name="video"></a>Video: 

In deze video behandelen we veelvoorkomende problemen van Time Series Insights-klanten en oplossingen:</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-1-no-data-is-shown"></a>Probleem 1: Er zijn geen gegevens worden weergegeven

Er zijn geen gegevens in de [Azure Time Series Insights explorer](https://insights.timeseries.azure.com) voor enkele veelvoorkomende redenen optreden:

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Oorzaak A: Gebeurtenisgegevens van de bron zich niet in JSON-indeling

Azure Time Series Insights biedt alleen ondersteuning voor JSON-gegevens. Zie voor voorbeelden van JSON, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Oorzaak B: De sleutel van de bron gebeurtenis ontbreekt een vereiste machtiging

* Voor een IoT-hub in Azure IoT Hub, moet u de sleutel die is **service verbinding maken met** machtigingen. Een van de **iothubowner** of **service** beleidsregels werken omdat ze beide **service verbinding maken met** machtigingen.

   ![IoT Hub-service verbindingsmachtigingen](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)


* Voor een event hub in Azure Event Hubs, moet u de sleutel die is **luisteren** machtigingen. Een van de **lezen** of **beheren** beleidsregels werken omdat ze beide **luisteren** machtigingen.

   ![Event hub listen-machtigingen](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

### <a name="cause-c-the-consumer-group-provided-isnt-exclusive-to-time-series-insights"></a>Oorzaak C: De opgegeven consumergroep niet is exclusief voor Time Series Insights

Als u een IoT-hub of een event hub registreert, is het belangrijk om te stellen de consumergroep die u wilt gebruiken om de gegevens te lezen. Deze consumergroep *kan niet worden gedeeld*. Als de consumergroep wordt gedeeld, de onderliggende IoT-hub of event hub automatisch en willekeurig verbreekt verbinding met een van de lezers. Geef een unieke consumergroep voor Time Series Insights om uit te lezen.

## <a name="problem-2-some-data-is-shown-but-data-is-missing"></a>Probleem 2: Sommige gegevens worden weergegeven, maar er ontbreken gegevens

Wanneer gegevens slechts gedeeltelijk wordt weergegeven en de gegevens wordt weergegeven om te worden vertraging, moet u rekening houden met verschillende mogelijkheden.

### <a name="cause-a-your-environment-is-being-throttled"></a>Oorzaak A: Uw omgeving wordt beperkt

Beperking is een veelvoorkomend probleem wanneer omgevingen zijn ingericht, nadat u een gebeurtenisbron met gegevens maken. Azure IoT Hub en Azure gebeurtenissen Hubs opslaan van gegevens voor maximaal zeven dagen. Time Series Insights begin altijd bij de oudste gebeurtenissen in de gebeurtenis bron (first in, First out, of *FIFO*). 

Als u 5 miljoen gebeurtenissen in een gebeurtenisbron hebt wanneer u verbinding met een S1 maakt, leest één eenheid Time Series Insights-omgeving, Time Series Insights ongeveer 1 miljoen gebeurtenissen per dag. Er kan uitzien zoals Time Series Insights vijf dagen met een latentie van ondervindt. Wat gebeurt er is echter dat de omgeving wordt beperkt. 

Als u oude gebeurtenissen in de gebeurtenisbron hebt, kunt u benaderen beperking op twee manieren:

- Limieten voor het bewaren van de bron van de gebeurtenis als u wilt verwijderen van oude gebeurtenissen die u niet wilt weergegeven in de Time Series Insights wijzigen.
- Richt een groter van omgeving (het aantal eenheden) voor een betere de doorvoer van oude gebeurtenissen. Met behulp van het vorige voorbeeld, als u dezelfde S1 omgeving vijf eenheden voor één dag verhogen, moet de omgeving achterstand binnen een dag. Als uw productie-onveranderlijke gebeurtenis 1 miljoen of minder gebeurtenissen per dag is, kunt u de capaciteit van de gebeurtenis één eenheid beperken nadat deze de resultaten.

De beperkingslimiet wordt afgedwongen op basis van de SKU-type van de omgeving en capaciteit. Alle bronnen van gebeurtenissen in de omgeving delen deze capaciteit. Als de bron van de gebeurtenis voor uw IoT-hub of event hub pushes gegevens buiten de grenzen afgedwongen, ziet u de beperking en een vertraging.

De volgende afbeelding toont een Time Series Insights-omgeving met een SKU van S1 en een capaciteit van 3. Kan het binnenkomende 3 miljoen gebeurtenissen per dag.

![Huidige omgeving SKU-capaciteit](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Als voorbeeld wordt ervan uitgegaan dat deze omgeving berichten van een event hub neemt. De volgende afbeelding toont de frequentie van de inkomende gegevens:

![Voorbeeld van de inkomende tarief voor een event hub](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Het dagelijkse tarief voor inkomend verkeer is ~ 67,000 berichten. Dit tarief wordt omgezet in ongeveer 46 berichten per minuut. Als de event hub-bericht is samengevoegd tot één gebeurtenis Time Series Insights, plaats beperking niet. Als de event hub-bericht is samengevoegd tot 100 Time Series Insights-gebeurtenissen, moeten 4,600 gebeurtenissen per minuut worden opgenomen. Een S1 SKU-omgeving met een capaciteit van 3 kan alleen 2100 ingangsgebeurtenissen per minuut (1 miljoen gebeurtenissen per dag = 700 gebeurtenissen per minuut op basis van drie eenheden = 2100 gebeurtenissen per minuut). U ziet een vertraging vanwege een beperking voor deze instelling. 

Zie voor een grondig inzicht in hoe logische afvlakken werkt, [ondersteund JSON-vormen](./how-to-shape-query-json.md).

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Aanbevolen oplossingen voor overmatige beperking

Om op te lossen de vertraging, vergroot u de SKU-capaciteit van uw omgeving. Zie voor meer informatie, [uw Time Series Insights-omgeving schalen](time-series-insights-how-to-scale-your-environment.md).

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Oorzaak B: Eerste opname van historische gegevens inkomend verkeer wordt vertraagd

Als u een bestaande gebeurtenisbron verbindt, is het waarschijnlijk dat uw IoT-hub of event hub al gegevens bevat. De omgeving wordt gestart binnenhalen van gegevens vanaf het begin van de bewaarperiode van de bron van de gebeurtenis-bericht. Dit is de standaardverwerking en kan niet worden overschreven. U kunt betrekken beperking. Beperking kan duren voor meer informatie zoals het historische gegevens worden opgenomen.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Aanbevolen oplossingen voor grote eerste gegevensopname

Om op te lossen de vertraging:

1. Verhoog de SKU-capaciteit voor de maximaal toegestane waarde (in dit geval 10). Nadat u de capaciteit verhoogt, wordt het proces inkomend verkeer wordt gestart om af te vangen veel sneller. U betaalt voor de grotere capaciteit. Om te visualiseren hoe snel u hebt vastgelegd, vindt u de beschikbaarheidsgrafiek in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com). 

2. Wanneer de vertraging is bijgewerkt, verlaagt u de SKU-capaciteit op de snelheid van de normale inkomend verkeer.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Probleem 3: Mijn gebeurtenisbron timestamp eigenschap de naam van instelling werkt niet

Zorg ervoor dat de timestamp eigenschapsnaam en waarde aan de volgende regels voldoet:
* Naam van de tijdstempeleigenschap is hoofdlettergevoelig.
* De waarde van de timestamp eigenschap die afkomstig zijn uit uw gebeurtenisbron als een JSON-tekenreeks moet de indeling hebben _jjjj-MM-ddTUU. FFFFFFFK_. Een voorbeeld is **2008-04-12T12:53Z**.

De eenvoudigste manier om ervoor te zorgen dat de naam van de tijdstempeleigenschap is vastgelegd en correct werkt, is met de Verkenner van Time Series Insights. Selecteer een bepaalde tijd nadat u de naam van de tijdstempeleigenschap ingevoerd in de Time Series Insights explorer, met behulp van de grafiek. Met de rechtermuisknop op de selectie en selecteer vervolgens de **gebeurtenissen onderzoeken** optie. 

De eerste kolomkop moet de naam van de tijdstempeleigenschap. Naast het woord **Timestamp**, ziet u **($ts)**. 

Niet ziet u de volgende waarden:
- *(abc)* : Geeft aan dat Time Series Insights is worden gebruikt voor het lezen van de gegevenswaarden als tekenreeksen.
- *Pictogram Agenda*: Geeft aan dat de gegevenswaarde is lezen van Time Series Insights *datum-/*.
- *#*: Geeft aan dat Time Series Insights de gegevenswaarden wordt gelezen als een geheel getal zijn.


## <a name="next-steps"></a>Volgende stappen

- Hulp nodig hebt, kunt u een gesprek starten de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Gebruik voor voor de ondersteuningsopties, [ondersteuning van Azure](https://azure.microsoft.com/support/options/).
