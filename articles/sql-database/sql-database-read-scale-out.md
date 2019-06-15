---
title: Azure SQL Database - query's op de replica's lezen | Microsoft Docs
description: De Azure SQL Database biedt de mogelijkheid voor het verdelen van alleen-lezen-werkbelastingen met behulp van de capaciteit van alleen-lezen replica's - Read Scale-Out genoemd.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 06/03/2019
ms.openlocfilehash: 1b452fb0bac91429793f8d55e439c36c70784722
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66492725"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Alleen-lezen-replica's te verdelen alleen-lezen query workloads

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als onderdeel van de [architectuur voor hoge beschikbaarheid](./sql-database-high-availability.md#premium-and-business-critical-service-tier-availability), elke database in de servicelaag Premium, bedrijfskritiek of zeer grootschalige automatisch wordt geleverd met een primaire replica en meerdere secundaire replica's. De secundaire replica's worden ingericht met de dezelfde compute groot is als de primaire replica. De **Read Scale-Out** functie kunt u taakverdeling op SQL-Database alleen-lezen-werkbelastingen met behulp van de capaciteit van een van de alleen-lezen replica's in plaats van het delen van de replica voor lezen / schrijven. Op deze manier de alleen-lezen-werkbelasting worden geïsoleerd van de belangrijkste workload voor lezen / schrijven en heeft geen invloed op de prestaties. De functie is bedoeld voor de toepassingen die logisch gescheiden alleen-lezen-werkbelastingen, zoals analytics bevatten. Ze kunnen krijgen prestatievoordelen met behulp van deze extra capaciteit zonder extra kosten.

Het volgende diagram illustreert het gebruik van een bedrijfskritieke database.

![Alleen-lezen replica 's](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

De functie Read Scale-Out is ingeschakeld standaard op de nieuwe Premium, bedrijfskritiek en grootschalige databases. Als uw SQL-verbindingsreeks is geconfigureerd met `ApplicationIntent=ReadOnly`, wordt de toepassing omgeleid door de gateway naar een alleen-lezen replica van die database. Voor meer informatie over het gebruik van de `ApplicationIntent` eigenschap, Zie [Toepassingsintentie op te geven](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Als u wilt om ervoor te zorgen dat de toepassing verbinding met de primaire replica ongeacht maakt de `ApplicationIntent` instellen in de SQL-verbindingsreeks, moet u expliciet uitschakelen lezen scale-out bij het maken van de database of wanneer de configuratie ervan te wijzigen. Als u de database van de laag Standard- of algemeen gebruik naar de laag Premium, bedrijfskritiek of zeer grootschalige bijwerkt en zorg ervoor dat alle verbindingen verder te gaan naar de primaire replica wilt, bijvoorbeeld lezen Scale-out uitschakelen. Zie voor meer informatie over het uitschakelen van deze [inschakelen en uitschakelen van Read Scale-Out](#enable-and-disable-read-scale-out).

> [!NOTE]
> Query Data Store, Extended Events SQL Profiler en controlebeleid functies worden niet ondersteund op de alleen-lezen replica's. 

## <a name="data-consistency"></a>Gegevensconsistentie

Een van de voordelen van replica's is dat de replica's altijd de transactioneel consistente status hebben, maar op verschillende momenten in de tijd kunnen er enkele kleine latentie tussen verschillende replica's. Read Scale-Out biedt ondersteuning voor consistentie van de sessie-niveau. Het betekent als de alleen-lezen-sessie opnieuw verbinding maakt nadat een verbindingsfout veroorzaakt door replica niet beschikbaar zijn, kan deze worden omgeleid naar een replica die is niet 100% up-to-date met de replica voor lezen / schrijven. Op dezelfde manier als een toepassing schrijft gegevens met behulp van een sessie voor lezen / schrijven en leest direct met behulp van een alleen-lezen-sessie, is het mogelijk dat de meest recente updates niet meteen zichtbaar op de replica zijn. De latentie wordt veroorzaakt door een asynchrone transaction log opnieuw wordt uitgevoerd.

> [!NOTE]
> Replicatielatentie binnen de regio laag zijn en deze situatie is zeldzaam.

## <a name="connect-to-a-read-only-replica"></a>Verbinding maken met een alleen-lezen replica

Wanneer u Read Scale-Out voor een database inschakelen, de `ApplicationIntent` optie in de verbindingsreeks die is opgegeven door de client bepaalt of de verbinding wordt doorgestuurd naar de replica schrijven of naar een alleen-lezen-replica. Met name als de `ApplicationIntent` waarde `ReadWrite` (de standaardwaarde), de verbinding worden omgeleid naar de databasereplica voor lezen / schrijven. Dit is gelijk aan het bestaande gedrag. Als de `ApplicationIntent` waarde `ReadOnly`, de verbinding wordt doorgestuurd naar een alleen-lezen-replica.

De volgende verbindingsreeks maakt de client bijvoorbeeld verbinding met een alleen-lezen replica (de items in de punthaken vervangen door de juiste waarden voor uw omgeving en de punthaken neerzetten):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Een van de volgende tekenreeksen voor databaseverbindingen maakt de client verbinding met een alleen-lezen-replica (de items in de punthaken vervangen door de juiste waarden voor uw omgeving en neer te zetten de punthaken):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Controleren of er een verbinding met een alleen-lezen replica is

U kunt controleren of u met een alleen-lezen-replica verbonden bent met de volgende query uit te voeren. Alleen-lezen wanneer verbonden met een alleen-lezen replica wordt geretourneerd.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> Op een bepaald moment is slechts één van de AlwaysON-replica's toegankelijk door de alleen-lezen-sessies.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>Bewaking en probleemoplossing van alleen-lezen replica

Wanneer verbonden met een alleen-lezen replica, kunt u toegang de prestaties van metrische gegevens via de `sys.dm_db_resource_stats` DMV. Gebruiken voor toegang tot query plan statistieken, de `sys.dm_exec_query_stats`, `sys.dm_exec_query_plan` en `sys.dm_exec_sql_text` DMV's.

> [!NOTE]
> De DMV `sys.resource_stats` in de logische hoofddatabase CPU-gebruik en de opslag gegevens van de primaire replica worden geretourneerd.


## <a name="enable-and-disable-read-scale-out"></a>In- en uitschakelen van Read Scale-Out

Read Scale-Out is standaard ingeschakeld op Premium, bedrijfskritiek en grootschalige service-lagen. Read Scale-Out kan niet worden ingeschakeld in de Servicelagen Basic, Standard- of algemeen gebruik. Read Scale-Out is automatisch uitgeschakeld op Hyperschaal-databases die zijn geconfigureerd met 0 replica's. 

U kunt uitschakelen en weer inschakelen Read Scale-Out voor individuele databases en elastische pool-databases in Premium en bedrijfskritiek servicelaag met behulp van de volgende methoden.

> [!NOTE]
> De mogelijkheid om uit te schakelen Read Scale-Out wordt aangeboden voor achterwaartse compatibiliteit.

### <a name="azure-portal"></a>Azure Portal

U kunt de instelling lezen Sacle-out beheren op de **configureren** databaseblade. 

### <a name="powershell"></a>PowerShell

De December 2016 beheren Read Scale-Out in Azure PowerShell vereist Azure PowerShell versie of hoger. Zie voor de nieuwste versie van PowerShell, [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

U kunt uitschakelen of opnieuw lezen Scale-Out in Azure PowerShell in te schakelen door het aanroepen van de [Set AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet en aan te in de gewenste waarde – `Enabled` of `Disabled` --voor de `-ReadScale` parameter. 

Om uit te schakelen lezen scale-out op een bestaande database (de items in de punthaken vervangen door de juiste waarden voor uw omgeving en de punthaken neerzetten):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```
Om uit te schakelen lezen scale-out op een nieuwe database (de items in de punthaken vervangen door de juiste waarden voor uw omgeving en de punthaken neerzetten):

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled -Edition Premium
```

Opnieuw inschakelen lezen scale-out op een bestaande database (de items in de punthaken vervangen door de juiste waarden voor uw omgeving en de punthaken neerzetten):

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

### <a name="rest-api"></a>REST-API

Een database maakt met lezen scale-out is uitgeschakeld of de instelling voor een bestaande database te wijzigen, gebruikt u de volgende methode met de `readScale` eigenschap ingesteld op `Enabled` of `Disabled` zoals in het onderstaande voorbeeld van een aanvraag.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Disabled"
   }
}
```

Zie voor meer informatie, [Databases - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>Met behulp van TempDB op alleen-lezen replica

De TempDB-database niet is gerepliceerd naar de alleen-lezen replica's. Elke replica heeft een eigen versie van TempDB-database die wordt gemaakt wanneer de replica is gemaakt. Dit zorgt ervoor dat TempDB die kan worden bijgewerkt en kan worden gewijzigd tijdens de uitvoering van uw query. Als uw werkbelasting alleen-lezen, is afhankelijk van de TempDB-objecten gebruiken, moet u deze objecten maken als onderdeel van uw query-script. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Gebruik Read Scale-Out met geo-replicatie-databases

Als u Read Scale-Out op verdelen alleen-lezen-workloads op een database die is geo-replicatie (bijvoorbeeld, als een lid van een failovergroep) gebruikt, zorg er dan voor dat die lezen scale-out is ingeschakeld op zowel de primaire en de geo-replicatie secundaire databases. Deze configuratie zorgt ervoor dat dezelfde load balancing-ervaring wordt voortgezet wanneer uw toepassing verbinding met de nieuwe primaire na een failover maakt. Als u verbinding met de secundaire database met geo-replicatie met leesschaal ingeschakeld maakt, de sessies met `ApplicationIntent=ReadOnly` worden doorgestuurd naar een van de replica's dezelfde manier als we verbindingen routeren op de primaire database.  De sessies zonder `ApplicationIntent=ReadOnly` worden doorgestuurd naar de primaire replica van de secundaire geo-replicatie, die ook is alleen-lezen. Omdat secundaire database via geo-replicatie een ander eindpunt heeft als de primaire database is, in het verleden voor toegang tot de secundaire het is niet vereist om in te stellen `ApplicationIntent=ReadOnly`. Om ervoor te zorgen voor neerwaartse compatibiliteit `sys.geo_replication_links` DMV toont `secondary_allow_connections=2` (alle clientverbinding is toegestaan).

> [!NOTE]
> Round robin of een andere load balancing routering tussen de lokale replica's van de secundaire database wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over SQL Database grootschalige aanbieding [grootschalige servicelaag](./sql-database-service-tier-hyperscale.md).
