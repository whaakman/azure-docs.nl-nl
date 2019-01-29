---
title: Azure SQL Data Warehouse back-up en herstellen - momentopnamen, geografisch redundante | Microsoft Docs
description: Meer informatie over hoe back-up en herstel werkt in Azure SQL Data Warehouse. Gebruik datawarehouse back-ups naar uw datawarehouse herstellen naar een herstelpunt in de primaire regio. Geografisch redundante back-ups gebruiken om te herstellen naar een andere geografische regio.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 12764574c92c494e27290f98f274d2b76c7a4dc6
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183222"
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Back-up en herstel in Azure SQL Data Warehouse
Meer informatie over hoe back-up en herstel werkt in Azure SQL Data Warehouse. Gebruik gegevens datawarehouse-momentopnamen voor herstel of uw datawarehouse kopiëren naar een vorige herstelpunt in de primaire regio. Gebruik datawarehouse-geografisch redundante back-ups om terug te zetten naar een andere geografische regio. 

## <a name="what-is-a-data-warehouse-snapshot"></a>Wat is een momentopname van een datawarehouse?
Een *datawarehouse momentopname* een herstelpunt dat u kunt gebruiken om te herstellen of uw datawarehouse naar een eerdere status kopie wordt gemaakt.  Omdat SQL Data Warehouse een gedistribueerd systeem is, bestaat er een momentopname van een datawarehouse met veel bestanden die zich in Azure storage. Momentopnamen vastleggen incrementele wijzigingen van de gegevens die zijn opgeslagen in uw datawarehouse.

Een *datawarehouse herstellen* is een nieuwe datawarehouse is gemaakt van een punt voor het herstellen van een bestaande of een verwijderd datawarehouse. Herstellen van uw datawarehouse is een essentieel onderdeel van een strategie voor zakelijke continuïteit en noodherstel, omdat de gegevens opnieuw worden gemaakt na het per ongeluk beschadigd of verwijderd. Datawarehouse is ook een krachtig mechanisme om kopieën van uw datawarehouse voor test- en testdoeleinden te maken.  SQL Data Warehouse maakt gebruik van snel herstel mechanismen binnen dezelfde regio die aan een grootte van minder dan 20 minuten duren voordat heeft zijn gemeten. 

## <a name="automatic-restore-points"></a>Automatische herstelpunten
Momentopnamen zijn herstelpunten voor een ingebouwde functie van de service die wordt gemaakt. U hoeft niet te deze functie inschakelt. Automatische herstelpunten kunnen niet op dit moment worden verwijderd door gebruikers waar de service gebruikt deze herstelpunten verwijst naar het onderhouden van SLA's voor herstel.

SQL Data Warehouse hanteert momentopnamen van uw datawarehouse gedurende de dag het maken van herstelpunten die beschikbaar voor de zeven dagen zijn. Deze bewaarperiode kan niet worden gewijzigd. SQL Data Warehouse biedt ondersteuning voor een acht uur beoogd herstelpunt (RPO). U kunt uw datawarehouse in de primaire regio herstellen vanuit een van de momentopnamen die in de afgelopen zeven dagen.

Als u wilt zien wanneer de laatste momentopname gestart, moet u deze query uitvoeren op uw online SQL Data Warehouse. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

