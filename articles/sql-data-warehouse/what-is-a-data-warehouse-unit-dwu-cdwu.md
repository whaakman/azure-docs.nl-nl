---
title: Data Warehouse Units (dwu's, cDWUs) in Azure SQL Data Warehouse | Microsoft Docs
description: Aanbevelingen over het kiezen van het aantal ideaal datawarehouse Units (dwu's, cDWUs) om te optimaliseren prijs en de prestaties en het wijzigen van het aantal eenheden.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: a83a9f9332d81e02a83efc019ad56027316301ab
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Data Warehouse Units (dwu's) en rekencapaciteit Data Warehouse Units (cDWUs)
Aanbevelingen over het kiezen van het aantal ideaal datawarehouse Units (dwu's, cDWUs) om te optimaliseren prijs en de prestaties en het wijzigen van het aantal eenheden. 

## <a name="what-are-data-warehouse-units"></a>Wat zijn Data Warehouse Units?
Met SQL Data Warehouse CPU, geheugen en I/O zijn gebundeld in eenheden van de compute-schaal Data Warehouse Units (dwu's) genoemd. Een DWU vertegenwoordigt een abstract, genormaliseerde meting van rekenresources en prestaties. U wijzigt het aantal dwu's die zijn toegewezen aan het systeem, dat op zijn beurt Hiermee past u de prestaties en de kosten van uw systeem door het wijzigen van uw serviceniveau. 

Om te betalen voor betere prestaties, kunt u het aantal datawarehouse Units verhogen. Om te betalen voor minder prestaties, datawarehouse units te verminderen. Kosten voor opslag en berekeningen worden afzonderlijk in rekening gebracht, dus de eenheden datawarehouse wijzigen heeft geen invloed op opslagkosten.

Prestaties voor datawarehouse units is gebaseerd op deze datawarehouse-workload metrieken:

- Hoe snel kunt een query op een standaard magazijnbeheer scannen een groot aantal rijen en vervolgens een complexe aggregatie uitvoeren? Deze bewerking is een i/o en CPU-intensief.
- Hoe snel kan het datawarehouse gegevens uit Azure Storage-Blobs of Azure Data Lake opnemen? Deze bewerking is het netwerk en CPU-intensief. 
- Hoe snel kunt de [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL-opdracht kopiëren van een tabel? Deze bewerking omvat het lezen van gegevens uit de opslag, verdelen over de knooppunten van het toestel en schrijven naar opslag opnieuw. Deze bewerking is CPU, IO en het netwerk intensief.

Dwu's verhogen:
- Prestaties van het systeem voor scans, aggregaties en CTAS instructies lineair wordt gewijzigd
- Verhoogt het aantal en schrijfprogramma voor bewerkingen van PolyBase laden
- Verhoogt het maximum aantal gelijktijdige query's en gelijktijdigheid sleuven.

## <a name="service-level-objective"></a>Serviceniveaudoelstelling
De Service Level Objective (SLO) is de instelling voor schaalbaarheid die de kosten en prestaties van uw datawarehouse bepaalt. Het niveau van de service voor het optimaliseren voor Compute prestaties laag schaal worden gemeten in compute datawarehouse-eenheden (cDWU), bijvoorbeeld DW2000c. De geoptimaliseerde voor serviceniveaus elasticiteit worden gemeten in dwu's, bijvoorbeeld dw2000 zijn. 

De instelling SERVICE_OBJECTIVE bepaalt het serviceniveau en de prestatielaag voor uw datawarehouse op T-SQL.

```sql
--Optimized for Elasticity
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Optimized for Compute
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Prestatielagen en datawarehouse Units

Elke prestatielaag maakt gebruik van een iets andere maateenheid voor hun datawarehouse-eenheden. Dit verschil wordt weergegeven op de factuur, omdat de eenheid van de schaal rechtstreeks worden vertaald naar facturering.

- De geoptimaliseerd is voor de prestatielaag elasticiteit wordt gemeten in Data Warehouse Units (dwu's).
- De geoptimaliseerde compute voor compute prestatielaag wordt gemeten in Data Warehouse Units (cDWUs). 

Zowel dwu's en cDWUs ondersteuning voor vergroten/verkleinen compute omhoog of omlaag en onderbreken berekenen wanneer u hoeft niet te gebruiken van het datawarehouse. Deze bewerkingen zijn alle op aanvraag. De geoptimaliseerde voor compute prestatielaag gebruikt ook een lokale cache op basis van schijven op de rekenknooppunten om prestaties te verbeteren. Als u het systeem onderbreken of schalen, de cache is ongeldig en dus een periode van opwarmen van cache is vereist voordat de optimale prestaties wordt bereikt.  

Als u datawarehouse units verhoogt, kunt u computerbronnen zijn lineair oplopende. De geoptimaliseerde voor compute prestatielaag biedt de beste queryprestaties en hoogste schaal heeft maar een hogere vermelding prijs. Het is ontworpen voor bedrijven die u een constante vraag voor prestaties hebt. Deze systemen maken de meeste gebruik van de cache. 

### <a name="capacity-limits"></a>Capaciteitslimieten
Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft een [Database Transaction Unit (DTU)](../sql-database/sql-database-what-is-a-dtu.md) quotum waarmee een bepaald aantal datawarehouse Units. Zie voor meer informatie de [werkbelasting management Capaciteitslimieten](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Hoeveel gegevens datawarehouse eenheden moet ik gebruiken?
Het aantal ideaal datawarehouse Units afhankelijk van te veel uw werkbelasting en de hoeveelheid gegevens die u hebt geladen in het systeem.

Stappen voor het vinden van de beste DWU voor uw workload:

1. Tijdens de ontwikkeling, te beginnen met het selecteren van een kleinere DWU met behulp van de geoptimaliseerd voor de prestatielaag elasticiteit.  Omdat het probleem in deze fase functionele validatie is, is de optimaliseren voor de prestatielaag elasticiteit een redelijke optie. Een goed uitgangspunt is DW200. 
2. De toepassingsprestaties van uw bewaken als u gegevens geladen in het systeem testen, het aantal dwu's geselecteerd vergeleken met de prestaties die u merkt observeren.
3. Identificeer eventuele bijkomende vereisten voor periodieke perioden piek-activiteit. Als de werkbelasting aanzienlijke pieken en holten in de activiteit weergegeven en er is een goede reden regelmatig schalen en vervolgens de geoptimaliseerde voorkeur voor de prestatielaag elasticiteit.
4. Als u meer dan 1000 DWU nodig, voorkeur voor het optimaliseren voor de prestatielaag Compute omdat Hiermee geeft u de beste prestaties.

SQL Data Warehouse is een scale-out-systeem dat de enorme hoeveelheden berekenings- en aanzienlijke hoeveelheid gegevens kunt inrichten. Als u wilt zien van de werkelijke capaciteit voor het schalen, met name bij een groter aantal dwu's, wordt u aangeraden de gegevensset schalen terwijl u schaalt om ervoor te zorgen dat er voldoende gegevens om de CPU's. Voor het testen van de schaal wordt u aangeraden ten minste 1 TB.

> [!NOTE]
>
> Prestaties van query's worden alleen met meer garandeert verhoogd als het werk kan worden gesplitst tussen rekenknooppunten. Als u merkt dat schalen niet van de prestaties van uw wijzigen is, moet u wellicht uw tabelontwerp en/of uw query's optimaliseren. Zie voor query afstemmen richtlijnen, [gebruiker query's beheren](sql-data-warehouse-overview-manage-user-queries.md). 

## <a name="permissions"></a>Machtigingen

Als u de datawarehouse eenheden vereist de machtigingen beschreven in [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql). 

## <a name="view-current-dwu-settings"></a>De huidige DWU-instellingen weergeven

De huidige DWU-instelling weergeven:

1. Open SQL Server-Objectverkenner in Visual Studio.
2. Verbinding maken met de database master is gekoppeld aan de logische SQL Database-server.
3. Selecteer in de weergave van de dynamische Beheerweergave sys.database_service_objectives. Hier volgt een voorbeeld: 

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Datawarehouse eenheden wijzigen

### <a name="azure-portal"></a>Azure Portal
Dwu's of cDWUs wijzigen:

1. Open de [Azure-portal](https://portal.azure.com), opent u de database en klik op **Scale**.

2. Onder **Scale**, verplaats de schuifregelaar naar links of rechts om de DWU-instelling te wijzigen.

3. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **geen** om te annuleren.

### <a name="powershell"></a>PowerShell
Het aantal dwu's of cDWUs wilt wijzigen, gebruikt u de [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) PowerShell-cmdlet. Het volgende voorbeeld wordt de serviceniveaudoelstelling op DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

Zie voor meer informatie [PowerShell-cmdlets voor SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md)

### <a name="t-sql"></a>T-SQL
Met T-SQL kunt u de huidige DWU of cDWU instellingen weergeven, de instellingen wijzigen en de voortgang controleren. 

Het aantal dwu's of cDWUs wijzigen:

1. Verbinding maken met de database master die zijn gekoppeld aan de logische SQL Database-server.
2. Gebruik de [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) TSQL-instructie. Het volgende voorbeeld wordt de serviceniveaudoelstelling ingesteld op DW1000 voor de database MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST-API’s

U kunt het aantal dwu's wijzigen met de [maken of de Database bijwerken](/rest/api/sql/databases/createorupdate) REST-API. Het volgende voorbeeld wordt de serviceniveaudoelstelling op DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

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

## <a name="check-status-of-dwu-changes"></a>Controleer de status van DWU wijzigingen

DWU wijzigingen kunnen enkele minuten duren. Als u automatisch schalen, kunt u logica om ervoor te zorgen dat bepaalde bewerkingen zijn voltooid voordat u doorgaat met een andere actie. 

De status van de database via verschillende eindpunten te controleren, kunt u automation correct te implementeren. De portal biedt melding na voltooiing van een bewerking en de huidige status van de databases, maar is niet toegestaan voor de programmatische controle van status. 

U kunt de status van de database voor scale-out-bewerkingen met de Azure portal niet controleren.

Controleer de status van DWU wijzigingen:

1. Verbinding maken met de database master die zijn gekoppeld aan de logische SQL Database-server.
2. De volgende query om te controleren van de status van de database verzenden.


```sql
SELECT    *
FROM      sys.databases
;
```

3. Verzenden van de volgende query om te controleren van de status van bewerking

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Deze DMV retourneert informatie over het beheer van verschillende bewerkingen op uw SQL Data Warehouse zoals de werking en de status van de bewerking worden IN_PROGRESS of voltooid.

## <a name="the-scaling-workflow"></a>De werkstroom vergroten/verkleinen

Wanneer u een schaalaanpassing initieert, wordt in het systeem eerst is funest alle geopende sessies, zodat een consistente status open transacties worden teruggedraaid. Voor scale-bewerkingen schalen alleen vindt plaats nadat deze transactionele terugdraaiactie is voltooid.  

- Voor het uitvoeren van een scale-up het systeem voorziet de aanvullende berekenen en vervolgens reattaches aan de storage-laag. 
- Voor een bewerking omlaag schalen de overbodige voor knooppunten uit de opslag en weer koppelt aan de resterende knooppunten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het beheren van prestaties, [Resource klassen voor het beheer van de werkbelasting](resource-classes-for-workload-management.md) en [geheugen en gelijktijdigheid limieten](memory-and-concurrency-limits.md).



