---
title: Veelvoorkomende problemen oplossen in Azure Stream Analytics
description: Dit artikel beschrijft enkele veelvoorkomende problemen in Azure Stream Analytics en stappen om op te lossen die problemen.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: d7784792cd7e1475028dc0a5e9664916f1d45a07
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699216"
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Veelvoorkomende problemen in Stream Analytics en stappen om op te lossen

## <a name="troubleshoot-malformed-input-events"></a>Oplossen van gebeurtenissen voor ongeldige invoer

 Serialisatieproblemen worden veroorzaakt wanneer de invoerstroom van uw Stream Analytics-taak onjuiste berichten bevat. Bijvoorbeeld, een onjuist ingedeeld bericht kan worden veroorzaakt door een ontbrekend haakje of een ontbrekend haakje in een JSON-object, of is onjuist stempel tijdnotatie in een tijd-veld. 
 
 Wanneer een Stream Analytics-taak ontvangt een onjuist ingedeeld bericht uit de invoer, wordt het bericht verwijderd en ontvangt de gebruiker een waarschuwing. Er wordt een waarschuwingssymbool weergegeven op de **invoer** tegel van uw Stream Analytics-taak (deze waarschuwing aanmelding bestaat, zolang de taak wordt uitgevoerd):

![Tegel invoer](media/stream-analytics-malformed-events/inputs_tile.png)

Als u meer informatie, schakel de diagnostische logboeken om de details van de waarschuwing weer te geven. Voor onjuist gevormde invoergebeurtenissen, bevatten de logboeken van de uitvoering van een vermelding met het bericht dat lijkt op: "bericht: kan de invoergebeurtenis(sen) van resource niet deserialiseren <blob URI> als json '. 

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

1. Navigeer naar de tegel invoer en klik op om waarschuwingen weer te geven.

2. De tegel invoer details toont een set waarschuwingen weergegeven met informatie over het probleem. Hieronder volgt een voorbeeld van de waarschuwing, de waarschuwing bevat de partitie, Offset en volgnummers waarbij er ongeldige JSON-gegevens. 

   ![Waarschuwing met offset](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Als u de JSON-gegevens die heeft een onjuiste indeling, voert u de code CheckMalformedEvents.cs. In dit voorbeeld is beschikbaar in de [opslagplaats met GitHub-voorbeelden](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Deze code leest de partitie-ID, verschuiving en de gegevens die zich in deze offset af te drukken. 

4. Zodra de gegevens zijn gelezen, kunt u de serialisatie-indeling analyseren en corrigeren.

5. U kunt ook [gebeurtenissen worden gelezen vanuit een IoT-Hub met de Service Bus Explorer](https://code.msdn.microsoft.com/How-to-read-events-from-an-1641eb1b).

## <a name="delayed-output"></a>Vertraagde uitvoer

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

## <a name="handle-duplicate-records-in-azure-sql-database-output"></a>Verwerking van dubbele records in Azure SQL Database-uitvoer

Als u Azure SQL-database als uitvoer naar een Stream Analytics-taak configureert, voegt het bulksgewijs records in de doeltabel. In het algemeen, Azure stream analytics garandeert [ten minste één keer levering]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) naar het uitvoereindpunt worden verplaatst, een kunt nog steeds [precies bereiken-bezorging]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) om SQL uitvoer wanneer SQL-tabel heeft een unique-beperking gedefinieerd. 

Wanneer unique key-beperkingen zijn ingesteld op de SQL-tabel en er zijn dubbele records worden ingevoegd in de SQL-tabel, geeft Azure Stream Analytics de dubbele record wordt verwijderd. Deze splitst de gegevens in batches en recursief de batches voegen tot één dubbele record wordt gevonden. Als de streaming-taak heeft een groot aantal dubbele rijen, deze splitsen en in te voegen proces is de duplicaten één voor één, dit minder efficiënt en tijdrovend is negeren. Als u meerdere waarschuwingsberichten voor schending van de sleutel in het activiteitenlogboek is opgenomen in het afgelopen uur ziet, is het waarschijnlijk dat de SQL-uitvoer de hele taak vertragen. 

U lost dit probleem, moet u [configureren van de index]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) die de schending van de sleutel wordt veroorzaakt doordat de optie IGNORE_DUP_KEY. Als u deze optie inschakelt, kunnen dubbele waarden moeten worden genegeerd door SQL tijdens bulksgewijs invoegen en SQL Azure levert gewoon een waarschuwingsbericht wordt weergegeven in plaats van een fout. Azure Stream Analytics produceren schending van de primaire sleutel fouten niet meer.

Houd rekening met de volgende opmerkingen bij het configureren van IGNORE_DUP_KEY voor verschillende soorten indexen:

* U kunt IGNORE_DUP_KEY niet instellen op een primaire sleutel of een unique-beperking die gebruikmaakt van ALTER INDEX, moet u verwijderen en opnieuw maken van de index.  
* U kunt de optie IGNORE_DUP_KEY voor een unieke index, die verschilt van de primaire sleutel/UNIQUE-beperking en gemaakt met behulp van de definitie van CREATE INDEX of INDEX met behulp van ALTER INDEX instellen.  
* Ignore_dup_key niet van toepassing op de columnstore-indexen omdat uniekheid op dergelijke indexen kunnen niet worden afgedwongen.  

## <a name="next-steps"></a>Volgende stappen
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
