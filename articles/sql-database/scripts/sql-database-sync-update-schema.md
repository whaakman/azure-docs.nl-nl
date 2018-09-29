---
title: 'PowerShell-voorbeeld: Synchronisatieschema voor SQL Data Sync bijwerken | Microsoft Docs'
description: Azure PowerShell-voorbeeldscript voor het bijwerken van het synchronisatieschema voor SQL Data Sync
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 01/10/2018
ms.openlocfilehash: bd3e3a7383098d74f3753908585d3392fafcadc4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056595"
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande synchronisatiegroep

Met dit PowerShell-voorbeeld werkt u het synchronisatieschema bij in een bestaande synchronisatiegroep van SQL Data Sync. Wanneer u meerdere tabellen synchroniseert, helpt dit script u om het synchronisatieschema efficiënt bij te werken. In dit voorbeeld wordt het gebruik van het script **UpdateSyncSchema** gedemonstreerd, dat beschikbaar is op GitHub als [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u ervoor kiest om PowerShell lokaal te installeren en te gebruiken, moet u moduleversie 5.7.0 of hoger van Azure PowerShell gebruiken voor deze zelfstudie. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.


Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](../sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

## <a name="sample-script"></a>Voorbeeldscript

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Voorbeeld 1: alle tabellen toevoegen aan het synchronisatieschema

In het volgende voorbeeld wordt het databaseschema vernieuwd en worden alle geldige tabellen in de hubdatabase aan het synchronisatieschema toegevoegd.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Voorbeeld 2: tabellen en kolommen toevoegen en verwijderen

In het volgende voorbeeld worden `[dbo].[Table1]` en `[dbo].[Table2].[Column1]` aan het synchronisatieschema toegevoegd en wordt `[dbo].[Table3]` verwijderd.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Scriptparameters

Het script **UpdateSyncSchema** heeft de volgende parameters:

| Parameter | Opmerkingen |
|---|---|
| $SubscriptionId | Het abonnement waarin de synchronisatiegroep wordt gemaakt. |
| $ResourceGroupName | De resourcegroep waarin de synchronisatiegroep wordt gemaakt.|
| $ServerName | De servernaam van de hubdatabase.|
| $DatabaseName | De naam van de hubdatabase. |
| $SyncGroupName | De naam van de synchronisatiegroep. |
| $MemberName | Geef de lidnaam op als u het databaseschema wilt laden uit het synchronisatielid in plaats van uit de hubdatabase. Als u het databaseschema uit de hub wilt laden, laat u deze parameter leeg. |
| $TimeoutInSeconds | Time-out waarna het script het databaseschema vernieuwt. De standaardwaarde is 900 seconden. |
| $RefreshDatabaseSchema | Geef aan of het script het databaseschema moet vernieuwen. Als uw databaseschema na de vorige configuratie is gewijzigd, bijvoorbeeld, als u een nieuwe tabel of kolom hebt toegevoegd, moet u het schema vernieuwen voordat u het opnieuw configureert. De standaardinstelling is onwaar. |
| $AddAllTables | Als deze waarde op waar is ingesteld, worden alle geldige tabellen en kolommen aan het synchronisatieschema toegevoegd. De waarden van $TablesAndColumnsToAdd en $TablesAndColumnsToRemove worden genegeerd. |
| $TablesAndColumnsToAdd | Hier kunt u opgeven welke tabellen en kolommen aan het synchronisatieschema moeten worden toegevoegd. Elke tabel- of kolomnaam moet volledig worden gescheiden met de schemanaam. Bijvoorbeeld: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. U kunt meerdere tabel- of kolomnamen opgeven, gescheiden door een komma (,). |
| $TablesAndColumnsToRemove | Hier kunt u opgeven welke tabellen en kolommen uit het synchronisatieschema moeten worden verwijderd. Elke tabel- of kolomnaam moet volledig worden gescheiden met de schemanaam. Bijvoorbeeld: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. U kunt meerdere tabel- of kolomnamen opgeven, gescheiden door een komma (,). |
|||

## <a name="script-explanation"></a>Uitleg van het script

Het script **UpdateSyncSchema** gebruikt de volgende opdrachten. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Retourneert informatie over een synchronisatiegroep. |
| [Update-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Werkt een synchronisatiegroep bij. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Retourneert informatie over een synchronisatielid. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Retourneert informatie over een synchronisatieschema. |
| [Update-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Werkt een synchronisatieschema bij. |
|||

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie over Azure PowerShell](/powershell/azure/overview) voor meer informatie over Azure PowerShell.

Aanvullende voorbeelden van SQL Database PowerShell-scripts vindt u in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).

Zie de volgende onderwerpen voor meer informatie over SQL Data Sync:

-   [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync (Preview)](../sql-database-sync-data.md)
-   [Azure SQL Data Sync instellen](../sql-database-get-started-sql-data-sync.md)
-   [Aanbevolen procedures voor Azure SQL Data Sync](../sql-database-best-practices-data-sync.md)
-   [Azure SQL Data Sync bewaken met Log Analytics](../sql-database-sync-monitor-oms.md)
-   [Problemen oplossen met Azure SQL Data Sync](../sql-database-troubleshoot-data-sync.md)

-   Voer PowerShell-voorbeelden uit die laten zien hoe u SQL Data Sync configureert:
    -   [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](sql-database-sync-data-between-azure-onprem.md)

Zie de volgende onderwerpen voor meer informatie over SQL Database:

-   [Wat is de Azure SQL Database-service?](../sql-database-technical-overview.md)
-   [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
