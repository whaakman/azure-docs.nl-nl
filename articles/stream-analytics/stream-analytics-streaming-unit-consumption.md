---
title: Begrijpen en aanpassen van Streaming-eenheden in Azure Stream Analytics
description: Dit artikel beschrijft de instelling van de Streaming-eenheden en andere factoren die invloed hebben op prestaties in Azure Stream Analytics.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: b7abbd486e9c357a5bdba093214a3801f88c39ab
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575895"
---
# <a name="understand-and-adjust-streaming-units"></a>Begrijpen en aanpassen van Streaming-eenheden

Streaming-eenheden (su's) vertegenwoordigt de computerbronnen die zijn toegewezen voor het uitvoeren van een taak. Hoe hoger de waarde van SUs, hoe meer resources van de CPU en geheugen worden toegewezen voor uw taak. Deze capaciteit kunt die u zich op de querylogica richten en de noodzaak voor het beheren van de hardware voor het uitvoeren van uw Stream Analytics-taak tijdig samenvattingen.

Als u wilt bereiken met lage latentie verwerking, uitvoeren Azure Stream Analytics-taken de verwerking in het geheugen. Bij het uitvoeren van onvoldoende geheugen, mislukt de streaming-taak. Als gevolg hiervan voor een productietaak is het belangrijk een streamingtaak Resourcegebruik te bewaken en zorg ervoor dat er voldoende resources toegewezen om te voorkomen dat de 24/7-taken.

De SU % gebruik metrische gegevens, die varieert van 0% tot 100%, wordt het geheugengebruik van uw workload beschreven. Deze waarde is voor een streaming-taak met minimale voetafdruk, meestal tussen 10 tot 20%. Als gebruikspercentage voor laag is en invoergebeurtenissen ophalen nog moeten worden uitgevoerd, moet uw workload waarschijnlijk meer rekenresources, waarvoor u het aantal SUs te verhogen. Het is raadzaam om te houden van de metriek SU dan 80% ter compensatie van af en toe pieken. Microsoft raadt aan om een waarschuwing instellen op 80% SU-gebruik metrische gegevens om te voorkomen dat bronuitputting. Zie voor meer informatie, [zelfstudie: waarschuwingen instellen voor Azure Stream Analytics-taken](stream-analytics-set-up-alerts.md).

## <a name="configure-stream-analytics-streaming-units-sus"></a>Stream Analytics Streaming-eenheden (su's) configureren
1. Aanmelden bij [Azure-portal](http://portal.azure.com/)

2. Zoek de Stream Analytics-taak die u wilt schalen en open het in de lijst met resources. 

3. In de pagina van de taak, onder de **configureren** kop, selecteer **schaal**. 

    ![Azure portal taakconfiguratie van Stream Analytics][img.stream.analytics.preview.portal.settings.scale]
    
4. Gebruik de schuifregelaar om in te stellen de su's voor de taak. U ziet dat u beperkt tot specifieke SU-instellingen bent. 

## <a name="monitor-job-performance"></a>Prestaties van de taak controleren
Met behulp van de Azure-portal, kunt u de doorvoer van een taak volgen:

![Bewaken van Azure Stream Analytics-taken][img.stream.analytics.monitor.job]

Bereken de verwachte doorvoer van de werkbelasting. Als de doorvoer kleiner is dan verwacht, de invoerpartitie af te stemmen, afstemmen van de query en su's toevoegen aan uw taak.

## <a name="how-many-sus-are-required-for-a-job"></a>Hoeveel su's zijn vereist voor een taak?

Het aantal vereiste su's voor een bepaalde taak kiezen, is afhankelijk van de partitieconfiguratie voor de invoer en de query die gedefinieerd in de taak. De **schaal** op de pagina kunt u het juiste aantal su's instellen. Er is een aanbevolen procedure om toe te wijzen meer SUs dan nodig is. De Stream Analytics-verwerkingsengine is geoptimaliseerd voor latentie en doorvoer leiden bij het toewijzen van extra geheugen.

In het algemeen is de aanbevolen procedure is om te beginnen met 6 su's voor query's die geen gebruik **PARTITION BY**. Vervolgens kunt u de positie sweet bepalen met behulp van een methode voor vallen en opstaan waarin u het aantal SUs wijzigen nadat u representatieve hoeveelheden gegevens doorgeven en het SU-% gebruik metrische gegevens bekijkt. Het maximum aantal streamingeenheden dat kan worden gebruikt door een Stream Analytics-taak is afhankelijk van het aantal stappen in de query die is gedefinieerd voor de taak en het aantal partities in elke stap. U kunt meer informatie over de limieten [hier](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#calculate-the-maximum-streaming-units-of-a-job).

Zie voor meer informatie over het kiezen van het juiste aantal su's op deze pagina: [Scale Azure Stream Analytics-taken om doorvoer te vergroten](stream-analytics-scale-jobs.md)

> [!Note]
> Hoeveel su's zijn vereist voor een bepaalde taak is afhankelijk van de partitieconfiguratie voor de invoer en de query die is opgegeven voor de taak. U kunt maximaal uw quotum in su's voor een taak selecteren. Elk Azure-abonnement heeft standaard een quotum van maximaal 200 su's voor alle analytics-taken in een bepaalde regio. Als u wilt verhogen su's voor uw abonnementen buiten dit quotum, neem contact op met [Microsoft Support](http://support.microsoft.com). Geldige waarden voor su's per taak zijn 1, 3, 6, en in stappen van 6.

## <a name="factors-that-increase-su-utilization"></a>Factoren die gebruikspercentage voor verhogen 

Tijdelijke (tijdgebaseerde) query-elementen zijn de kernset aan stateful operators geleverd door Stream Analytics. Stream Analytics beheert de status van deze bewerkingen intern namens gebruiker, door het beheer van geheugenverbruik, plaatsen van controlepunten voor tolerantie en herstel van de status tijdens upgrades van de service. Hoewel de Stream Analytics wordt volledig beheerd voor de Staten, zijn er een aantal aanbevolen werkwijzen besproken waarmee gebruikers rekening moeten houden.

## <a name="stateful-query-logic-in-temporal-elements"></a>Stateful querylogica in de tijdelijke elementen
Een van de unieke mogelijkheid van Azure Stream Analytics-taak is stateful verwerking, zoals statische functies in vensters, tijdelijke joins en tijdelijke analytische functies uit te voeren. Elk van deze operators houdt de statusinformatie. De maximale venstergrootte voor deze query-elementen is zeven dagen. 

Het concept tijdelijk venster wordt weergegeven in de verschillende elementen van de Stream Analytics-query:
1. Statistische functies in vensters: groep door van Tumbling, Hopping plaatsvindt, en schuiven windows

2. Tijdelijke joins: lid met de functie DATEDIFF

3. Tijdelijke analytische functies: ISFIRST, LAST en LAG met LIMIT DURATION

De volgende factoren van invloed zijn op het geheugen dat wordt gebruikt (deel van het streaming-eenheden metrische gegevens) door Stream Analytics-taken:

## <a name="windowed-aggregates"></a>Statistische functies in vensters
Het verbruikte geheugen (grootte van de gebruikersstatus) voor een in vensters statistische functie is niet altijd rechtstreeks in verhouding met de-venstergrootte. In plaats daarvan is het verbruikte geheugen evenredig aan de kardinaliteit van de gegevens of het aantal groepen in elke tijdvenster.


Bijvoorbeeld, in de volgende query, het aantal die zijn gekoppeld aan `clusterid` is de kardinaliteit van de query. 

   ```sql
   SELECT count(*)
   FROM input 
   GROUP BY  clusterid, tumblingwindow (minutes, 5)
   ```

Om te voorkomen die samenhangen problemen veroorzaakt door hoge kardinaliteit in de vorige query, stuurt u gebeurtenissen naar Event Hub gepartitioneerd op basis van `clusterid`, en scale-out van de query doordat het systeem voor het verwerken van elke invoer partitie afzonderlijk met **partitie DOOR** zoals wordt weergegeven in het voorbeeld hieronder:

   ```sql
   SELECT count(*) 
   FROM input PARTITION BY PartitionId
   GROUP BY PartitionId, clusterid, tumblingwindow (minutes, 5)
   ```

Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Als gevolg hiervan, het aantal `clusterid` waarden op elk knooppunt wordt verminderd waardoor de kardinaliteit van de groep met operator. 

Event Hub-partities moeten worden gepartitioneerd op de groepering-toets om te voorkomen dat de noodzaak voor een verkleiningsstap. Zie voor meer informatie, [Event Hubs-overzicht](../event-hubs/event-hubs-what-is-event-hubs.md). 

## <a name="temporal-joins"></a>Tijdelijke joins
Het verbruikte geheugen (grootte van de gebruikersstatus) van een tijdelijke join staat in verhouding met het aantal gebeurtenissen in de tijdelijke Verjaardag zaal van de join, dit is de snelheid van invoer vermenigvuldigen met de grootte van de ruimte verjaardag. Met andere woorden, is het geheugen die wordt gebruikt door samenvoegingen evenredig met het tijdsbereik voor DateDiff vermenigvuldigd met gemiddelde snelheid van gebeurtenissen.

Het aantal niet-overeenkomende gebeurtenissen in de join van invloed zijn op het geheugengebruik voor de query. Met de volgende query wordt gezocht naar advertentieweergaven waarmee klikken worden gegenereerd:

   ```sql
   SELECT clicks.id
   FROM clicks 
   INNER JOIN impressions ON impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10.
   ```

In dit voorbeeld is het mogelijk dat veel advertenties worden weergegeven en weinig mensen erop klikken en deze vereist is dat alle gebeurtenissen in het tijdvenster. Het verbruikte geheugen is gerelateerd aan de venstergrootte en de snelheid waarmee gebeurtenissen elkaar opvolgen. 

Om dit te verhelpen, kunt u gebeurtenissen verzenden naar Event Hub gepartitioneerd op basis van de join-sleutels (id in dit geval) en scale-out van de query doordat het systeem voor het verwerken van elke invoer partitie afzonderlijk met **PARTITION BY** zoals wordt weergegeven:

   ```sql
   SELECT clicks.id
   FROM clicks PARTITION BY PartitionId
   INNER JOIN impressions PARTITION BY PartitionId 
   ON impression.PartitionId = clicks.PartitionId AND impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10 
   ```

Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Als gevolg hiervan is het aantal gebeurtenissen op elk knooppunt verminderd waardoor de grootte van de status in het venster lid worden bewaard. 

## <a name="temporal-analytic-functions"></a>Tijdelijke analytische functies
Het verbruikte geheugen (grootte van de gebruikersstatus) van een tijdelijke analytische functie is evenredig met de snelheid van vermenigvuldigen door de duur. De hoeveelheid geheugen die worden gebruikt door de analytische functies is niet in verhouding met de grootte van het venster, maar in plaats daarvan partitioneren het aantal in elke periode.

Het herstel is vergelijkbaar met tijdelijke join. U kunt schalen om de query via **PARTITION BY**. 

## <a name="out-of-order-buffer"></a>Niet-geordende buffer 
Gebruiker kan de niet-geordende buffergrootte configureren in het geval van deelvenster configuratie het bestellen. De buffer wordt gebruikt voor het opslaan van invoer voor de duur van het venster en volgorde. De grootte van de buffer is evenredig aan de snelheid van invoer vermenigvuldigen door het formaat van het niet de juiste volgorde. De standaardgrootte van het venster is 0. 

Als u wilt herstellen overloop van de buffer niet de juiste volgorde, de schaal vergroten met behulp van query **PARTITION BY**. Wanneer de query is gepartitioneerd, wordt deze verspreid over verschillende knooppunten. Als gevolg hiervan is het aantal gebeurtenissen op elk knooppunt verminderd waardoor het aantal gebeurtenissen in de buffer voor opnieuw ordenen. 

## <a name="input-partition-count"></a>Aantal invoer partities 
Elke invoer partitie van een taak die de invoer heeft een buffer. Het grotere aantal invoer partities, de meer resource wordt de taak verbruikt. Voor elke streaming-eenheid, kan Azure Stream Analytics verwerken van ongeveer 1 MB/s van invoer. U kunt daarom optimaliseren door die overeenkomt met het nummer van Stream Analytics streaming-eenheden met het aantal partities in uw Event Hub. 

Een taak die is geconfigureerd met één streaming-eenheid is meestal voldoende voor een Event Hub met twee partities (dit is het minimale aantal voor Event Hub). Als de Event Hub meer partities heeft, worden uw Stream Analytics-taak meer bronnen verbruikt, maar niet per se maakt gebruik van de extra doorvoer is geleverd door de Event Hub. 

Voor een taak met 6 streaming-eenheden, moet u 4 of 8 partities van de Event Hub. Vermijd echter te veel onnodige partities omdat dat ervoor zorgt overmatig gebruik van resources dat. Bijvoorbeeld, een Event Hub met 16 partities of groter zijn in een Stream Analytics-taak waarvoor 1 streaming-eenheid. 

## <a name="reference-data"></a>Referentiegegevens 
Referentiegegevens in ASA worden geladen in het geheugen voor het snel opzoeken. Met de huidige implementatie houdt elke join-bewerking met referentiegegevens een kopie van de referentiegegevens in het geheugen, zelfs als u meerdere keren bij de dezelfde referentiegegevens. Voor query's met **PARTITION BY**, elke partitie heeft een kopie van de referentiegegevens, zodat de partities volledig elkaar zijn losgekoppeld zijn. Met het effect vermenigvuldiger ontvangt geheugengebruik snel zeer hoog als u bij referentiegegevens meerdere keren met meerdere partities.  

### <a name="use-of-udf-functions"></a>Gebruik van de UDF-functies
Wanneer u een UDF-functie toevoegt, worden de JavaScript-runtime in Azure Stream Analytics in geheugen geladen. Dit heeft invloed op de % SU.

## <a name="next-steps"></a>Volgende stappen
* [Worden opgestart query's maken in Azure Stream Analytics](stream-analytics-parallelization.md)
* [Schalen van Azure Stream Analytics-taken voor een betere doorvoer](stream-analytics-scale-jobs.md)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   
