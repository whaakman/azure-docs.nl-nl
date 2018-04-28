---
title: Veelvoorkomende problemen oplossen in de Azure Stream Analytics
description: Dit artikel worden enkele veelvoorkomende problemen in Azure Stream Analytics en de stappen die problemen kunt oplossen.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: e04d1072acee635235b0a5bd8465ca38c861017b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Veelvoorkomende problemen in de Stream Analytics en stappen op te lossen

## <a name="troubleshoot-malformed-input-events"></a>Ongeldige invoer gebeurtenissen oplossen

 Serialisatie problemen worden veroorzaakt wanneer de invoerstroom van de Stream Analytics-taak onjuiste berichten bevat. Bijvoorbeeld, een onjuist ingedeeld bericht kan worden veroorzaakt door een ontbrekende haakje of een haakje ontbreekt in een JSON-object of onjuist stempel tijdnotatie in een tijd-veld. 
 
 Wanneer een onjuist ingedeeld bericht een Stream Analytics-taak van een invoer ontvangt, verwijdert het bericht en de gebruiker met een waarschuwing ontvangt een melding. Een waarschuwingssymbool wordt weergegeven op de **invoer** tegel van de Stream Analytics-taak (als deze waarschuwing aanmelding bestaat, zolang de taak wordt uitgevoerd):

![Invoer-tegel](media/stream-analytics-malformed-events/inputs_tile.png)

Voor meer informatie, schakelt u de logboeken met diagnostische gegevens om de details van de waarschuwing weer te geven. Onjuiste invoer gebeurtenissen, bevatten de logboeken van de uitvoering van een item met het bericht dat lijkt op: ' bericht: kan de invoer gebeurtenis(sen) van resource niet deserialiseren <blob URI> als json '. 

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

1. Navigeer naar de invoer tegel en klik hier voor weergave van waarschuwingen.

2. De details van de invoer-tegel wordt een set van waarschuwingen met informatie over het probleem. Hieronder volgt een voorbeeld van de waarschuwing weergegeven, ziet u de waarschuwing de partitie, Offset en volgnummers wanneer er een ongeldige JSON-gegevens. 

   ![Waarschuwingsbericht weergegeven met verschuiving](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Als u de JSON-gegevens die door een onjuiste indeling heeft, voert u de code CheckMalformedEvents.cs. In dit voorbeeld is beschikbaar in de [GitHub-opslagplaats voor voorbeelden](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Deze code leest de partitie-ID, offset en de gegevens die zich in deze offset afdrukken. 

4. Zodra de gegevens zijn gelezen, kunt u de serialisatie-indeling analyseren en corrigeren. 

## <a name="delayed-output"></a>Vertraagde uitvoer

### <a name="first-output-is-delayed"></a>Eerste uitvoer is vertraagd
Wanneer een Stream Analytics-taak wordt gestart, wordt de invoer gebeurtenissen worden gelezen, maar kan er een vertraging in de uitvoer wordt geproduceerd in bepaalde omstandigheden.

Grote tijdwaarden in de tijdelijke query-elementen kunnen bijdragen aan de uitvoer vertraging. Juiste uitvoer via de grote tijdvensters produceren, de streaming-taak wordt gestart met lezen van gegevens van de meest recente tijd mogelijke (maximaal zeven dagen geleden) zodat deze het tijdvenster. Gedurende die tijd, wordt geen uitvoer geproduceerd tot het lezen van het bijwerken van de uitstaande invoer gebeurtenissen voltooid is. Dit probleem kunt surface wanneer het systeem wordt bijgewerkt de streaming-taken, waardoor de taak opnieuw te starten. Dergelijke upgrades optreden in het algemeen eenmaal elke aantal maanden. 

Dus wees voorzichtig bij het ontwerpen van uw Stream Analytics-query. Als u een grote tijdvenster (meer dan enkele uren maximaal zeven dagen) voor de tijdelijke elementen in de querysyntaxis van de taak kan leiden tot een vertraging op de eerste uitvoer wanneer de taak wordt gestart of opnieuw gestart.  

Een beperking voor dit type van de eerste uitvoer vertraging is het query-garandeert technieken (partitioneren van de gegevens), of Voeg meer Streaming-eenheden ter verbetering van de doorvoer totdat de taak de resultaten.  Zie voor meer informatie [overwegingen bij het maken van de Stream Analytics-taken](stream-analytics-concepts-checkpoint-replay.md)

Deze factoren invloed op de actualiteit van de eerste uitvoer die wordt gegenereerd:

1. Gebruik van functies in vensters (groep door van Tumbling, Hopping en Verschuivend windows)
   - Voor tumbling of hopping venster statistische functies, worden de resultaten gegenereerd aan het einde van het venster tijdsbestek. 
   - Voor een sliding window, worden de resultaten worden gegenereerd wanneer een gebeurtenis binnengaat of het sliding venster verlaat. 
   - Als u van plan bent te grote venstergrootte (> 1 uur) gebruikt, is het beste venster hopping of sliding kiezen, zodat u de uitvoer vaker ziet.

2. Gebruik van tijdelijke joins (JOIN met DATEDIFF)
   - Komt overeen met worden gegenereerd als bij het ontvangen van beide zijden van de overeenkomende gebeurtenissen.
   - Gegevens die beschikt niet over een overeenkomst (LINKER OUTER JOINS) is gegenereerd aan het einde van het venster DATEDIFF met betrekking tot elke gebeurtenis aan de linkerkant.

3. Gebruik van tijdelijke analytische functies (ISFIRST, LAST en LAG met LIMIT DURATION)
   - Voor analytische functies, de uitvoer voor elke gebeurtenis wordt gegenereerd, wordt er geen vertraging.

### <a name="output-falls-behind"></a>Uitvoer valt achter
Tijdens de normale werking van de taak als u de uitvoer van de taak valt achter (langer latentie), vindt kunt u speldenpunt de hoofdoorzaken aan de hand van deze factoren:
- Hiermee wordt aangegeven of de downstream-sink is beperkt
- Hiermee wordt aangegeven of de upstream-bron is beperkt
- De verwerking van logica in de query wordt aangegeven of rekenintensieve

Voor deze informatie, in de Azure portal, selecteer de streaming-taak en selecteer de **taak diagram**. Voor elke invoer; er is een per partitie achterstand gebeurtenis metriek. Als de achterstand gebeurtenis metriek verhogen houdt, is het een indicatie dat de resources worden beperkt. Mogelijk die het gevolg is van de uitvoer sink-beperking, of intensief CPU. Zie voor meer informatie over het gebruik van het diagram taak [gegevensgestuurde foutopsporing met behulp van het diagram taak](stream-analytics-job-diagram-with-metrics.md).

## <a name="handle-duplicate-records-in-azure-sql-database-output"></a>Verwerking van dubbele records in de uitvoer van de Azure SQL Database

Als u Azure SQL-database als uitvoer naar een Stream Analytics-taak configureert, voegt het massaal records in de doeltabel. In het algemeen wordt gegarandeerd dat Azure stream analytics [ten minste eenmaal levering]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) naar de uitvoerlocatie een kan nog steeds [exact bereiken-eenmalig worden afgeleverd]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) naar SQL uitvoer wanneer SQL-tabel heeft een unique-beperking gedefinieerd. 

