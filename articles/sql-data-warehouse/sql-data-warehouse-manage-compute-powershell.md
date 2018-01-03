---
title: Beheren van de rekencapaciteit in Azure SQL Data Warehouse (PowerShell) | Microsoft Docs
description: PowerShell-taken voor het beheren van rekencapaciteit. Het aantal rekenresources door dwu's aan te passen. Of onderbreken en hervatten rekenresources kosten besparen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 6a185d96447c2e1b0b463439dd062081e783da5f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>De rekencapaciteit in Azure SQL Data Warehouse (PowerShell) beheren
> [!div class="op_single_selector"]
> * [Overzicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Voordat u begint
### <a name="install-the-latest-version-of-azure-powershell"></a>Installeer de nieuwste versie van Azure PowerShell
> [!NOTE]
> Als u Azure PowerShell met SQL Data Warehouse, moet u Azure PowerShell-versie 1.0.3 of hoger.  Om te controleren of uw huidige versie, voert u de opdracht **Get-Module - ListAvailable-Name Azure**. U kunt installeren de nieuwste versie van [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Zie voor meer informatie [installeren en configureren van Azure PowerShell][How to install and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Aan de slag met Azure PowerShell-cmdlets
Aan de slag:

1. Open Azure PowerShell.
2. Voer deze opdrachten aanmelden in de Azure Resource Manager en selecteer uw abonnement op de PowerShell-prompt.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>De rekencapaciteit schaal
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

U kunt het aantal dwu's wijzigen met de [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell-cmdlet. Het volgende voorbeeld wordt de serviceniveaudoelstelling op DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken, gebruikt u de [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] cmdlet. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 onderbroken. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

> [!NOTE]
> Let op: als uw server foo.database.windows.net, is "foo" als - servernaam in de PowerShell-cmdlets gebruiken.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Een Variant deze volgende voorbeeld wordt de database opgehaald in het $database-object. Geeft vervolgens het object [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. De resultaten worden opgeslagen in de resultDatabase object. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Compute hervatten
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Voor het starten van een database, gebruikt u de [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] cmdlet. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 gestart. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Een Variant deze volgende voorbeeld wordt de database opgehaald in het $database-object. Geeft vervolgens het object [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] en slaat de resultaten in $resultDatabase. De laatste opdracht toont de resultaten.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Controleer de databasestatus van de

Zoals u in de bovenstaande voorbeelden, gebruiken een [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] cmdlet voor informatie over een database, waardoor de status te controleren, maar ook om te gebruiken als een argument. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Die zal leiden tot iets dat 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Waarin u kunt vervolgens controleren om te zien de *Status* van de database. In dit geval kunt u zien dat deze database online is. 

Wanneer u deze opdracht uitvoert, ontvangt u een waarde voor de Status van een Online, onderbreken, hervat, schaal en onderbroken.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Zie voor andere beheertaken [Beheeroverzicht][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
