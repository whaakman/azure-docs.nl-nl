---
title: Azure SQL Data Warehouse back-ups - momentopnamen, geografisch redundante | Microsoft Docs
description: Meer informatie over SQL Data Warehouse ingebouwde databaseback-ups waarmee u een Azure SQL Data Warehouse herstellen naar een herstelpunt of een andere geografische regio.
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/23/2017
ms.author: jrj;barbkess
ms.openlocfilehash: e76349ef7a2afa02d4f9e5295f299bb8084d1e08
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="backup-and-restore-in-sql-data-warehouse"></a>Back-up en herstel in SQL Data Warehouse
Dit artikel wordt uitgelegd dat de details van back-ups in SQL Data Warehouse. Back-ups van gegevens datawarehouse gebruiken om een momentopname maken van de primaire regio database te herstellen of herstel van een geo-back-up voor uw regio geo gekoppeld. 

## <a name="what-is-a-data-warehouse-backup"></a>Wat is er een back-up van gegevens datawarehouse?
Een back-up van gegevens datawarehouse is het exemplaar van de database die u gebruiken kunt voor het herstellen van een datawarehouse.  Omdat SQL Data Warehouse een gedistribueerd systeem is, wordt een back-up van gegevens datawarehouse bestaat uit veel bestanden die zich in Azure-opslag bevinden. Een back-up van gegevens datawarehouse bevat momentopnamen van de lokale database en de geo-back-ups van de databases en bestanden die gekoppeld aan een datawarehouse zijn. 

## <a name="local-snapshot-backups"></a>Lokale momentopname back-ups
SQL Data Warehouse duurt momentopnamen van uw datawarehouse gedurende de dag. Momentopnamen zijn beschikbaar voor de zeven dagen. SQL Data Warehouse ondersteunt een acht uur beoogd herstelpunt (RPO). U kunt uw datawarehouse in de primaire regio terugzetten naar een van de momentopnamen die zijn uitgevoerd in de afgelopen zeven dagen.

Als u wilt zien wanneer de laatste momentopname gestart, moet u deze query uitvoeren op uw online SQL Data Warehouse. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

## <a name="geo-backups"></a>Geo-back-ups
SQL Data Warehouse voert een geo-back-up eenmaal per dag een [gekoppeld Datacenter](../best-practices-availability-paired-regions.md). De RPO voor een geo-restore is 24 uur. U kunt de geo-back-up terugzetten naar de server in de regio geo gekoppeld. geo-back-up zorgt ervoor dat u kunt datawarehouse herstellen als u geen toegang de momentopnamen die in uw primaire regio tot.

Geo-back-ups zijn standaard ingeschakeld. Als uw datawarehouse is geoptimaliseerd voor elasticiteit, kunt u [opt-out](https://docs.microsoft.com/powershell/resourcemanager/Azurerm.sql/v2.1.0/Set-AzureRmSqlDatabaseGeoBackupPolicyredirectedfrom=msdn) als u wenst. U geo-back-ups met de geoptimaliseerde kunnen zich niet afmelden voor compute prestatielaag.

## <a name="backup-costs"></a>Back-kosten
U ziet dat de Azure-factuur heeft een regelitem voor Azure Premium-opslag- en een regelitem voor geografisch redundante opslag. De kosten van de Premium-opslag is de totale kosten voor het opslaan van gegevens in de primaire regio, waaronder momentopnamen.  De geografisch redundante kosten bevat informatie over de kosten voor het opslaan van de geo-back-ups.  

De totale kosten voor de primaire datawarehouse en de zeven dagen van Azure Blob-momentopnamen wordt afgerond op het dichtstbijzijnde TB. Als uw datawarehouse 1,5 TB is en de momentopnamen 100 GB gebruiken, kunt u wordt bijvoorbeeld voor gefactureerd voor 2 TB aan gegevens volgens de tarieven voor Azure Premium-opslag. 

> [!NOTE]
> Elke momentopname in eerste instantie leeg is en neemt toe naarmate u wijzigingen in het primaire datawarehouse aanbrengt. Alle momentopnamen toeneemt in omvang als de gegevens datawarehouse worden gewijzigd. Daarom worden de kosten voor opslag voor momentopnamen volgens de wijzigingssnelheid groeien.
> 
> 

Als u geografisch redundante opslag gebruikt, ontvangt u een afzonderlijke opslag kosten. Geografisch redundante opslag wordt gefactureerd op de standaard-geografisch redundante opslag met leestoegang (RA-GRS)-snelheid.

Zie voor meer informatie over prijzen van SQL Data Warehouse [prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Momentopname bewaren bij een datawarehouse is onderbroken
SQL Data Warehouse geen momentopnamen maken en momentopnamen niet verloopt terwijl een datawarehouse is onderbroken. De leeftijd van de momentopname wordt niet gewijzigd terwijl het datawarehouse is onderbroken. Momentopname bewaarperiode is gebaseerd op het aantal dagen dat het datawarehouse niet kalenderdagen online is.

Als een momentopname oktober 1 bij 16: 00 begint en het datawarehouse is onderbroken oktober 3 4 uur, zijn de momentopnamen die maximaal twee dagen. Wanneer het datawarehouse weer online komt is de momentopname twee dagen oud. Als het datawarehouse online wordt gezet oktober 5 om 16: 00, wordt de momentopname is twee dagen oud en blijft gedurende vijf dagen.

Wanneer het datawarehouse weer online wordt gezet, wordt SQL Data Warehouse hervat van nieuwe momentopnamen en momentopnamen verloopt wanneer ze meer dan zeven dagen aan gegevens hebben.

## <a name="can-i-restore-a-dropped-data-warehouse"></a>Kan ik een verwijderde datawarehouse terugzetten?
Wanneer u een datawarehouse verwijderen, wordt in SQL Data Warehouse maakt een momentopname van een definitieve en slaat het voor de zeven dagen. Het datawarehouse kunt u herstellen naar het laatste herstelpunt gemaakt op verwijderen. 

> [!IMPORTANT]
> Als u een logische SQL server-exemplaar verwijdert, worden alle databases die deel uitmaken van het exemplaar worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.
> 

## <a name="next-steps"></a>Volgende stappen
Zie voor het herstellen van een SQL datawarehouse [herstellen van een SQL datawarehouse](sql-data-warehouse-restore-database-overview.md).

Zie voor een overzicht zakelijke continuïteit [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)