Zodra de unique key-beperkingen zijn ingesteld op de SQL-tabel en er zijn dubbele records worden ingevoegd in SQL-tabel, is Azure Stream Analytics Hiermee verwijdert u de dubbele record. Deze splitst de gegevens in batches en recursief invoegen van de batches tot één dubbele record is gevonden. Als de streaming-taak heeft een aanzienlijk aantal dubbele rijen, deze splitsen en invoegen proces heeft voor het negeren van de duplicaten één voor één, die minder efficiënt en tijdrovend is. Als u meerdere sleutelconflict waarschuwingsberichten in het logboek activiteit binnen het afgelopen uur ziet, is het waarschijnlijk dat de SQL-uitvoer is vertraging in de gehele taak. 

U lost dit probleem, moet u [configureren van de index]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) inbreuk op de sleutel veroorzaakt doordat de optie IGNORE_DUP_KEY. Inschakelen van deze optie kunt u dubbele waarden moeten worden genegeerd door SQL tijdens bulksgewijs invoegen en SQL Azure gewoon genereert een waarschuwing weergegeven in plaats van een fout opgetreden. Azure Stream Analytics levert geen primaire sleutel is geschonden fouten meer.

Houd rekening met de volgende opmerkingen bij het configureren van IGNORE_DUP_KEY voor verschillende soorten indexen:

* U kunt geen IGNORE_DUP_KEY instellen op een primaire sleutel of unieke beperking die gebruikmaakt van ALTER INDEX, moet u om te verwijderen en opnieuw maken van de index.  
* U kunt de optie IGNORE_DUP_KEY voor een unieke index, die verschilt van de primaire sleutel/UNIQUE-beperking en gemaakt met de definitie van CREATE INDEX of INDEX met behulp van ALTER INDEX instellen.  
* Ignore_dup_key geldt niet voor columnstore-indexen omdat uniekheid op dergelijke indexen kunnen niet worden afgedwongen.  

## <a name="next-steps"></a>Volgende stappen
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
