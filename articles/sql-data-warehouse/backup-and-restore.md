---
title: Azure SQL Data Warehouse-back-up en herstel - momentopnamen, geografisch redundante | Microsoft Docs
description: Meer informatie over hoe back-up en herstel werkt in Azure SQL Data Warehouse. Back-ups van gegevens datawarehouse gebruiken om te zetten van uw datawarehouse naar een herstelpunt in de primaire regio of geografisch redundante back-ups gebruiken om te zetten naar een ander geografische regio.
services: sql-data-warehouse
documentationcenter: ''
author: barbkess
manager: jhubbard
editor: ''
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 03/22/2018
ms.author: jrj;barbkess
ms.openlocfilehash: e909cb6f31d8bc677d9dfd267dab242eb99f42df
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Back-up en herstel in Azure SQL Data Warehouse
Meer informatie over hoe back-up en herstel werkt in Azure SQL Data Warehouse. Back-ups van gegevens datawarehouse gebruiken om te zetten van uw datawarehouse naar een herstelpunt in de primaire regio of geografisch redundante back-ups gebruiken om te zetten naar een ander geografische regio. 

## <a name="what-is-backup-and-restore"></a>Wat is de back-up en herstellen?
Een *datawarehouse-back-up* het exemplaar van de database die u gebruiken kunt voor het herstellen van een datawarehouse.  Omdat SQL Data Warehouse een gedistribueerd systeem is, wordt een back-up van gegevens datawarehouse bestaat uit veel bestanden die zich in Azure-opslag bevinden. Een back-up van gegevens datawarehouse bevat momentopnamen van de lokale database en de geo-back-ups van de databases en bestanden die gekoppeld aan een datawarehouse zijn. 

Een *datawarehouse terugzetten* is een nieuw datawarehouse is gemaakt van een back-up van een bestaande of verwijderde datawarehouse. De herstelde datawarehouse wordt opnieuw gemaakt voor het datawarehouse van een back-up op een bepaald tijdstip. Herstellen van uw datawarehouse is een essentieel onderdeel van een zakelijke continuïteit en noodherstel herstelstrategie opnieuw wordt uw gegevens gemaakt na het per ongeluk beschadigd of verwijderd.

Zowel lokale als geografische herstelacties uitmaken deel van SQL Data Warehouse disaster recovery mogelijkheden. 

## <a name="local-snapshot-backups"></a>Lokale momentopname back-ups
Lokale momentopname back-ups zijn een ingebouwde functie van de service.  U beschikt niet over deze inschakelen. 

SQL Data Warehouse duurt momentopnamen van uw datawarehouse gedurende de dag. Momentopnamen zijn beschikbaar voor de zeven dagen. SQL Data Warehouse ondersteunt een acht uur beoogd herstelpunt (RPO). U kunt uw datawarehouse in de primaire regio terugzetten naar een van de momentopnamen die zijn uitgevoerd in de afgelopen zeven dagen.

Als u wilt zien wanneer de laatste momentopname gestart, moet u deze query uitvoeren op uw online SQL Data Warehouse. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

### <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Momentopname bewaren bij een datawarehouse is onderbroken
SQL Data Warehouse geen momentopnamen maken en momentopnamen niet verloopt terwijl een datawarehouse is onderbroken. De leeftijd van de momentopname wordt niet gewijzigd terwijl het datawarehouse is onderbroken. Momentopname bewaarperiode is gebaseerd op het aantal dagen dat het datawarehouse niet kalenderdagen online is.

Als een momentopname oktober 1 bij 16: 00 begint en het datawarehouse is onderbroken oktober 3 4 uur, zijn de momentopnamen die maximaal twee dagen. Wanneer het datawarehouse weer online komt is de momentopname twee dagen oud. Als het datawarehouse online wordt gezet oktober 5 om 16: 00, wordt de momentopname is twee dagen oud en blijft gedurende vijf dagen.

Wanneer het datawarehouse weer online wordt gezet, wordt SQL Data Warehouse hervat van nieuwe momentopnamen en momentopnamen verloopt wanneer ze meer dan zeven dagen aan gegevens hebben.

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Momentopname bewaren bij een datawarehouse is verwijderd
Wanneer u een datawarehouse verwijderen, wordt in SQL Data Warehouse maakt een momentopname van een definitieve en slaat het voor de zeven dagen. Het datawarehouse kunt u herstellen naar het laatste herstelpunt gemaakt op verwijderen. 

> [!IMPORTANT]
> Als u een logische SQL server-exemplaar verwijdert, worden alle databases die deel uitmaken van het exemplaar worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.
> 

## <a name="geo-backups"></a>Geo-back-ups
SQL Data Warehouse voert een geo-back-up eenmaal per dag een [gekoppeld Datacenter](../best-practices-availability-paired-regions.md). De RPO voor een geo-restore is 24 uur. U kunt de geo-back-up terugzetten naar de server in de regio geo gekoppeld. Een geo-back-up zorgt ervoor dat u kunt datawarehouse herstellen als u geen toegang de momentopnamen die in uw primaire regio tot.

