---
title: 'Azure Stream Analytics: Begrijpen en Streaming-eenheden aanpassen | Microsoft Docs'
description: Begrijpen welke factoren invloed hebben op prestaties in Azure Stream Analytics.
keywords: Streaming-eenheid, de prestaties van query 's
services: stream-analytics
documentationcenter: 
author: JSeb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: e8812f10662ee7b571e8e353074c2537d1a3181b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="understand-and-adjust-streaming-units"></a>Begrijpen en aanpassen van Streaming-eenheden

Azure Stream Analytics maakt een aggregatie van de prestaties 'gewicht' van een taak die worden uitgevoerd in de Streaming-eenheden (SUs). SUs vertegenwoordigen de computerbronnen die worden gebruikt voor het uitvoeren van een taak. SU's bieden een manier om de relatieve capaciteit voor het verwerken van gebeurtenissen te beschrijven op basis van gecombineerde resultaten van de metingen van CPU, geheugen en de lees- en schrijfsnelheid. Met deze capaciteit kunt u zich richten op de query logica en verwijdert u hoeven weten opslag servicetier Prestatieoverwegingen, handmatig geheugen toewijzen voor uw werk en geschatte het core-aantal CPU's die nodig zijn voor uw werk tijdig.

Om te kunnen bereiken lage latentie streaming verwerking, wordt in Azure Stream Analytics-taken alle bewerkingen uitvoeren in het geheugen. Als u onvoldoende geheugen, mislukt de streaming-taak. Als gevolg hiervan voor een productietaak is het belangrijk om te controleren van het Resourcegebruik van een streaming-taak en zorg ervoor dat er onvoldoende bronnen om te voorkomen dat de 24/7-taken zijn toegewezen.

De meetwaarde is een getal van 0% percentage tot 100%. De meetwaarde SU % gebruik is voor een streaming-taak met minimale footprint meestal tussen 10 tot 20%. Het is raadzaam de metriek hieronder 80% om incidentele pieken rekening te houden.  U kunt een waarschuwing instellen voor de metriek (Zie [hier metrische waarschuwingen instellen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)).



