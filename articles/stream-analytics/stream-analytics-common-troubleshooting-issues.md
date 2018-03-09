---
title: Problemen oplossen voor een verkeerd ingedeelde invoervelden in Azure Stream Analytics | Microsoft Docs
description: Hoe weet ik welke gebeurtenis in mijn invoer gegevens probleem wordt veroorzaakt door in een Stream Analytics-taak
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/05/2018
ms.author: sngun
ms.openlocfilehash: 6b6c154568fe97b7495ae70dc162dc475169afea
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Veelvoorkomende problemen in de Stream Analytics en stappen op te lossen

## <a name="troubleshoot-for-malformed-input-events"></a>Ongeldige invoer gebeurtenissen oplossen

Wanneer de invoerstroom van de Stream Analytics-taak onjuiste berichten bevat, wordt de serialisatie problemen veroorzaakt. Onjuiste berichten bevatten onjuiste serialisatie zoals haakje openen ontbreekt in een JSON-object of een onjuiste stempel tijdnotatie. Wanneer een Stream Analytics-taak heeft een onjuist gevormd bericht ontvangen, komt het bericht en waarschuwt de gebruiker met een waarschuwing. Een waarschuwingssymbool wordt weergegeven op de **invoer** tegel van de Stream Analytics-taak (als deze waarschuwing aanmelding bestaat, zolang de taak wordt uitgevoerd):

![Invoer-tegel](media/stream-analytics-malformed-events/inputs_tile.png)

Hier kunt u de logboeken met diagnostische gegevens om de details van de waarschuwing weer te geven. Malformatted invoer gebeurtenissen, bevatten de logboeken van de uitvoering van een item met het bericht dat lijkt op: ' bericht: kan de invoer gebeurtenis(sen) niet deserialiseren van resource <blob URI> als json) '. 

### <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

1. Navigeer naar de invoer tegel en klik hier voor weergave van waarschuwingen.
2. De details van de invoer-tegel wordt een set van waarschuwingen met informatie over het probleem. Hieronder volgt een voorbeeld van de waarschuwing weergegeven, ziet u de waarschuwing de partitie, Offset en volgnummers wanneer er een ongeldige JSON-gegevens. 

   ![Waarschuwingsbericht weergegeven met verschuiving](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Als u de JSON-gegevens die door een onjuiste indeling heeft, voert u de code CheckMalformedEvents.cs, krijgt u de it van de [GitHub-opslagplaats voor voorbeelden](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Deze code leest de partitie-Id, offset en de gegevens die zich in deze offset worden afgedrukt. 

4. Zodra u de gegevens gelezen, kunt u deze kunt analyseren en verbeter de serialisatie-indeling. 

## <a name="handling-duplicate-records-when-using-azure-sql-database-as-output-for-a-stream-analytics-job"></a>Verwerking van dubbele records bij gebruik van Azure SQL Database als uitvoer voor een Stream Analytics-taak

Als u Azure SQL-database als uitvoer naar een Stream Analytics-taak configureert, voegt het massaal records in de doeltabel. In het algemeen wordt gegarandeerd dat Azure stream analytics [ten minste eenmaal levering]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) naar de uitvoerlocatie een kan nog steeds [exact bereiken-eenmalig worden afgeleverd]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) naar SQL uitvoer wanneer SQL-tabel heeft een unique-beperking gedefinieerd. 

Zodra de unique key-beperkingen zijn ingesteld op de SQL-tabel en er zijn dubbele records worden ingevoegd in SQL-tabel, Azure Stream Analytics Hiermee verwijdert u de dubbele record door te verdelen van de gegevens in batches en recursief de batches tot één invoegen dubbele record is gevonden. Als er aanzienlijk aantal dubbele rijen in de pipeline, is splitsen en het invoegen van gegevens wordt genegeerd duplicaten één voor één recursief een tijdrovend proces zijn. Als u meerdere sleutelconflict waarschuwingsberichten in het logboek activiteit binnen het afgelopen uur ziet, is het waarschijnlijk dat de SQL-uitvoer is vertraging in de gehele taak. U lost dit probleem, moet u [configureren van de index]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) inbreuk op de sleutel veroorzaakt doordat de optie IGNORE_DUP_KEY. Inschakelen van deze optie kunt u dubbele waarden moeten worden genegeerd door SQL tijdens bulksgewijs invoegen en SQL Azure gewoon genereert een waarschuwing weergegeven in plaats van een fout opgetreden. Azure Stream Analytics levert geen primaire sleutel is geschonden fouten meer.

Houd rekening met de volgende opmerkingen bij het configureren van IGNORE_DUP_KEY voor verschillende soorten indexen:

* U kunt geen IGNORE_DUP_KEY instellen op een primaire sleutel of unieke beperking die gebruikmaakt van ALTER INDEX, moet u om te verwijderen en opnieuw maken van de index.  
* U kunt de optie IGNORE_DUP_KEY voor een unieke index, die verschilt van de primaire sleutel/UNIQUE-beperking en gemaakt met de definitie van CREATE INDEX of INDEX met behulp van ALTER INDEX instellen.  
* Ignore_dup_key geldt niet voor columnstore-indexen omdat uniekheid op dergelijke indexen kunnen niet worden afgedwongen.  

## <a name="next-steps"></a>Volgende stappen

* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)

