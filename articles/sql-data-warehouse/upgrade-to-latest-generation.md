---
title: Een upgrade uitvoeren naar de nieuwste generatie van Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 11/26/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 1e2f1a3c46c9d343c305292a217fff5750f442fa
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682551"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Prestaties optimaliseren door bijwerken van SQL Data Warehouse
Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.

## <a name="why-upgrade"></a>Waarom een upgrade uitvoeren?
U kunt nu naadloos upgraden naar de SQL Data Warehouse Compute geoptimaliseerd Gen2 laag in Azure portal. Als u een datawarehouse voor geoptimaliseerd Gen1 Compute-laag hebt, wordt een upgrade aanbevolen. Door een upgrade uitvoert, u kunt de nieuwste generatie van Azure-hardware en verbeterde opslagarchitectuur. U kunt profiteren van betere prestaties, hogere schaalbaarheid en onbeperkte opslag in kolommen. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Van toepassing op
Deze upgrade is van toepassing op datawarehouses geoptimaliseerd Gen1 Compute-laag.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Voordat u begint
> [!NOTE]
> Als uw bestaande datawarehouse voor geoptimaliseerd Gen1 Compute-laag niet in een regio waar de laag geoptimaliseerd Gen2 Compute beschikbaar is is, kunt u [geo-herstel](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) via PowerShell naar een ondersteunde regio.
>
> 

1. Als het datawarehouse geoptimaliseerd Gen1 Compute-laag moet worden bijgewerkt is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md).

2. Worden voorbereid voor een paar minuten uitvaltijd. 

3. Alle codeverwijzingen naar geoptimaliseerd Gen1 Compute-prestatieniveaus identificeren en ze naar de equivalente geoptimaliseerd Gen2 Compute-prestatieniveau wijzigen. Hieronder vindt u twee voorbeelden van waar u verwijzingen naar code vóór de upgrade bijwerken moet:

   Oorspronkelijke Gen1 PowerShell-opdracht uit:

   ```powershell
   Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   Gewijzigd op:

   ```powershell
   Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
   ```

   > [!NOTE] 
   > -RequestedServiceObjectiveName "DW300" is gewijzigd in - RequestedServiceObjectiveName "DW300**c**"
   >

   Oorspronkelijke Gen1 T-SQL-opdracht:

   ```SQL
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300') ;
   ```

   Gewijzigd op:

   ```sql
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300c') ; 
   ```
    > [!NOTE] 
    > SERVICE_OBJECTIVE = 'DW300' wordt gewijzigd naar SERVICE_OBJECTIVE = ' DW300**c**'



## <a name="start-the-upgrade"></a>Start de upgrade

1. Ga naar uw Compute geoptimaliseerd Gen1 laag datawarehouse in Azure portal en klik op de **upgraden naar Gen2** kaart op het tabblad taken: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
    > [!NOTE]
    > Als u niet ziet de **upgraden naar Gen2** kaart op het tabblad taken, uw abonnementstype is beperkt in de huidige regio.
    > [Verzend een ondersteuningsticket](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) aan uw abonnement in de whitelist opgenomen.

2. Standaard **selecteert u de voorgestelde prestatieniveau** voor het datawarehouse op basis van het huidige prestatieniveau op de laag geoptimaliseerd Gen1 Compute met behulp van de onderstaande toewijzing:

   | Gen1 met geoptimaliseerde rekenlaag | Gen2 met geoptimaliseerde rekenlaag |
   | :-------------------------: | :-------------------------: |
   |            DW100            |           DW100c            |
   |            DW200            |           DW200c            |
   |            DW300            |           DW300c            |
   |            DW400            |           DW400c            |
   |            DW500            |           DW500c            |
   |            DW600            |           DW500c            |
   |           DW1000            |           DW1000c           |
   |           DW1200            |           DW1000c           |
   |           DW1500            |           DW1500c           |
   |           DW2000            |           DW2000c           |
   |           DW3000            |           DW3000c           |
   |           DW6000            |           DW6000c           |

3. Zorg ervoor dat uw workload vóór de upgrade wordt uitgevoerd en stilgelegd is voltooid. U wordt uitvaltijd moeten ervaren over een paar minuten voordat uw datawarehouse weer online is als een datawarehouse geoptimaliseerd Gen2 Compute-laag is. **Klik op upgraden**:

   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **De upgrade van uw bewaken** door het controleren van de status in Azure portal:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)

   De eerste stap van het upgradeproces verloopt via de schaalbewerking ('upgrade - Offline") waarbij alle gebruikerssessies worden verwijderd, en verbindingen wordt verwijderd. 

   De tweede stap van het upgradeproces is gegevensmigratie ('upgrade - Online'). Migratie van gegevens is een achtergrondproces online nieuwe deze langzaam kolomgegevens van de oude opslagarchitectuur verplaatst naar de nieuwe opslagarchitectuur gebruik te maken van een lokale SSD-cache. Gedurende deze tijd is uw datawarehouse online zijn voor het uitvoeren van query's en het laden. Al uw gegevens zijn beschikbaar om op te vragen, ongeacht of deze is gemigreerd of niet. De gegevensmigratie gebeurt tegen een tarief variëren, afhankelijk van de gegevensgrootte van uw, het prestatieniveau en het nummer van de columnstore-segmenten. 

5. **Optionele aanbeveling:** versnellen het achtergrondproces voor migratie van gegevens, kunt u de verplaatsing van gegevens direct forceren door uit te voeren [Alter Index opnieuw opbouwen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) op alle primaire columnstore-tabellen worden query's op een grotere SLO uitvoeren zou en resourceklasse. Deze bewerking is **offline** vergeleken met het nieuwe achtergrondproces dat uur, afhankelijk van het aantal en de grootte van uw tabellen kunt duren; gegevensmigratie zijn echter veel sneller waar u kunt vervolgens profiteren van de nieuwe verbeterde opslagarchitectuur als u klaar bent met hoogwaardige Rijgroepen. 

De volgende query worden de vereiste Alter Index Rebuild-opdrachten voor het versnellen van het migratieproces van gegevens gegenereerd:

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
Uw bijgewerkte datawarehouse is online. Als u wilt profiteren van de verbeterde architectuur, Zie [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
