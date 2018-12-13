---
title: Azure Stream Analytics-uitvoer oplossen
description: Dit artikel wordt beschreven technieken voor het oplossen van de uitvoer-verbindingen in Azure Stream Analytics-taken.
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 92cb427149e6e6cbddfb96c6e4488017641e6482
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53164905"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Azure Stream Analytics-uitvoer oplossen

Deze pagina worden veelvoorkomende problemen met verbindingen van de uitvoer en het oplossen van problemen en los deze beschreven.

## <a name="output-not-produced-by-job"></a>Uitvoer niet geproduceerd door de taak 
1.  Controleer de verbinding met de uitvoer met behulp van de **testverbinding** knop voor elke uitvoer.

2.  Bekijk [ **metrische bewakingsgegevens** ](stream-analytics-monitoring.md) op de **Monitor** tabblad. Omdat de waarden worden samengevoegd, worden de metrische gegevens die door een paar minuten vertraagd.
    - Als Invoergebeurtenissen > 0, de taak is niet lezen van invoergegevens. Als Invoergebeurtenissen niet > 0 is, klikt u vervolgens:
      - Als u wilt zien of de gegevensbron geldige gegevens bevat, controleren met behulp van [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Deze controle is van toepassing als de taak van Event Hub als invoer gebruikmaakt.
      - Controleer of de gegevens serialisatie-indeling en gegevenscodering zijn zoals verwacht.
      - Als de taak een Event Hub gebruikt wordt, controleert u of de hoofdtekst van het bericht is *Null*.
      
    - Als Gegevensconversiefouten > 0 en stijgt, kan het volgende waar zijn:
      - De uitvoergebeurtenis komt niet overeen met het schema van de doel-sink. 
      - Het gebeurtenissenschema mogelijk niet overeen met het opgegeven of verwachte schema van de gebeurtenissen in de query.
      - De gegevenstypen van sommige velden in de gebeurtenis mogelijk niet overeen met de verwachtingen.
      
    - Als runtimefouten > 0, betekent dit dat de taak de gegevens kan worden ontvangen maar fouten tijdens het verwerken van de query wordt gegenereerd.
      - Als u de fouten zoekt, gaat u naar de [auditlogboeken](../azure-resource-manager/resource-group-audit.md) en filtert u op *mislukt* status.
      
    - Als invoergebeurtenissen groter is dan > 0 en Uitvoergebeurtenissen = 0, betekent dit dat een van de volgende geldt:
      - Het verwerken van de query heeft geen uitvoergebeurtenissen opgeleverd.
      - Gebeurtenissen of de bijbehorende velden is mogelijk onjuist, wat resulteert in een lege uitvoer na verwerking van query's.
      - De taak is kan geen gegevens naar het uitvoereindpunt worden verplaatst om connectiviteit of verificatie redenen pushen.
      
    - In alle hierboven genoemde foutgevallen bewerkingenlogbestandberichten extra details gegeven (waaronder wat er gebeurt), behalve in gevallen waarbij de querylogica alle gebeurtenissen gefilterd. Als de verwerking van meerdere gebeurtenissen fouten genereert, registreert Stream Analytics de eerste drie foutberichten van hetzelfde type binnen 10 minuten Operations-Logboeken. Vervolgens onderdrukt andere identieke fouten met een bericht dat wordt gelezen "Fouten plaatsvinden te snel, dat deze worden onderdrukt."
    
## <a name="job-output-is-delayed"></a>Taakuitvoer is vertraagd

### <a name="first-output-is-delayed"></a>Eerste uitvoer is vertraagd
Wanneer een Stream Analytics-taak wordt gestart, wordt de invoer gebeurtenissen worden gelezen, maar kan er een vertraging in de uitvoer wordt geproduceerd in bepaalde omstandigheden.

Grote tijdwaarden in tijdelijke query-elementen kunnen bijdragen aan de uitvoer vertraging. Juiste uitvoer produceren via de grote tijdvensters, de streaming-taak wordt gestart met het lezen van gegevens van de meest recente tijdstip mogelijke (maximaal zeven dagen geleden) om op te vullen van het tijdvenster. Gedurende die tijd, wordt geen uitvoer geproduceerd totdat de achterstallige lezen van de openstaande invoer gebeurtenissen voltooid is. Dit probleem kan het oppervlak wanneer het systeem de streaming taken, dus de taak opnieuw starten wordt bijgewerkt. Dergelijke upgrades optreden in het algemeen eenmaal elke aantal maanden. 

Dus wees voorzichtig bij het ontwerpen van uw Stream Analytics-query. Als u een groot tijdsvenster (meer dan enkele uren, maximaal zeven dagen) voor de tijdelijke-elementen in de query-syntaxis van de taak, kan dit leiden tot een vertraging in de eerste uitvoer wanneer de taak wordt gestart of opnieuw opgestart.  

Een beperking voor dit type van de eerste uitvoer vertraging is het gebruik van query-parallellisatie technieken (partitionering van gegevens) of Voeg meer Streaming-eenheden voor de verbetering van de doorvoer totdat de taak de resultaten.  Zie voor meer informatie, [overwegingen bij het maken van Stream Analytics-taken](stream-analytics-concepts-checkpoint-replay.md)

Deze factoren van invloed zijn op de actualiteit van de eerste uitvoer die wordt gegenereerd:

1. Gebruik van statistische functies in vensters, (groep door van Tumbling, Hopping plaatsvindt, en schuiven windows)
   - Voor tumbling of hopping venster statistische functies, worden de resultaten aan het einde van het venster tijdsbestek gegenereerd. 
   - Voor een sliding window, worden de resultaten worden gegenereerd wanneer een gebeurtenis binnengaat of de sliding window van verlaat. 
   - Als u van plan bent te grote venstergrootte (> 1 uur) gebruikt, is het beste venster verspringen of sliding kiezen zodat u kunt de uitvoer vaker zien.

2. Gebruik van tijdelijke joins (lid worden met DATEDIFF)
   - Overeenkomsten worden gegenereerd als wanneer beide zijden van de overeenkomende gebeurtenissen aankomen.
   - Gegevens die beschikt niet over een overeenkomst (LEFT OUTER JOIN) is gegenereerd aan het einde van het venster DATEDIFF met betrekking tot elke gebeurtenis aan de linkerkant.

3. Gebruik van tijdelijke analytische functies (ISFIRST, LAST en LAG met LIMIT DURATION)
   - De uitvoer voor elke gebeurtenis wordt gegenereerd, is er geen vertraging voor analytische functies.

### <a name="output-falls-behind"></a>Uitvoer valt achter
Tijdens normale werking van de taak als u van de taak uitvoer valt achter (langer latentie), u kunt met deze functie de hoofdoorzaken aan de hand van deze factoren:
- Of de downstream-sink is beperkt
- Of de upstream-bron is beperkt
- Of de verwerkingslogica in de query is rekenintensieve

Als u wilt zien van de gegevens, in de Azure-portal, selecteert u de streaming-taak en selecteer de **taakdiagram**. Voor elke invoer, is er een per partitie achterstand gebeurtenis metrisch gegeven. Als de achterstand gebeurtenis metrische gegevens stijgen blijft, is dit een indicatie dat de systeemresources die zijn beperkt. Mogelijk die het gevolg is van uitvoer-sink beperking of intensief CPU. Zie voor meer informatie over het gebruik van het taakdiagram [gegevensgestuurde foutopsporing met behulp van het taakdiagram](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Waarschuwing voor schending van de sleutel met Azure SQL Database-uitvoer

Als u Azure SQL-database als uitvoer naar een Stream Analytics-taak configureert, voegt het bulksgewijs records in de doeltabel. In het algemeen, Azure stream analytics garandeert [ten minste één keer levering]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) naar het uitvoereindpunt worden verplaatst, een kunt nog steeds [precies bereiken-bezorging]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) om SQL uitvoer wanneer SQL-tabel heeft een unique-beperking gedefinieerd. 

