---
title: Upgrade naar de nieuwste van Azure SQL Data Warehouse | Microsoft Docs
description: Stappen om Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 6ea45398b0bf7fca43c75797313b7e683972b1ab
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Prestaties optimaliseren door het upgraden van SQL Data Warehouse

U kunt nu probleemloos upgraden naar de optimaliseren voor de prestatielaag Compute in de Azure portal. Als u een geoptimaliseerd voor elasticiteit datawarehouse hebt, kunt dat u een upgrade uitvoeren voor de nieuwste Azure hardware en een verbeterde storage-architectuur. U kunt zich profiteren van betere prestaties, hogere schaalbaarheid en onbeperkte kolommen opslag. 

## <a name="applies-to"></a>Van toepassing op
Deze upgrade geldt voor datawarehouses in de optimaliseren voor de prestatielaag elasticiteit.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint

> [!NOTE]
> Vanaf 3/30, hebt u [server controle](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing#subheading-8) uitgeschakeld voordat u de upgrade start.
> 
>

> [!NOTE]
> Als uw bestaande geoptimaliseerd voor elasticiteit datawarehouse niet is in een regio waar geoptimaliseerd voor Compute beschikbaar is, kunt u [geo-herstel te optimaliseren voor Compute](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) via PowerShell naar een ondersteunde regio.
> 
>

1. Als de geoptimaliseerde voor elasticiteit datawarehouse moet worden bijgewerkt, is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md).
2. Worden voorbereid voor een aantal minuten uitvaltijd. 



## <a name="start-the-upgrade"></a>De upgrade start

1. Ga naar uw optimaliseren voor elasticiteit datawarehouse in de Azure portal op en klik op **Upgrade naar geoptimaliseerd voor Compute**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. Standaard **selecteert u de voorgestelde prestatieniveau** voor het datawarehouse op basis van uw huidige prestatieniveau van optimaliseren voor elasticiteit met behulp van de onderstaande toewijzing:
    
| Geoptimaliseerd voor flexibiliteit | Geoptimaliseerd voor rekenen |
| :----------------------: | :-------------------: |
|      DW100 – DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. Zorg ervoor dat uw werkbelasting vóór de upgrade worden uitgevoerd, en stilgelegd is voltooid. Treedt uitvaltijd voor enkele minuten duren voordat uw datawarehouse weer online als een geoptimaliseerd voor Compute-datawarehouse is. **Klik op upgraden**. De prijs van de optimaliseren voor de prestatielaag Compute is momenteel half-uitgeschakeld tijdens de preview-periode:
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Controleren van de upgrade** door het controleren van de status in de Azure-portal:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   De eerste stap van het upgradeproces, doorloopt de schaalaanpassing ('upgrade - Offline') waarbij alle gebruikerssessies worden verwijderd en worden deze verbindingen verwijderd. 
   
   De tweede stap van het upgradeproces is de gegevensmigratie ('upgrade - Online'). Gegevensmigratie is een achtergrondproces online nieuwe die kolomgegevens van de oude Gen1 opslagarchitectuur langzaam worden verplaatst naar de nieuwe Gen2 opslagarchitectuur gebruikmaken van de lokale SSD-cache van Gen2. Tijdens deze periode worden uw datawarehouse online zijn voor het doorzoeken en laden. Al uw gegevens zijn beschikbaar om op te vragen ongeacht of deze is gemigreerd of niet. De gegevensmigratie gebeurt met een frequentie variëren afhankelijk van de gegevensgrootte van uw, het prestatieniveau en het nummer van de columnstore-segmenten. 

5. **Optionele aanbeveling:** sneller het migratieproces achtergrond, wordt aanbevolen om af te dwingen onmiddellijk verplaatsing van gegevens door te voeren [Alter Index rebuild](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-index) op alle columnstore-tabellen op een grotere SLO en resource de klasse. Deze bewerking is offline vergeleken met de nieuwe achtergrondproces; de gegevensmigratie wordt wel veel sneller waar u kunt vervolgens profiteren van de opslagarchitectuur Gen2 eenmaal met een hoge kwaliteit rowgroups voltooid. 

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
 
