---
title: Data Warehouse Units (dwu's, cDWUs) in Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen over het kiezen van het ideale aantal datawarehouse units (dwu's, cDWUs) om te optimaliseren prijs en prestaties en het wijzigen van het aantal eenheden.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 7da813022527830a5ede62c9f4ce98645fe63d4b
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56871590"
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Data Warehouse Units (dwu's) en compute Data Warehouse Units (cDWUs)
Aanbevelingen over het kiezen van het ideale aantal datawarehouse units (dwu's, cDWUs) om te optimaliseren prijs en prestaties en het wijzigen van het aantal eenheden. 

## <a name="what-are-data-warehouse-units"></a>Wat zijn Data Warehouse Units?
Met SQL Data Warehouse CPU, geheugen en i/o-worden gebundeld in eenheden van de schaal van compute Data Warehouse Units (dwu's) genoemd. Een DWU vertegenwoordigt een abstract, genormaliseerde meting van rekenresources en prestaties. U wijzigt het aantal dwu's die zijn toegewezen aan het systeem, dat op zijn beurt past u de prestaties en de kosten van uw systeem door het wijzigen van uw service. 

Om te betalen voor betere prestaties, kunt u het aantal datawarehouse-eenheden te verhogen. Om te betalen voor minder prestaties, verkleinen dwu's. Kosten voor opslag en Computing worden apart in rekening gebracht, zodat de datawarehouse-eenheden wijzigen heeft geen invloed op kosten voor opslag.

Prestaties voor datawarehouse-eenheden is gebaseerd op deze datawarehouse-workload metrieken:

- Hoe snel kunt een query op een standaard magazijnbeheer scannen een groot aantal rijen en vervolgens een complexe aggregatie uitvoeren? Deze bewerking is een i/o en CPU-intensief.
- Hoe snel kan het datawarehouse gegevens uit Azure Storage-Blobs of Azure Data Lake opnemen? Deze bewerking is de netwerk- en CPU-intensief. 
- Hoe snel kunt de [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL-opdracht een tabel kopiëren? Met deze bewerking omvat het lezen van gegevens uit de opslag, verdelen over de knooppunten van het apparaat en wegschrijven naar opslag opnieuw. Deze bewerking is CPU en i/o-intensieve netwerk.

Steeds meer dwu's:
- Prestaties van het systeem voor scans en aggregaties CTAS instructies lineair wordt gewijzigd
- Vergroot het aantal lezers en schrijvers voor PolyBase load bewerkingen
- Verhoogt de maximum aantal gelijktijdige query's en gelijktijdigheidssleuven in beslag.

## <a name="service-level-objective"></a>Serviceniveaudoelstelling
De Service Level Objective (SLO) is de schaalbaarheid-instelling die de kosten en prestaties van uw datawarehouse bepaalt. Het niveau van de service voor Gen2 worden gemeten in compute datawarehouse-eenheden (cDWU), bijvoorbeeld DW2000c. Gen1 serviceniveaus worden gemeten in dwu's, bijvoorbeeld DW2000.
  > [!NOTE]
  > Azure SQL Data Warehouse Gen2 onlangs extra mogelijkheden voor ter ondersteuning van rekenlagen slechts 100 cDWU toegevoegd. Bestaande datawarehouses momenteel op Gen1 waarvoor de laagste compute lagen kunnen nu upgraden naar Gen2 in de regio's die momenteel beschikbaar voor zonder extra kosten zijn.  Als uw regio wordt nog niet ondersteund, kunt u nog steeds bijwerken naar een ondersteunde regio. Zie voor meer informatie [upgraden naar Gen2](upgrade-to-latest-generation.md).

In T-SQL bepaalt de instelling SERVICE_OBJECTIVE het serviceniveau en de prestatielaag voor uw datawarehouse.

```sql
--Gen1
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Gen2
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Prestatielagen en datawarehouse Units

Elke prestatielaag maakt gebruik van een iets andere maateenheid voor de datawarehouse-eenheden. Dit verschil wordt op de factuur weergegeven als de eenheid van de schaal direct wordt omgezet in facturering.

- Gen1 datawarehouses worden gemeten in Data Warehouse Units (dwu's).
- Gen2 data warehouses worden gemeten in compute Data Warehouse Units (cDWUs). 

Dwu's zowel cDWUs ondersteuning voor vergroten/verkleinen compute omhoog of omlaag en onderbreken compute wanneer u niet nodig hebt voor het gebruik van het datawarehouse. Deze bewerkingen zijn alle op aanvraag. Gen2 maakt gebruik van een lokale schijf op basis van cache op de rekenknooppunten om prestaties te verbeteren. Als u het systeem onderbreken of schalen, de cache ongeldig is gemaakt en dus een periode van het opwarmen van cache is vereist voordat de optimale prestaties wordt bereikt.  

Als u de datawarehouse-eenheden verhoogt, zijn u Lineair computerbronnen te vergroten. Gen2 biedt de beste prestaties van query's en de hoogste schaal. Deze systemen maken de meeste gebruik van de cache.

### <a name="capacity-limits"></a>Capaciteitslimieten
Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft een [Database Transaction Unit (DTU)](../sql-database/sql-database-what-is-a-dtu.md) quotum waarmee een bepaald aantal dwu's. Zie voor meer informatie de [Capaciteitslimieten management workload](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Het aantal dwu's heb ik nodig?
Het ideale aantal dwu's afhankelijk is al te veel van de werkbelasting en de hoeveelheid gegevens die u hebt geladen in het systeem.

Stappen voor het vinden van de beste DWU voor uw workload:

1. Beginnen met een kleinere DWU te selecteren. 
2. Prestaties van uw toepassing controleren als u laden van gegevens in het systeem testen, het aantal dwu's geselecteerd ten opzichte van de prestaties die u ziet dat uw probleem.
3. Identificeer eventuele bijkomende vereisten voor periodieke perioden van piekactiviteit. Als de werkbelasting grote pieken en een holten laat van de activiteit zien en er een goede reden is voor het schalen van vaak.

SQL Data Warehouse is een scale-out-systeem die grote hoeveelheden reken- en aanzienlijke hoeveelheden gegevens kunt inrichten. Als u wilt zien van de werkelijke capaciteit voor schalen, met name bij een groter aantal dwu's, wordt u aangeraden de gegevensset te schalen wanneer u schaalt om ervoor te zorgen dat er voldoende gegevens om de CPU's. Voor het testen van schaal, wordt u aangeraden ten minste 1 TB.

> [!NOTE]
>
> Prestaties van query's alleen wordt aangepast met meer parallellisering als het werk kan worden gesplitst tussen rekenknooppunten. Als u vindt dat schalen is niet gewijzigd, de prestaties van uw, moet u mogelijk om af te stemmen uw tabelontwerp en/of uw query's. Zie voor het afstemmen van query's richtlijnen, [query's van gebruikers beheren](sql-data-warehouse-overview-manage-user-queries.md). 

## <a name="permissions"></a>Machtigingen

De machtigingen die worden beschreven in de datawarehouse-eenheden wijzigen vereist [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql). 

Ingebouwde rollen voor Azure-resources, zoals ' SQL DB Contributor ' en Inzender voor SQL Server kunnen DWU-instellingen wijzigen. 

## <a name="view-current-dwu-settings"></a>De huidige DWU-instellingen weergeven

De huidige DWU-instelling weergeven:

1. Open SQL Server-Objectverkenner in Visual Studio.
2. Verbinding maken met de hoofddatabase die zijn gekoppeld aan de logische SQL Database-server.
3. Selecteer in de dynamische Beheerweergave sys.database_service_objectives. Hier volgt een voorbeeld: 

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Dwu's wijzigen

### <a name="azure-portal"></a>Azure Portal
Dwu's of cDWUs wijzigen:

1. Open de [Azure-portal](https://portal.azure.com), opent u uw database en klik op **schaal**.

2. Onder **schaal**, verplaats de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **Nee** om te annuleren.

### <a name="powershell"></a>PowerShell
De dwu's of cDWUs wilt wijzigen, gebruikt u de [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell-cmdlet. Het volgende voorbeeld wordt de serviceniveaudoelstelling naar DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

Zie voor meer informatie, [PowerShell-cmdlets voor SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md)

### <a name="t-sql"></a>T-SQL
Met T-SQL kunt u de huidige DWU of cDWU-instellingen weergeven, de instellingen wijzigen en de voortgang controleren. 

De dwu's of cDWUs wijzigen:

1. Verbinding maken met de hoofddatabase die zijn gekoppeld aan de logische SQL Database-server.
2. Gebruik de [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) TSQL-instructie. Het volgende voorbeeld wordt de serviceniveaudoelstelling naar DW1000 voor de database MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST-API’s

U kunt de dwu's wijzigen met de [maken of bijwerken Database](/rest/api/sql/databases/createorupdate) REST-API. Het volgende voorbeeld wordt de serviceniveaudoelstelling naar DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Zie voor meer voorbeelden van de REST-API, [REST-API's voor SQL Data Warehouse](sql-data-warehouse-manage-compute-rest-api.md).

## <a name="check-status-of-dwu-changes"></a>Controleer de status van DWU-wijzigingen

DWU-wijzigingen kunnen enkele minuten duren. Als u automatisch schalen, kunt u overwegen logica om ervoor te zorgen dat bepaalde bewerkingen zijn voltooid voordat u doorgaat met een andere actie. 

De status van de database via verschillende eindpunten te controleren, kunt u automation correct te implementeren. De portal biedt-mailmelding bij voltooiing van een bewerking en de huidige status van databases, maar is niet toegestaan voor het controleren van programmatische van status. 

U kunt de status van de database voor scale-out-bewerkingen met de Azure-portal niet controleren.

Om te controleren of de status van DWU-wijzigingen:

1. Verbinding maken met de hoofddatabase die zijn gekoppeld aan de logische SQL Database-server.
2. De volgende query uit om te controleren of de status van de database verzenden.


```sql
SELECT    *
FROM      sys.databases
;
```

3. Verzenden van de volgende query uit om te controleren of de status van bewerking

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Deze DMV retourneert informatie over het beheer van verschillende bewerkingen op uw SQL Data Warehouse, zoals de bewerking en de status van de bewerking worden IN_PROGRESS of voltooid.

## <a name="the-scaling-workflow"></a>De werkstroom vergroten/verkleinen

Wanneer u een schaalbewerking hebt gestart, beëindigt het systeem eerst alle geopende sessies, het terugdraaien van een open transacties om te controleren of een consistente status. Voor bewerkingen voor schalen, schalen is alleen sprake wanneer deze transactionele ongedaan maken is voltooid.  

- Voor een bewerking omhoog de bepalingen van het systeem de extra berekenen en vervolgens doelschijf aan de storage-laag. 
- Voor een bewerking omlaag schalen, de overbodige knooppunten los te koppelen van de opslag en de schijf opnieuw koppelen aan de resterende knooppunten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van prestaties, [resourceklassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md) en [limieten voor geheugen en gelijktijdigheid](memory-and-concurrency-limits.md).



