---
title: Onderbreken, hervatten, schalen met rest-opslag in Azure SQL Data Warehouse | Microsoft Docs
description: Compute-kracht in SQL Data Warehouse via REST API's beheren.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/29/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 658b35163e20d024118bc7a3142c86614540f00c
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804697"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>REST-API's voor Azure SQL datawarehouse
REST-API's voor het beheren van compute in Azure SQL Data Warehouse.

## <a name="scale-compute"></a>De schaal van Compute aanpassen
Als u wilt wijzigen van de datawarehouse-eenheden, gebruikt u de [maken of bijwerken Database](/rest/api/sql/databases/createorupdate) REST-API. Het volgende voorbeeld wordt de datawarehouse-eenheden naar DW1000 voor de database MySQLDW, die wordt gehost op de server MijnServer. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

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

Als u wilt onderbreken van een database, gebruikt u de [Database onderbreken](/rest/api/sql/databases/pause) REST-API. Het volgende voorbeeld wordt een database met de naam die wordt gehost op een server met de naam Server01 Database02 onderbroken. De server zich in een Azure-resourcegroep met de naam ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>De rekencapaciteit hervatten

Voor het starten van een database, gebruikt u de [hervatten Database](/rest/api/sql/databases/resume) REST-API. Het volgende voorbeeld wordt een database met de naam die wordt gehost op een server met de naam Server01 Database02 gestart. De server zich in een Azure-resourcegroep met de naam ResourceGroup1. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Controleer de databasestatus van de

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Onderhoudsplanning ophalen
Controleer de onderhoudsplanning die is ingesteld voor een datawarehouse. 

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Onderhoudsplanning instellen
Instellen en bijwerken van een planning maintnenance op een bestaande datawarehouse.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": Saturday,
                                "startTime": 00:00,
                                "duration": 08:00,
                },
                {
                                "dayOfWeek": Wednesday
                                "startTime": 00:00,
                                "duration": 08:00,
                }
                ]
    }
}

```


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie, [compute beheren](sql-data-warehouse-manage-compute-overview.md).

