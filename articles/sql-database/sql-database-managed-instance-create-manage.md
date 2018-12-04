---
title: Maken en beheren van Azure SQL Managed Instance | Microsoft Docs
description: Meer informatie over het maken en beheren van Azure SQL Database beheerde instanties.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 3e714df775d316ceaafe1a0ce9b55c4e986804cd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856058"
---
# <a name="create-and-manage-azure-sql-database-managed-instances"></a>Maken en beheren van Azure SQL Database beheerde instanties

U kunt maken en beheren van Azure SQL Database Managed Instances met behulp van de Azure portal, PowerShell, Azure CLI, REST-API en Transact-SQL.

## <a name="azure-portal-create-a-managed-instance"></a>Azure-portal: een beheerd exemplaar maken

Zie voor een snelstart u voor het maken van een Azure SQL Database Managed Instance, [Quickstart: maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-a-managed-instance"></a>PowerShell: Maken en beheren van een beheerd exemplaar

Als u wilt maken en beheren van Azure SQL-server, databases en firewalls met Azure PowerShell, gebruikt u de volgende PowerShell-cmdlets. Als u wilt installeren of upgraden van PowerShell, Zie [Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

> [!TIP]
> Zie voor PowerShell-voorbeeldscripts, [Quick start-script: maken van Azure SQL Managed Instance met behulp van PowerShell-bibliotheek](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Beschrijving |
| --- | --- |
|[Nieuwe AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Hiermee maakt u een beheerd exemplaar voor Azure SQL Database |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Get-AzureRmSqlInstance)|Retourneert informatie over Azure SQL Managed Instance|
|[Set-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlInstance)|Hiermee stelt u eigenschappen voor een Azure SQL Database Managed Instance|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlInstance)|Hiermee verwijdert u een beheerde Database-exemplaar voor Azure SQL|

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: Logische servers en databases beheren

Maken en beheren van Azure SQL-server, databases en firewalls met [Azure CLI](/cli/azure), gebruikt u de volgende [Azure CLI SQL Managed Instance](/cli/azure/sql/mi) opdrachten. Gebruik de [Cloud Shell](/azure/cloud-shell/overview) om de CLI in uw browser uit te voeren of [installeer](/cli/azure/install-azure-cli) de CLI op macOS, Linux of Windows.

> [!TIP]
> Zie voor een Azure CLI-Quick Start [werkt met SQL beheerd exemplaar met behulp van Azure CLI met](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
>

| Cmdlet | Beschrijving |
| --- | --- |
|[AZ sql mi maken](https://docs.microsoft.com/cli/azure/sql/db#az-sql-mi-create) |Hiermee maakt u een beheerd exemplaar|
|[lijst met AZ sql mi](https://docs.microsoft.com/cli/azure/sql/db#az-sql-mi-list)|Een lijst met beschikbare beheerde exemplaren|
|[AZ sql mi weergeven](/cli/azure/sql/db#az-sql-mi-show)|De details ophalen voor een beheerd exemplaar|
|[AZ sql mi-update](/cli/azure/sql/db#az-sql-mi-update)|Updates van een beheerd exemplaar|
|[AZ sql mi verwijderen](/cli/azure/sql/db#az-sql-mi-delete)|Hiermee verwijdert u een beheerd exemplaar|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Logische servers en databases beheren

Als u wilt maken en beheren van Azure SQL Database Managed Instance database nadat het beheerde exemplaar is gemaakt, moet u de volgende T-SQL-opdrachten gebruiken. U kunt deze opdrachten met de Azure-portal, de opdracht [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), of andere programma's die kan verbinding maken met een Azure SQL Database-server en Transact-SQL-opdrachten doorgeven.

> [!TIP]
> Voor Zie snelstartgidsen waarin u moet configureren en verbinding maken met een beheerd exemplaar van SQL Server Management Studio op Microsoft Windows, [Quick Start: Azure-VM configureren om te verbinden met een Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vm.md) en [ Snelstartgids: Een punt-naar-site-verbinding configureren naar een Azure SQL Database Managed Instance van on-premises](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> U kunt maken of verwijderen van een beheerd exemplaar met behulp van Transact-SQL.

| Opdracht | Beschrijving |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Hiermee maakt u een nieuwe database in de Managed Instance. U moet zijn verbonden met de hoofddatabase om een nieuwe database te maken.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Hiermee wijzigt u een beheerd exemplaar voor Azure SQL-database.|

## <a name="rest-api-manage-logical-servers-and-databases"></a>REST-API: Beheer van logische servers en databases

Als u wilt maken en beheren van Azure SQL Database Managed Instance, gebruikt u deze REST-API-aanvragen.

| Opdracht | Beschrijving |
| --- | --- |
|[Beheerde exemplaren - maken of bijwerken](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Hiermee of een beheerd exemplaar bijwerken.|
|[Beheerde exemplaren - verwijderen](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Hiermee verwijdert u een beheerd exemplaar.|
|[Beheerde exemplaren - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Hiermee haalt u een beheerd exemplaar.|
|[Beheerde exemplaren - lijst](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Retourneert een lijst met beheerde instanties in een abonnement.|
|[Beheerde exemplaren - lijst per groep](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Retourneert een lijst met beheerde instanties in een resourcegroep.|
|[Beheerde exemplaren - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Een beheerd exemplaar van updates.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het migreren van een SQL Server-database naar Azure, [migreren naar Azure SQL Database](sql-database-cloud-migrate.md).
- Zie [Functies](sql-database-features.md) voor meer informatie over ondersteunde functies.
