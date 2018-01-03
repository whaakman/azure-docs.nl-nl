---
title: Onderbreken, hervatten, schalen met REST in Azure SQL Data Warehouse | Microsoft Docs
description: De rekencapaciteit in SQL Data Warehouse via REST, T-SQL en PowerShell beheren.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 21de7337-9356-49bb-a6eb-06c1beeba2c4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 07/25/2017
ms.author: elbutter
ms.openlocfilehash: 24e43205c0c562fca9b1c2c0e5eed4da54e17ed7
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Rekencapaciteit in Azure SQL Data Warehouse (REST) beheren
> [!div class="op_single_selector"]
> * [Overzicht](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>De rekencapaciteit schaal
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

U kunt het aantal dwu's wijzigen met de [maken of de Database bijwerken] [ Create or Update Database] REST-API. Het volgende voorbeeld wordt de serviceniveaudoelstelling op DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>De rekencapaciteit onderbreken
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Een database onderbreken, gebruikt u de [Database onderbreken] [ Pause Database] REST-API. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 onderbroken. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Compute hervatten
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Voor het starten van een database, gebruikt u de [Database hervatten] [ Resume Database] REST-API. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 gestart. De server zich in een Azure-resourcegroep met de naam ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Controleer de databasestatus van de

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Zie voor andere beheertaken [Beheeroverzicht][Management overview].

<!--Image references-->

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Pause Database]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Resume Database]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Create or Update Database]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
