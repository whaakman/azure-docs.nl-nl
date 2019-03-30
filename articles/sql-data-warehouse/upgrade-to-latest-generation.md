---
title: Een upgrade uitvoeren naar de nieuwste generatie van Azure SQL Data Warehouse | Microsoft Docs
description: Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/19/2019
ms.author: martinle
ms.reviewer: jrasnick
ms.openlocfilehash: 522f1f2f24e8c8c3f68a42569d4057a7694754d1
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651070"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Prestaties optimaliseren door bijwerken van SQL Data Warehouse

Azure SQL Data Warehouse een upgrade uitvoert naar de nieuwste generatie van Azure hardware- en storage-architectuur.

## <a name="why-upgrade"></a>Waarom een upgrade uitvoeren?

U kunt nu naadloos upgraden naar de SQL Data Warehouse Compute geoptimaliseerd Gen2 laag in de Azure-portal voor [ondersteunde regio's](gen2-migration-schedule.md#automated-schedule-and-region-availability-table). Als uw regio biedt geen ondersteuning voor het zelf een upgrade uitvoert, kunt u een upgrade uitvoert naar een ondersteunde regio of wachten op Automatische upgrade beschikbaar zijn in uw regio. Upgrade nu om te profiteren van de nieuwste generatie van Azure-hardware- en verbeterde opslagarchitectuur zoals snellere prestaties, hogere schaalbaarheid en onbeperkte opslag in kolommen. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Van toepassing op

Deze upgrade is van toepassing op geoptimaliseerd Gen1 Compute-laag voor datawarehouses in [ondersteunde regio's](gen2-migration-schedule.md#automated-schedule-and-region-availability-table).

## <a name="before-you-begin"></a>Voordat u begint

1. Controleer of uw [regio](gen2-migration-schedule.md#automated-schedule-and-region-availability-table) wordt ondersteund voor GEN1 GEN2 migreren. Houd er rekening mee de datums automatische migratie. Als u wilt voorkomen van conflicten met het geautomatiseerde proces, moet u de handmatige migratie vóór de begindatum geautomatiseerd proces plannen.
2. Als u zich in een regio die nog niet wordt ondersteund, blijven om te controleren voor uw regio om te worden toegevoegd of [bijwerken met behulp van restore](#upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal) naar een ondersteunde regio.
3. Als uw regio wordt ondersteund, [bijwerken via de Azure-portal](#upgrade-in-a-supported-region-using-the-azure-portal)
4. **Selecteer het voorgestelde prestatieniveau** voor het datawarehouse op basis van het huidige prestatieniveau op de laag geoptimaliseerd Gen1 Compute met behulp van de onderstaande toewijzing:

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

> [!Note]
> Voorgestelde prestatieniveaus zijn niet een directe conversie. Bijvoorbeeld, beste de continuïteit van DW600 DW500c.

## <a name="upgrade-in-a-supported-region-using-the-azure-portal"></a>Een upgrade uitvoert in een ondersteunde regio met Azure portal

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!NOTE]
> Migratie van Gen1 naar Gen2 via Azure portal is definitief. Er is een proces voor het retourneren van Gen1.  

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Als het datawarehouse geoptimaliseerd Gen1 Compute-laag moet worden bijgewerkt is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md).

   > [!NOTE]
   > Azure SQL Data Warehouse moet worden uitgevoerd als u wilt migreren naar Gen2.

2. Worden voorbereid voor een paar minuten uitvaltijd. 

3. Alle codeverwijzingen naar geoptimaliseerd Gen1 Compute-prestatieniveaus identificeren en ze naar de equivalente geoptimaliseerd Gen2 Compute-prestatieniveau wijzigen. Hieronder vindt u twee voorbeelden van waar u verwijzingen naar code vóór de upgrade bijwerken moet:

   Oorspronkelijke Gen1 PowerShell-opdracht uit:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   Gewijzigd op:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
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

1. Ga naar uw datawarehouse geoptimaliseerd Gen1 Compute-laag in Azure portal. Als het datawarehouse geoptimaliseerd Gen1 Compute-laag moet worden bijgewerkt is onderbroken, [hervatten van het datawarehouse](pause-and-resume-compute-portal.md). 
2. Selecteer **upgraden naar Gen2** kaart op het tabblad taken:  ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
    > [!NOTE]
    > Als u niet ziet de **upgraden naar Gen2** kaart op het tabblad taken, uw abonnementstype is beperkt in de huidige regio.
    > [Verzend een ondersteuningsticket](sql-data-warehouse-get-started-create-support-ticket.md) aan uw abonnement in de whitelist opgenomen.

3. Zorg ervoor dat uw workload vóór de upgrade wordt uitgevoerd en stilgelegd is voltooid. U zult uitvaltijd moeten ervaren over een paar minuten voordat uw datawarehouse weer online is als een datawarehouse geoptimaliseerd Gen2 Compute-laag is. **Selecteer Upgrade**:

   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **De upgrade van uw bewaken** door het controleren van de status in Azure portal:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)

   De eerste stap van het upgradeproces verloopt via de schaalbewerking ('upgrade - Offline") waarbij alle gebruikerssessies worden verwijderd, en verbindingen wordt verwijderd. 

   De tweede stap van het upgradeproces is gegevensmigratie ('upgrade - Online'). Migratie van gegevens is een online nieuwe achtergrondproces. Dit proces verplaatst langzaam kolomgegevens van de oude opslagarchitectuur naar de nieuwe opslagarchitectuur met behulp van een lokale SSD-cache. Gedurende deze tijd is uw datawarehouse online zijn voor het uitvoeren van query's en het laden. Uw gegevens zijn beschikbaar om op te vragen, ongeacht of deze is gemigreerd of niet. De gegevensmigratie gebeurt op basis van verschillende tarieven, afhankelijk van de gegevensgrootte van uw, het prestatieniveau en het nummer van de columnstore-segmenten. 

5. **Optionele aanbeveling:** Zodra de vergroten/verkleinen bewerking voltooid is, kunt u het migratieproces achtergrond versnellen. U kunt afdwingen dat verplaatsing van gegevens door te voeren [Alter Index opnieuw opbouwen](sql-data-warehouse-tables-index.md) op alle primaire columnstore-tabellen zou u query's op een grotere SLO en resource-klasse. Deze bewerking is **offline** vergeleken met de nieuwe achtergrond-proces, dat uur, afhankelijk van het aantal en de grootte van uw tabellen kan duren. Echter, als u klaar bent, gegevensmigratie is veel sneller vanwege de nieuwe, verbeterde opslagarchitectuur met hoogwaardige Rijgroepen.
 
> [!NOTE]
> Het herbouwen van ALTER Index is een offline bewerking en de tabellen zijn niet beschikbaar totdat het herstellen is voltooid.

De volgende query worden de vereiste Alter Index Rebuild-opdrachten voor het versnellen van de gegevensmigratie gegenereerd:

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

## <a name="upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal"></a>Een upgrade uitvoert van een Azure-geografische regio met herstellen via Azure portal

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Een gebruiker gedefinieerde herstelpunt met behulp van de Azure portal maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Navigeer naar de SQL datawarehouse die u wilt maken van een herstelpunt voor.

3. Selecteer aan de bovenkant van de sectie overzicht **+ nieuw herstelpunt**.

    ![Nieuw herstelpunt](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)

4. Geef een naam voor het herstelpunt.

    ![Naam van het herstelpunt](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Herstellen van een actieve of onderbroken database met behulp van de Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Navigeer naar de SQL datawarehouse die u herstellen wilt uit.
3. Selecteer aan de bovenkant van de sectie overzicht **herstellen**.

    ![ Overzicht van Herstellen](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)

4. Selecteer een **automatisch herstelpunten** of **herstelpunten zelfgedefinieerde**.

    ![Automatische herstelpunten](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)

5. Voor door de gebruiker gedefinieerde herstellen punten **Selecteer een herstelpunt** of **maken van een nieuw herstelpunt voor gebruiker gedefinieerde**. Kies dat een server in een Gen2 ondersteunde geografische regio. 

    ![Herstelpunten gebruiker gedefinieerd](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>Herstellen van een Azure-geografische regio met behulp van PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt een database herstelt, gebruikt u de [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.

> [!NOTE]
> U kunt een geo-herstel naar Gen2 uitvoeren! Om dit te doen, Geef een ServiceObjectiveName Gen2 (bijvoorbeeld DW1000**c**) als een optionele parameter.

1. Open Windows PowerShell.
2. Verbinding maken met uw Azure-account en alle abonnementen die zijn gekoppeld aan uw account vermelden.
3. Selecteer het abonnement met de database te herstellen.
4. Ophalen van de database die u wilt herstellen.
5. Maak de aanvraag voor het herstel voor de database, een ServiceObjectiveName Gen2 op te geven.
6. Controleer of de status van de database geo hersteld.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>" -RequestedServiceObjectiveName "DW300c"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Zie configureren van uw database nadat het herstel is voltooid, [configureren van uw database na het herstel](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).

De herstelde database worden TDE is ingeschakeld als de brondatabase TDE is ingeschakeld is.


Als u problemen met uw datawarehouse ondervindt, maakt u een [ondersteuningsaanvraag](sql-data-warehouse-get-started-create-support-ticket.md) en verwijzen naar "Gen2 upgrade' als mogelijke oorzaak.

## <a name="next-steps"></a>Volgende stappen

Uw bijgewerkte datawarehouse is online. Als u wilt profiteren van de verbeterde architectuur, Zie [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md).