## <a name="user-defined-restore-points"></a>Door de gebruiker gedefinieerde herstelpunten
Deze functie kunt u handmatig trigger momentopnamen om herstelpunten te maken van uw datawarehouse voor en na grote wijzigingen. Deze functionaliteit zorgt ervoor dat herstelpunten logisch consistent waarmee u aanvullende gegevensbeveiliging in het geval van eventuele onderbrekingen van de werkbelasting of gebruikersfouten voor snel hersteltijd. Herstelpunten die door de gebruiker gedefinieerde zijn beschikbaar voor de zeven dagen en worden automatisch verwijderd uit uw naam. U kunt de bewaarperiode van de gebruiker gedefinieerde herstelpunten niet wijzigen. **herstelpunten van de gebruiker gedefinieerde 42** op elk gewenst moment in de tijd worden gegarandeerd, zodat ze moet [verwijderd](https://go.microsoft.com/fwlink/?linkid=875299) voordat het maken van een ander herstelpunt. U kunt momentopnamen voor het maken van herstelpunten door gebruiker gedefinieerde activeren [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaserestorepoint?view=azurermps-6.2.0#examples) of de Azure-portal.


> [!NOTE]
> Als u meer dan zeven dagen herstelpunten nodig hebt, kunt stemmen voor deze mogelijkheid [hier](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/35114410-user-defined-retention-periods-for-restore-points). U kunt ook een door de gebruiker gedefinieerde herstelpunt maken en uit het zojuist gemaakte herstelpunt te herstellen naar een nieuwe datawarehouse. Nadat u hebt hersteld, moet u het datawarehouse online zijn en voor onbepaalde tijd om op te slaan van de kosten voor rekenuren kunt onderbreken. De onderbroken database leidt tot opslagkosten voor tegen het tarief van Azure Premium Storage. Als u een actieve kopie van de teruggezette datawarehouse nodig hebt, kunt u hervatten die slechts een paar minuten duurt.
>

### <a name="restore-point-retention"></a>Bewaarperiode van het herstelpunt herstellen
Het volgende is, worden de details op terugzetten punt bewaarperioden beschreven:
1. SQL Data Warehouse een herstelpunt wordt verwijderd wanneer deze de bewaarperiode van 7 dagen **en** wanneer er ten minste 42 totale aantal herstelpunten (met inbegrip van de gebruiker gedefinieerde zowel automatische)
2. Momentopnamen worden niet gemaakt als een datawarehouse wordt onderbroken
3. De leeftijd van een herstelpunt wordt gemeten door de absolute kalenderdagen vanaf het moment dat het herstelpunt wordt gemaakt, met inbegrip van wanneer het datawarehouse wordt onderbroken
4. Op elk gewenst moment in-time, een datawarehouse kan worden gegarandeerd kunnen maximaal 42 door de gebruiker gedefinieerde herstelpunten opslaan en 42 Automatische herstelpunten zolang deze herstelpunten de bewaarperiode van 7 dagen niet hebben bereikt
5. Als een momentopname wordt gemaakt, het datawarehouse Klik voor meer dan 7 dagen is onderbroken en hervat, is het mogelijk voor herstelpunt om vast te leggen totdat er 42 totale aantal herstelpunten (met inbegrip van de gebruiker gedefinieerde zowel automatische)

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Momentopname bewaren wanneer een datawarehouse is verwijderd
Wanneer u een datawarehouse neerzet, wordt SQL Data Warehouse maakt u een momentopname van een definitieve en slaat het voor de zeven dagen. U kunt het datawarehouse herstellen naar het laatste herstelpunt is gemaakt op verwijderen. 

> [!IMPORTANT]
> Als u een logische exemplaar van SQL server verwijdert, worden alle databases die deel uitmaken van het exemplaar worden ook verwijderd en kunnen niet worden hersteld. U kunt een server verwijderd niet herstellen.
>

## <a name="geo-backups"></a>Geo-backups
SQL Data Warehouse voert een geo-back-up eenmaal per dag een [gekoppeld Datacenter](../best-practices-availability-paired-regions.md). Het RPO voor geo-herstel is 24 uur. U kunt de geo-back-up terugzetten op een server in een andere regio waar de SQL Data Warehouse wordt ondersteund. Een geo-back-up zorgt ervoor dat u kunt datawarehouse herstellen in het geval u geen toegang de herstelpunten in de primaire regio tot.

Geo-back-ups zijn standaard ingeschakeld. Als uw datawarehouse Gen1 is, kunt u [opt-out](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy) als u wenst. Afnemen geo-back-ups voor Gen2 omdat de beveiliging van gegevens een ingebouwde gegarandeerd is.

> [!NOTE]
> Als u een kortere RPO voor geo-back-ups vereist, stemmen voor deze mogelijkheid [hier](https://feedback.azure.com/forums/307516-sql-data-warehouse). U kunt ook een door de gebruiker gedefinieerde herstelpunt maken en uit het zojuist gemaakte herstelpunt te herstellen naar een nieuwe datawarehouse in een andere regio. Nadat u hebt hersteld, moet u het datawarehouse online zijn en voor onbepaalde tijd om op te slaan van de kosten voor rekenuren kunt onderbreken. De onderbroken database leidt tot opslagkosten voor tegen het tarief van Azure Premium Storage. en vervolgens onderbreekt.<!-- should this be removed or is something missing? --> Als u moet een actieve kopie van het datawarehouse, kunt u hervatten die slechts een paar minuten duurt.
>


## <a name="backup-and-restore-costs"></a>Kosten voor back-up en herstel
U ziet dat de Azure-factuur heeft een regelitem voor opslag en een regelitem voor Disaster Recovery-opslag. De kosten voor opslag zijn de totale kosten voor het opslaan van gegevens in de primaire regio, samen met de incrementele wijzigingen vastgelegd door momentopnamen. Raadpleeg voor een meer gedetailleerde uitleg van hoe momentopnamen worden in rekening gebracht, [inzicht in hoe momentopnamen toerekenen kosten](https://docs.microsoft.com/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges?redirectedfrom=MSDN#snapshot-billing-scenarios). De kosten voor geografisch redundante dekt de kosten voor het opslaan van de geo-back-ups.  

De totale kosten voor uw primaire datawarehouse en gedurende zeven dagen van de momentopname van wijzigingen wordt afgerond op het dichtstbijzijnde aantal TB. Bijvoorbeeld, als uw datawarehouse 1,5 TB is en de momentopnamen worden vastgelegd van 100 GB, u in rekening gebracht voor 2 TB aan gegevens in Azure Premium Storage-tarieven. 

Als u geografisch redundante opslag gebruikt, ontvangt u een afzonderlijke opslag in rekening gebracht. De geografisch redundante opslag wordt in rekening gebracht op het tarief voor standard-geografisch redundante opslag met leestoegang (RA-GRS).

Zie voor meer informatie over prijzen van SQL Data Warehouse, [prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) en [kosten voor uitgaand verkeer](https://azure.microsoft.com/pricing/details/bandwidth/) bij het herstellen van cross-regio.

## <a name="restoring-from-restore-points"></a>Herstellen van herstelpunten
Elke momentopname wordt gemaakt van een herstelpunt dat staat voor de begintijd van de momentopname. Als u een datawarehouse herstellen, kies een herstelpunt en terugzetten opdracht geven.  

U kunt de teruggezette datawarehouse en de huidige houden of verwijder een van deze. Als u vervangen door het huidige datawarehouse met de herstelde datawarehouse wilt, kunt u wijzigen met behulp van [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) met de naam wijzigen-optie. 

Als u een datawarehouse herstellen, Zie [herstellen van een datawarehouse met behulp van de Azure-portal](sql-data-warehouse-restore-database-portal.md), [herstellen van een datawarehouse met behulp van PowerShell](sql-data-warehouse-restore-database-powershell.md), of [herstellen van een datawarehouse met behulp van REST-API's](sql-data-warehouse-restore-database-rest-api.md).

Voor het herstellen van een datawarehouse verwijderd of is onderbroken, kunt u [een ondersteuningsticket maken](sql-data-warehouse-get-started-create-support-ticket.md). 


## <a name="geo-redundant-restore"></a>Geografisch redundante herstellen
U kunt [uw datawarehouse herstellen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) in elke regio ondersteuning van SQL Data Warehouse op het prestatieniveau van uw gekozen. 

> [!NOTE]
> Als u wilt een geografisch redundante terugzetbewerking moet u niet hebt uitgeschakeld dat deze functie.
>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de planning voor noodherstel [overzicht voor zakelijke continuïteit](../sql-database/sql-database-business-continuity.md)
