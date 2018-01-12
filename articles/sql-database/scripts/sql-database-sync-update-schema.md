---
title: Voorbeeld van een PowerShell - Update SQL-gegevenssynchronisatie synchronisatieschema | Microsoft Docs
description: Azure PowerShell-voorbeeldscript om bij te werken van het synchronisatieschema voor synchroniseren van de SQL-gegevens
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 66bf084f585b86979e6521321daf466c571de10c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande groep voor synchronisatie

In dit voorbeeld PowerShell werkt het synchronisatieschema in een bestaande groep voor synchronisatie. Wanneer u meerdere tabellen zijn gesynchroniseerd, wordt dit script kunt u het synchronisatieschema efficiënt bijwerken.

In dit voorbeeld wordt het gebruik van de **UpdateSyncSchema** script, dat beschikbaar op GitHub als is [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

Zie voor een overzicht van de SQL-gegevenssynchronisatie [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Vereisten

Dit voorbeeld is de Azure PowerShell-moduleversie 4.2 of hoger vereist. Voer `Get-Module -ListAvailable AzureRM` de geïnstalleerde versie vinden. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Voer `Login-AzureRmAccount` geen verbinding maken met Azure.

## <a name="examples"></a>Voorbeelden

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Voorbeeld 1: alle tabellen toevoegen aan het synchronisatieschema

Het volgende voorbeeld wordt het databaseschema vernieuwd en voegt alle geldige tabellen in de database van de hub aan het synchronisatieschema.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Voorbeeld 2: toevoegen en verwijderen van tabellen en kolommen

Het volgende voorbeeld wordt `[dbo].[Table1]` en `[dbo].[Table2].[Column1]` naar het synchronisatieschema en verwijdert `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Scriptparameters

De **UpdateSyncSchema** script heeft de volgende parameters:

| Parameter | Opmerkingen |
|---|---|
| $SubscriptionId | Het abonnement waar de groep voor synchronisatie wordt gemaakt. |
| $ResourceGroupName | De resourcegroep waar de groep voor synchronisatie wordt gemaakt.|
| $ServerName | De servernaam van de database van de hub.|
| $DatabaseName | De naam van de hub-database. |
| $SyncGroupName | De naam van de synchronisatie. |
| $MemberName | Geef de naam van lid als u wilt het databaseschema laden van het lid van de synchronisatie in plaats van uit de database van de hub. Als u laden van schema van de database van de hub wilt, laat u deze parameter leeg. |
| $TimeoutInSeconds | Time-out tijdens het databaseschema door het script wordt vernieuwd. Standaard is 900 seconden. |
| $RefreshDatabaseSchema | Geef op of het script moet worden vernieuwd van schema van de database. Als uw databaseschema is gewijzigd van de vorige configuratie - bijvoorbeeld, als u een nieuwe tabel toegevoegd of opnieuw kolom), moet u het schema vernieuwen voordat u het opnieuw configureren. Standaard is ingesteld op false. |
| $AddAllTables | Als deze waarde true is, worden alle geldige tabellen en kolommen toegevoegd aan het synchronisatieschema. De waarden van $TablesAndColumnsToAdd en $TablesAndColumnsToRemove worden genegeerd. |
| $TablesAndColumnsToAdd | Tabellen of kolommen worden toegevoegd aan het synchronisatieschema opgeven. De naam van elke tabel of kolom moet volledig worden gescheiden met de naam van het schema. Bijvoorbeeld: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Meerdere namen voor tabel of kolom worden opgegeven en gescheiden door een komma (,). |
| $TablesAndColumnsToRemove | Tabellen of kolommen worden verwijderd uit het synchronisatieschema opgeven. De naam van elke tabel of kolom moet volledig worden gescheiden met de naam van schema. Bijvoorbeeld: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Meerdere namen voor tabel of kolom worden opgegeven en gescheiden door een komma (,). |
|||

## <a name="script-explanation"></a>Script uitleg

De **UpdateSyncSchema** script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Retourneert informatie over een groep voor synchronisatie. |
| [Update AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Een groep voor synchronisatie-updates. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Retourneert informatie over een lid van de synchronisatie. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Retourneert informatie over een synchronisatieschema. |
| [Update AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Een synchronisatieschema-updates. |
|||

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure PowerShell [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van aanvullende SQL Database PowerShell-script kunnen worden gevonden in [Azure SQL Database PowerShell-scripts](../sql-database-powershell-samples.md).

Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met synchroniseren van Azure SQL-gegevens](../sql-database-sync-data.md)
-   [Synchroniseren van Azure SQL-gegevens instellen](../sql-database-get-started-sql-data-sync.md)
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL-gegevens](../sql-database-best-practices-data-sync.md)
-   [Monitor Azure SQL-gegevens synchroniseren met OMS Log Analytics](../sql-database-sync-monitor-oms.md)
-   [Problemen oplossen met het synchroniseren van Azure SQL-gegevens](../sql-database-troubleshoot-data-sync.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](../sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)
