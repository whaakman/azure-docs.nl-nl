---
title: Bewaken en beperken van beperkingen in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u bewaken, opsporen en oplossen van prestatieproblemen waardoor latentie en beperkingen in Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/27/2017
ms.openlocfilehash: a404eb1393f9e99c2e2932c2d23724051f1b72a0
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628484"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Controleren en om te verminderen latentie in Azure Time Series Insights Aanvraagbeperkingen minimaliseren
Wanneer de hoeveelheid inkomende gegevens groter is dan de configuratie van uw omgeving, kunt u latentie of beperkingen in Azure Time Series Insights kan optreden.

U kunt voorkomen dat latentie en beperkingen door het correct configureren van uw omgeving voor de hoeveelheid gegevens die u wilt analyseren.

U bent waarschijnlijk om latentie en beperkingen wanneer u:

- Toevoegen van een gebeurtenisbron dat oude gegevens die kan groter zijn dan de snelheid van de toegewezen inkomend bevat (Time Series Insights moet voor meer informatie).
- Meer gebeurtenisbronnen toevoegen aan een omgeving, wat resulteert in een piek van aanvullende gebeurtenissen (dit kan groter zijn dan de capaciteit van uw omgeving).
- Grote hoeveelheden historische gebeurtenissen verzenden naar een gebeurtenisbron, wat resulteert in een vertraging (Time Series Insights moet voor meer informatie).
- Neem deel aan referentiegegevens met telemetrie, wat resulteert in de gebeurtenis groter.  Een gegevenspakket ingressed met een pakketgrootte van 32 KB wordt beschouwd als 32-gebeurtenissen van een beperking perspectief, elke grootte van 1 KB. De toegestane maximale gebeurtenisgrootte is 32 KB; gegevenspakketten die groter zijn dan 32 KB worden afgebroken.


## <a name="monitor-latency-and-throttling-with-alerts"></a>Latentie en aanvraagbeperking en waarschuwingen bewaken

Waarschuwingen kunnen u helpen te helpen opsporen en corrigeren van latentieproblemen met veroorzaakt door uw omgeving. 

1. Klik in de Azure-portal op **metrische gegevens**. 

   ![Metrische gegevens](media/environment-mitigate-latency/add-metrics.png)

2. Klik op **metrische waarschuwing toevoegen**.  

    ![Metrische waarschuwing toevoegen](media/environment-mitigate-latency/add-metric-alert.png)

Van daaruit kunt u waarschuwingen met behulp van de volgende metrische gegevens configureren:

|Gegevens  |Beschrijving  |
|---------|---------|
|**Inkomend verkeer ontvangen Bytes**     | Telling van onbewerkte bytes lezen uit bronnen van gebeurtenissen. Onbewerkte aantal omvat gewoonlijk de naam van de eigenschap en waarde.  |  
|**Inkomende gegevens ontvangen ongeldig berichten**     | Aantal ongeldige berichten lezen van alle bronnen van Azure Event Hubs of Azure IoT Hub gebeurtenissen.      |
|**Binnenkomende berichten ontvangen**   | Aantal berichten lezen van bronnen van gebeurtenissen voor alle Event Hubs of IoT-Hubs.        |
|**Binnenkomende Bytes opgeslagen**     | Totale grootte van gebeurtenissen die zijn opgeslagen en beschikbaar voor query's. Grootte wordt alleen op de waarde van de eigenschap berekend.        |
|**Inkomende gebeurtenissen opgeslagen**     |   Telt het aantal samengevoegde gebeurtenissen opgeslagen en beschikbaar voor query's.      |
|**Inkomend verkeer ontvangen bericht tijdsinterval**    |  Het verschil in seconden tussen het moment dat het bericht in de gebeurtenis in de wachtrij is bron- en de tijd die in inkomend verkeer wordt verwerkt.      |
|**Inkomend verkeer ontvangen vertraging van het aantal berichten**    |  Verschil tussen het volgnummer van bericht van de laatste in de wachtrij geplaatste gegevensbron in de gebeurtenis partitie en volgorde aantal bericht in de inkomende gegevens dat wordt verwerkt.      |


![Latentie](media/environment-mitigate-latency/latency.png)

Als u wordt beperkt, ziet u een waarde voor de *tijdsinterval inkomend verkeer ontvangen bericht*, melding van het aantal seconden achter TSI afkomstig van de werkelijke tijd het bericht is komt binnen via de bron van de gebeurtenis (met uitzondering van indexering tijd van appx. 30 tot 60 seconden).  *Inkomend verkeer ontvangen aantal berichten Lag* moet ook een waarde, zodat u kunt om te bepalen hoeveel berichten achter u zijn.  De eenvoudigste manier om het weer op de hoogte is om de capaciteit van uw omgeving in een grootte die u kunt strijden tegen het verschil te vergroten.  

Als u een omgeving met één eenheid S1 en Zie dat er een vertraging van vijf miljoen bericht is, kan u bijvoorbeeld de grootte van uw omgeving aan zes eenheden voor rond een dag te verhogen.  U kunt verhogen nog verder catch van sneller.  De periode achterstallige is vaak het geval bij het inrichten in eerste instantie van een omgeving, met name wanneer u verbinding maken met een gebeurtenisbron die al gebeurtenissen in het of wanneer u bulksgewijs uploaden van grote hoeveelheden historische gegevens.

Een andere methode is het instellen van een **opgeslagen Ingangsgebeurtenissen** waarschuwing > = een drempelwaarde iets onder de capaciteit van de totale omgeving voor een periode van twee uur.  Deze waarschuwing krijgt u inzicht in als u voortdurend zijn op capaciteit, waarmee een grote kans dat de latentie wordt aangegeven.  

Bijvoorbeeld, als u hebt drie S1-eenheden ingericht (of 2100 gebeurtenissen per minuut opnamecapaciteit), kunt u instellen een **opgeslagen Ingangsgebeurtenissen** voor waarschuwing > = 1900 gebeurtenissen gedurende 2 uur. Als u voortdurend van meer dan deze drempelwaarde, en daarom de waarschuwing wordt geactiveerd, bent u waarschijnlijk onder-ingericht.  

Ook als u vermoedt u wordt beperkt dat, u kunt vergelijken met uw **inkomend verkeer ontvangen berichten** met uw gebeurtenis bron berichten de egressed.  Als u inkomend verkeer naar uw Event Hub is groter dan uw **inkomend verkeer ontvangen berichten**, uw Time Series Insights zijn waarschijnlijk wordt beperkt.

## <a name="improving-performance"></a>Prestaties verbeteren 
Als u wilt beperken of ondervindt latentie verminderen, is de beste manier om deze te corrigeren om de capaciteit van uw omgeving te vergroten. 

U kunt voorkomen dat latentie en beperkingen door het correct configureren van uw omgeving voor de hoeveelheid gegevens die u wilt analyseren. Zie voor meer informatie over het toevoegen van capaciteit voor uw omgeving [schalen van uw omgeving](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Volgende stappen
- Voor extra stappen voor probleemoplossing [vaststellen en oplossen van problemen in uw Time Series Insights-omgeving](time-series-insights-diagnose-and-solve-problems.md).
- Voor verdere ondersteuning, een gesprek starten op de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). U kunt ook contact opnemen met [ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor ondersteuning.