Wanneer unique key-beperkingen zijn ingesteld op de SQL-tabel en er zijn dubbele records worden ingevoegd in de SQL-tabel, geeft Azure Stream Analytics de dubbele record wordt verwijderd. Deze splitst de gegevens in batches en recursief de batches voegen tot één dubbele record wordt gevonden. Als de streaming-taak heeft een groot aantal dubbele rijen, deze splitsen en in te voegen proces is de duplicaten één voor één, dit minder efficiënt en tijdrovend is negeren. Als u meerdere waarschuwingsberichten voor schending van de sleutel in het activiteitenlogboek is opgenomen in het afgelopen uur ziet, is het waarschijnlijk dat de SQL-uitvoer de hele taak vertragen. 

U lost dit probleem, moet u [configureren van de index]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) die de schending van de sleutel wordt veroorzaakt doordat de optie IGNORE_DUP_KEY. Als u deze optie inschakelt, kunnen dubbele waarden moeten worden genegeerd door SQL tijdens bulksgewijs invoegen en SQL Azure levert gewoon een waarschuwingsbericht wordt weergegeven in plaats van een fout. Azure Stream Analytics produceren schending van de primaire sleutel fouten niet meer.

Houd rekening met de volgende opmerkingen bij het configureren van IGNORE_DUP_KEY voor verschillende soorten indexen:

* U kunt IGNORE_DUP_KEY niet instellen op een primaire sleutel of een unique-beperking die gebruikmaakt van ALTER INDEX, moet u verwijderen en opnieuw maken van de index.  
* U kunt de optie IGNORE_DUP_KEY voor een unieke index, die verschilt van de primaire sleutel/UNIQUE-beperking en gemaakt met behulp van de definitie van CREATE INDEX of INDEX met behulp van ALTER INDEX instellen.  
* Ignore_dup_key niet van toepassing op de columnstore-indexen omdat uniekheid op dergelijke indexen kunnen niet worden afgedwongen.  

## <a name="get-help"></a>Help opvragen

Voor verdere ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