Geo-back-ups zijn standaard ingeschakeld. Als uw datawarehouse is geoptimaliseerd voor elasticiteit, kunt u [opt-out](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy) als u wenst. U geo-back-ups met de geoptimaliseerde kunnen zich niet afmelden voor compute prestatielaag.

## <a name="backup-costs"></a>Back-kosten
U ziet dat de Azure-factuur heeft een regelitem voor Azure Premium-opslag- en een regelitem voor geografisch redundante opslag. De kosten van de Premium-opslag is de totale kosten voor het opslaan van gegevens in de primaire regio, waaronder momentopnamen.  De geografisch redundante kosten bevat informatie over de kosten voor het opslaan van de geo-back-ups.  

De totale kosten voor de primaire datawarehouse en de zeven dagen van Azure Blob-momentopnamen wordt afgerond op het dichtstbijzijnde TB. Als uw datawarehouse 1,5 TB is en de momentopnamen 100 GB gebruiken, kunt u wordt bijvoorbeeld voor gefactureerd voor 2 TB aan gegevens volgens de tarieven voor Azure Premium-opslag. 

> [!NOTE]
> Elke momentopname in eerste instantie leeg is en neemt toe naarmate u wijzigingen in het primaire datawarehouse aanbrengt. Alle momentopnamen toeneemt in omvang als de gegevens datawarehouse worden gewijzigd. Daarom worden de kosten voor opslag voor momentopnamen volgens de wijzigingssnelheid groeien.
> 
> 

Als u geografisch redundante opslag gebruikt, ontvangt u een afzonderlijke opslag kosten. Geografisch redundante opslag wordt gefactureerd op de standaard-geografisch redundante opslag met leestoegang (RA-GRS)-snelheid.

Zie voor meer informatie over prijzen van SQL Data Warehouse [prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restoring-from-restore-points"></a>Herstellen van herstelpunten
Elke momentopname is een herstelpunt dat geeft de tijd dat de momentopname gestart. Voor het herstellen van een datawarehouse, kies een herstelpunt en voert u een restore-opdracht.  

SQL Data Warehouse herstelt altijd de back-up naar een nieuwe datawarehouse. U kunt het herstelde datawarehouse en de huidige houden of verwijder een van deze. Als u vervangen van het huidige datawarehouse met de herstelde datawarehouse wilt, kunt u wijzigen met behulp van [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) met de optie Naam wijzigen. 

Zie voor het herstellen van een datawarehouse [herstellen van een datawarehouse met de Azure portal](sql-data-warehouse-restore-database-portal.md), [herstellen van een datawarehouse met behulp van PowerShell](sql-data-warehouse-restore-database-powershell.md), of [herstellen van een datawarehouse met TSQL-](sql-data-warehouse-restore-database-rest-api.md) .

Voor het herstellen van een datawarehouse verwijderd of is onderbroken, kunt u [Maak een ondersteuningsticket](sql-data-warehouse-get-started-create-support-ticket.md). 


## <a name="geo-redundant-restore"></a>Geografisch redundante terugzetten
U kunt uw datawarehouse terugzetten naar een willekeurige regio Azure SQL Data Warehouse op het niveau van uw gekozen prestatie ondersteunende. 

> [!NOTE]
> Als u wilt een geografisch redundante herstel uitvoeren moet u niet hebt opgegeven buiten deze functie.
> 
> 

## <a name="restore-timeline"></a>Tijdlijn herstellen
U kunt een database herstellen naar elk beschikbaar herstelpunt binnen de afgelopen zeven dagen. Momentopnamen van elke vier tot acht uur start en zijn beschikbaar voor de zeven dagen. Wanneer een momentopname ouder dan zeven dagen is, het verloopt en het herstelpunt is niet meer beschikbaar. 

Back-ups niet het geval van een onderbroken datawarehouse. Als uw datawarehouse is onderbroken voor meer dan zeven dagen, hebt u geen herstelpunten zijn gemaakt. 

## <a name="restore-costs"></a>Kosten herstellen
De kosten voor opslag voor de herstelde datawarehouse wordt gefactureerd op de snelheid van Azure Premium-opslag. 

Als u een herstelde datawarehouse onderbreekt, wordt u in rekening gebracht voor opslag op de snelheid van Azure Premium-opslag. Het voordeel van onderbreken is dat u niet weet in rekening gebracht voor de computerbronnen.

Zie voor meer informatie over prijzen van SQL Data Warehouse [prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restore-use-cases"></a>Gebruiksvoorbeelden herstellen
De primair gebruik van datawarehouse terugzetten is om gegevens te herstellen na onbedoeld gegevensverlies of beschadigd. U kunt ook gegevens datawarehouse terugzetten gebruiken voor het bewaren van een back-up langer dan zeven dagen. Wanneer de back-up is teruggezet, moet u het datawarehouse online zijn en kunt onderbreken voor onbepaalde tijd compute om kosten te besparen. De onderbroken database leidt ertoe dat de opslagkosten snelheid Azure Premium-opslag. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het plannen van noodherstel [Business continuity-overzicht](../sql-database/sql-database-business-continuity.md)
