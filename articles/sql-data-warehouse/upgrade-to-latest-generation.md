---
title: Upgrade naar de nieuwste van Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL Data Warehouse upgraden naar de nieuwste generatie van Azure hardware- en storage-architectuur.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 58d65ef05ed872bb357070de9866253baea5dc70
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777804"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Prestaties optimaliseren door bijwerken van SQL Data Warehouse
Azure SQL Data Warehouse upgraden naar de nieuwste generatie van Azure hardware- en storage-architectuur.

## <a name="why-upgrade"></a>Waarom upgraden?
U kunt nu probleemloos upgraden naar SQL Data Warehouse Gen2 in de Azure portal. Als u een Gen1 datawarehouse hebt, wordt upgraden aanbevolen. Door een upgrade uitvoert, kunt u gebruiken de nieuwste Azure hardware en verbeterde opslagarchitectuur. U kunt profiteren van betere prestaties, hogere schaalbaarheid en onbeperkte kolommen opslag. 

## <a name="applies-to"></a>Van toepassing op
Deze upgrade geldt voor datawarehouses Gen1.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint
> [!NOTE]
> Als uw bestaande Gen1 datawarehouse zich niet in een regio waar Gen2 beschikbaar is, kunt u [geo-herstel naar Gen2](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) via PowerShell naar een ondersteunde regio.
> 
>

1. Als het datawarehouse Gen1 moeten worden bijgewerkt, is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md).
2. Worden voorbereid voor een aantal minuten uitvaltijd. 



## <a name="start-the-upgrade"></a>De upgrade start

1. Ga naar uw Gen1 datawarehouse in de Azure portal op en klik op **upgraden naar Gen2**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. Standaard **selecteert u de voorgestelde prestatieniveau** voor het datawarehouse op basis van uw huidige prestatieniveau van Gen1 met behulp van de onderstaande toewijzing:
    
| Gen1 | Gen2 |
| :----------------------: | :-------------------: |
|      DW100 – DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. Zorg ervoor dat uw werkbelasting vóór de upgrade worden uitgevoerd, en stilgelegd is voltooid. Treedt uitvaltijd voor enkele minuten duren voordat uw datawarehouse weer als een datawarehouse Gen2 online is. **Klik op upgraden**. De prijs van de prestatielaag Gen2 is momenteel half-uitgeschakeld tijdens de preview-periode:
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Controleren van de upgrade** door het controleren van de status in de Azure-portal:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   De eerste stap van het upgradeproces, doorloopt de schaalaanpassing ('upgrade - Offline') waarbij alle gebruikerssessies worden verwijderd en worden deze verbindingen verwijderd. 
   
   De tweede stap van het upgradeproces is de gegevensmigratie ('upgrade - Online'). Gegevensmigratie is een achtergrondproces online nieuwe die kolomgegevens van de oude opslagarchitectuur langzaam worden verplaatst naar de nieuwe opslagarchitectuur gebruik te maken van een lokale SSD-cache. Tijdens deze periode worden uw datawarehouse online zijn voor het doorzoeken en laden. Al uw gegevens zijn beschikbaar om op te vragen ongeacht of deze is gemigreerd of niet. De gegevensmigratie gebeurt met een frequentie variëren afhankelijk van de gegevensgrootte van uw, het prestatieniveau en het nummer van de columnstore-segmenten. 

5. **Zoeken naar uw datawarehouse Gen2** met behulp van de blade SQL database bladeren. 

> [!NOTE]
> Er is momenteel een probleem waarbij Gen2 gegevens worden in datawarehouses niet in de SQL datawarehouse weergegeven worden blade bladeren. Gebruik de blade SQL database bladeren om uw bijgewerkte Gen2 datawarehouse vinden. Er wordt gewerkt aan deze oplossing.
> 

6. **Optionele aanbeveling:** sneller het migratieproces achtergrond, wordt aanbevolen om af te dwingen onmiddellijk verplaatsing van gegevens door te voeren [Alter Index rebuild](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) op alle columnstore-tabellen op een grotere SLO en resource de klasse. Deze bewerking is offline vergeleken met de nieuwe achtergrondproces; de gegevensmigratie wordt wel veel sneller waar u kunt vervolgens profiteren van de nieuwe uitgebreide opslagarchitectuur met een hoge kwaliteit rowgroups eenmaal voltooid. 

Deze volgende query worden de vereiste Alter Index Rebuild-opdrachten voor het migratieproces snellere gegenereerd:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Volgende stappen
Uw bijgewerkte datawarehouse is online. Als u wilt profiteren van de verbeterde architectuur, Zie [Resource klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
 
