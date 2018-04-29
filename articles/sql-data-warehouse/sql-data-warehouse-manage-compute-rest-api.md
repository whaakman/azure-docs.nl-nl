---
title: Onderbreken, hervatten, schalen met REST in Azure SQL Data Warehouse | Microsoft Docs
description: De rekencapaciteit in SQL Data Warehouse via REST API's beheren.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 21423d69bf2cf06bcd208082ce492bf5dd038e29
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>REST-API's voor Azure SQL datawarehouse
REST-API's voor het beheren van rekenknooppunten in Azure SQL Data Warehouse.

## <a name="scale-compute"></a>De schaal van Compute aanpassen
De datawarehouse als eenheden wilt wijzigen, gebruikt u de [maken of bijwerken Database](/rest/api/sql/databases/createorupdate) REST-API. Het volgende voorbeeld wordt de datawarehouse eenheden ingesteld op DW1000 voor de database MySQLDW die wordt gehost op de server MijnServer. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>De rekencapaciteit onderbreken

Een database onderbreken, gebruikt u de [Database onderbreken](/rest/api/sql/databases/pause) REST-API. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 onderbroken. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Compute hervatten

Voor het starten van een database, gebruikt u de [Database hervatten](/rest/api/sql/databases/resume) REST-API. Het volgende voorbeeld wordt een database met naam Database02 gehost op een server met de naam Server01 gestart. De server zich in een Azure-resourcegroep met de naam ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Controleer de databasestatus van de

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie [compute beheren](sql-data-warehouse-manage-compute-overview.md).

