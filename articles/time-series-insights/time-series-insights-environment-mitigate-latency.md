---
title: Het controleren en te verminderen beperking in Azure Time Series Insights | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt controleren, diagnoses stellen en prestatieproblemen waardoor latentie en beperking in Azure Time Series Insights beperken.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: jasonh
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/27/2017
ms.openlocfilehash: 35860838d03d61e1145d35fd2516c1688c3bb64f
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130577"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Bewaken en te verhelpen bandbreedtebeperking om de latentie in Azure Time Series inzichten te verminderen
Wanneer de hoeveelheid binnenkomende gegevens groter is dan de configuratie van uw omgeving, kunnen wachttijden of beperking in Azure Time Series Insights optreden.

U kunt voorkomen dat latentie en beperkingen door het instellen van uw omgeving voor de hoeveelheid gegevens die u wilt analyseren.

Hoogstwaarschijnlijk optreden latentie en beperking wanneer u:

- Toevoegen van een gebeurtenisbron dat oude gegevens die overschrijdt misschien de snelheid waarmee u inkomend toegewezen bevat (tijd reeks inzichten nodig te lopen).
- Voeg meer bronnen van gebeurtenissen in een omgeving, wat resulteert in een piek van aanvullende gebeurtenissen (die kan groter zijn dan de capaciteit van uw omgeving).
- Grote hoeveelheden historische gebeurtenissen pushen naar een gebeurtenisbron, wat resulteert in een vertraging (tijd reeks inzichten nodig te lopen).
- Lid worden van referentiegegevens met telemetrie, wat leidt tot grotere gebeurtenisgrootte.  Vanuit een bandbreedteregeling perspectief wordt een ingressed gegevenspakket met een pakketgrootte 32 kB wordt behandeld als 32-gebeurtenissen, elke grootte van 1 KB. De grootte van de maximum toegestane gebeurtenis is 32 KB; gegevenspakketten die groter zijn dan 32 KB worden afgekapt.


## <a name="monitor-latency-and-throttling-with-alerts"></a>Monitor latentie en beperking met waarschuwingen

Waarschuwingen kunnen u helpen te opsporen en latentieproblemen veroorzaakt door uw omgeving te beperken. 

1. Klik in de Azure-portal op **metrische gegevens**. 

   ![Metrische gegevens](media/environment-mitigate-latency/add-metrics.png)

2. Klik op **metrische waarschuwing toevoegen**.  

    ![Metrische waarschuwing toevoegen](media/environment-mitigate-latency/add-metric-alert.png)

Daar kunt u waarschuwingen met behulp van de volgende metrische gegevens:

|Gegevens  |Beschrijving  |
|---------|---------|
|**Inkomend ontvangen Bytes**     | Het aantal onbewerkte bytes lezen uit bronnen van gebeurtenissen. Onbewerkte telling omvat gewoonlijk de eigenschapsnaam en waarde.  |  
|**Inkomend ontvangen ongeldige berichten**     | Aantal ongeldige berichten lezen uit alle bronnen van Azure Event Hubs of Azure IoT Hub gebeurtenissen.      |
|**Inkomende berichten ontvangen**   | Aantal berichten lezen uit alle Event Hubs of IoT Hubs bronnen van gebeurtenissen.        |
|**Inkomend opgeslagen Bytes**     | Totale grootte van gebeurtenissen die zijn opgeslagen en beschikbaar voor query. De grootte wordt berekend alleen op de waarde van eigenschap.        |
|**Inkomend opgeslagen gebeurtenissen**     |   Het aantal gebeurtenissen dat platte opgeslagen en beschikbaar voor query.      |
|**Inkomend ontvangen bericht tijdsinterval**    |  Verschil in seconden tussen het moment dat het bericht in de wachtrij in de gebeurtenisstroom is bron- en de tijd die in inkomend wordt verwerkt.      |
|**Inkomend ontvangen vertraging van het aantal berichten**    |  Verschil tussen het volgnummer van de laatste in de wachtrij bericht gegevensbron in het logboek partitie en volgorde aantal berichten in inkomend wordt verwerkt.      |


![Latentie](media/environment-mitigate-latency/latency.png)

Als u wordt beperkt, ziet u een waarde op voor de *tijdsverschil inkomend ontvangen bericht*, melding van hoeveel seconden achter TSI afkomstig van de werkelijke tijd het bericht is treffers in de gegevensbron (met uitzondering van indexering tijd van appx. 30 tot 60 seconden).  *Inkomend ontvangen aantal berichten Lag* ook een waarde, zodat u kunt bepalen hoeveel berichten achter u moet hebben.  De eenvoudigste manier om u te verleiden tot het is om de capaciteit van uw omgeving aan met een grootte waarmee u kunt het verschil te overwinnen te vergroten.  

Als u een omgeving met één eenheid S1 hebt en u ziet dat er een vertraging van vijf miljoen bericht, kan u bijvoorbeeld de grootte van uw omgeving aan zes eenheden voor rond een dag te verhogen.  Kan verhoogt u zelfs verder catch up sneller.  De periode bijwerken is vaak het geval bij het inrichten in eerste instantie van een omgeving met name wanneer u een verbinding maken met een gebeurtenisbron die al gebeurtenissen in het of wanneer u het uploaden van veel van historische gegevens bulksgewijs.

Een andere methode is het instellen van een **opgeslagen Ingangsgebeurtenissen** waarschuwing > = een drempelwaarde iets onder de capaciteit van de totale omgeving voor een periode van twee uur.  Deze waarschuwing vindt u informatie over als u zijn voortdurend op maximale capaciteit, waarmee wordt aangegeven van een hoge kans latentie.  

Bijvoorbeeld, als u hebt drie S1 eenheden ingericht (of 2100 gebeurtenissen per minuut inkomend capaciteit), kunt u instellen een **opgeslagen Ingangsgebeurtenissen** waarschuwing voor > = 1900 gebeurtenissen gedurende 2 uur. Als u voortdurend van meer dan deze drempelwaarde, en daarom activering van de waarschuwing, u waarschijnlijk onder-ingericht.  

Ook als u denkt u wordt beperkt dat, u kunt vergelijken met uw **inkomend ontvangen berichten** met uw gebeurtenis bron berichten de egressed.  Als inkomende gegevens naar uw Event Hub groter dan is uw **inkomend ontvangen berichten**, zijn waarschijnlijk inzicht in uw tijd reeks gegevens wordt beperkt.

## <a name="improving-performance"></a>Verbeterde prestaties 
Als u beperking of latentie ondervindt, is de beste manier om het te corrigeren om uw omgeving capaciteit te vergroten. 

U kunt voorkomen dat latentie en beperkingen door het instellen van uw omgeving voor de hoeveelheid gegevens die u wilt analyseren. Zie voor meer informatie over hoe u capaciteit toevoegt aan uw omgeving, [schalen van uw omgeving](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Volgende stappen
- Voor extra stappen voor probleemoplossing [diagnosticeren en oplossen van problemen in uw omgeving Time Series Insights](time-series-insights-diagnose-and-solve-problems.md).
- Voor aanvullende ondersteuning door een gesprek te starten op de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) of [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). U kunt ook contact opnemen met [ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor ondersteuning.