## <a name="configure-stream-analytics-streaming-units-sus"></a>Stream Analytics Streaming-eenheden (SUs) configureren
1. Aanmelden bij [Azure-portal](http://portal.azure.com/)
2. Zoek de Stream Analytics-taak die u wilt schalen en open het in de lijst met resources. 
3. Op de pagina taak configureren, klikt u op **Scale**. 

    ![Azure Stream Analytics-taak Portalconfiguratie][img.stream.analytics.preview.portal.settings.scale]
    
4. Gebruik de schuifregelaar om in te stellen de SUs voor de taak. U ziet dat u beperkt tot specifieke SU-instellingen. 


## <a name="monitor-job-performance"></a>Prestaties van de taak controleren
U kunt de doorvoer van een taak met de Azure-portal volgen:

![Azure Stream Analytics-taken voor monitor][img.stream.analytics.monitor.job]

Berekenen van de verwachte doorvoer van de werkbelasting. Als de doorvoer kleiner is dan verwacht, afstemmen van de invoer-partitie, afstemmen van de query en SUs toevoegen aan uw project.


## <a name="how-many-sus-are-required-for-a-job"></a>Hoeveel SUs zijn vereist voor een taak?

Het aantal vereiste SUs voor een bepaalde taak kiezen, is afhankelijk van de partitieconfiguratie voor de invoer en de query die gedefinieerd in de taak. De **Scale** pagina kunt u het juiste aantal SUs instellen. Het is een best practice om toe te wijzen meer SUs dan nodig. De Stream Analytics-verwerkingsengine wordt geoptimaliseerd voor latentie en doorvoer koste toewijzen van extra geheugen.

In het algemeen de beste manier is om te beginnen met 6 SUs voor query's die geen gebruikmaken van **PARTITION BY**. Vervolgens de spot sweet bepalen door een vallen en opstaan methode waarin u het aantal SUs wijzigt nadat u representatieve hoeveelheden gegevens doorgeven en de SU-% gebruik metrische gegevens bekijkt.

Zie voor meer informatie over het kiezen van het juiste aantal SUs deze pagina: [Scale Azure Stream Analytics-taken te verhogen doorvoer](stream-analytics-scale-jobs.md)

> [!Note]
> Hoeveel SUs kiezen zijn vereist voor een bepaalde taak is afhankelijk van de partitieconfiguratie van de voor de invoer en de query gedefinieerd voor de taak. U kunt maximaal uw quotum in SUs voor een taak selecteren. Elk Azure-abonnement heeft standaard een quotum van maximaal 200 SUs voor alle analytics-taken in een specifieke regio. Als u wilt SUs voor uw abonnementen buiten dit quotum verhogen, neem contact op met [Microsoft Support](http://support.microsoft.com). Geldige waarden voor SUs per taak zijn 1, 3, 6, en omhoog in stappen van 6.



## <a name="factors-increasing-su-utilization"></a>Factoren SU % gebruik verhogen 
### <a name="stateful-query-logic"></a>Stateful query logica 
Een van de unieke mogelijkheden van Azure Stream Analytics-taak is stateful processen, zoals functies in vensters en tijdelijke joins, tijdelijke analytische functies uitvoeren. Elk van deze operators houdt sommige staten. 
#### <a name="windowed-aggregates"></a>Functies in vensters
De grootte van de status van een vensters aggregaat is evenredig met het aantal groepen (kardinaliteit) in de group by-operator. Bijvoorbeeld, is het aantal clusterid gekoppeld in de volgende query de kardinaliteit van de query. 

    SELECT count(*)
    FROM input 
    GROUP BY  clusterid, tumblingwindow (minutes, 5)


Om de problemen veroorzaakt door hoge kardinaliteit in de vorige query ameliorate, kunt u gebeurtenissen verzenden naar Event Hub gepartitioneerd door '' clusterid'' en scale-out van de query doordat het systeem voor het verwerken van elke invoer partitie afzonderlijk met **PARTITION BY**  zoals weergegeven in het voorbeeld hieronder:


    SELECT count(*) 
    FROM PARTITION BY PartitionId
    GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)

Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Hierdoor het aantal clusterid binnenkort in elk knooppunt beperkt waardoor de kardinaliteit van de groep met operator. 

Event Hub partities moeten worden gepartitioneerd door de groeperingssleutel om te voorkomen dat de noodzaak van een verkleiningsstap. Aanvullende gegevens worden behandeld [hier](https://docs.microsoft.com/azure/event-hubs/event-hubs-overview). 
#### <a name="temporal-join"></a>Tijdelijke join
De grootte van de status van een tijdelijke join is evenredig met het aantal gebeurtenissen in de tijdelijke Verjaardag ruimte van de join, invoer snelheid van gebeurtenissen vermenigvuldigen met de grootte van de verjaardag kamer is. 

Het aantal niet-overeenkomende gebeurtenissen in de join invloed hebben op het geheugengebruik voor de query. Met de volgende query wordt gezocht naar advertentieweergaven waarmee klikken worden gegenereerd:

    SELECT id
    FROM clicks 
    INNER JOIN, impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.

In dit voorbeeld is het mogelijk dat veel advertenties worden weergegeven en klikt u op de enkele personen en dit vereist is dat alle gebeurtenissen in het tijdvenster. Het verbruikte geheugen is gerelateerd aan de venstergrootte en de snelheid waarmee gebeurtenissen elkaar opvolgen. 

Dit verhelpen, kunt u gebeurtenissen verzenden naar Event Hub gepartitioneerd door de join-sleutels (id in dit geval) en de scale-out van de query doordat het systeem voor het verwerken van elke invoer partitie afzonderlijk met **PARTITION BY** zoals wordt weergegeven:

    SELECT id
    FROM clicks PARTITION BY PartitionId
    INNER JOIN impressions PARTITION BY PartitionId 
    ON impression.PartitionId = clocks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
</code>

Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Hierdoor is het aantal gebeurtenissen die afkomstig zijn in elk knooppunt beperkt, waardoor de grootte van de status wordt bijgehouden in het venster join. 
#### <a name="temporal-analytic-function"></a>Tijdelijke analytische functie
De grootte van de status van een tijdelijke analytische functie is evenredig aan de snelheid van gebeurtenissen vermenigvuldigen door de duur. Het herstel is vergelijkbaar met de tijdelijke join. U kunt schalen om de query via **PARTITION BY**. 

### <a name="out-of-order-buffer"></a>De bufferruimte volgorde 
Gebruiker kan de volgorde buffergrootte configureren in het gebeurtenislogboek ordening deelvenster configuratie. De buffer die wordt gebruikt voor invoer voor de duur van het venster houdt en ze opnieuw indelen. De grootte van de buffer is evenredig aan de snelheid van gebeurtenissen invoer vermenigvuldigen met de volgorde venstergrootte. De standaardgrootte van het venster is 0. 

Als u wilt herstellen, dit, uitbreiden query met **PARTITION BY**. Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Als een resultaat is het aantal gebeurtenissen die afkomstig zijn in elk knooppunt beperkt, waardoor het aantal gebeurtenissen in elke volgorde buffer. 

### <a name="input-partition-count"></a>Aantal partities voor invoer 
Elke partitie invoer van een taak invoer heeft een buffer. De groter aantal invoer partities, de meer resource wordt de taak verbruikt. Voor elke SU kan Azure Stream Analytics ongeveer 1 MB/s van invoer, verwerken, zodat u kunt overeenkomen met ASA SU aantal met het nummer van de partitie van uw Event Hub. Taak 1SU is meestal voldoende voor een Event Hub met 2 partities (dit is de minimale voor Event Hub). Als de Event Hub meer partities heeft, worden uw taak ASA meer bronnen worden verbruikt, maar niet noodzakelijkerwijs gebruikt de extra doorvoer geleverd door de Event Hub. Voor een taak 6SU moet u 4 of 8 partities van de Event Hub. Met behulp van een Event Hub met 16 partities of groter zijn in een 1SU taak vaak draagt bij aan overmatige Resourcegebruik, en moet worden vermeden. 

### <a name="reference-data"></a>Referentiegegevens 
Referentiegegevens in ASA zijn geladen in het geheugen voor snelle zoekacties. Met de huidige implementatie houdt elke join-bewerking met referentiegegevens een kopie van de referentiegegevens in het geheugen, zelfs als het lid met dezelfde verwijzingsgegevens meerdere keren. Voor query's met **PARTITION BY**, elke partitie heeft een kopie van de referentiegegevens zodat de partities volledig ontkoppelde zijn. Met het effect vermenigvuldiger kan geheugengebruik downloaden zeer hoge als u deelneemt aan referentiegegevens worden meerdere keren met meerdere partities.  

#### <a name="use-of-udf-functions"></a>Gebruik van de UDF-functies
Wanneer u een UDF-functie toevoegt, wordt de JavaScript-runtime in Azure Stream Analytics geladen in het geheugen. Dit heeft invloed op de SU %.


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Worden query's maken in Azure Stream Analytics](stream-analytics-parallelization.md)
* [Azure Stream Analytics-taken voor een betere doorvoer schalen](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
