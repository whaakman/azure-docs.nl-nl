---
title: Onderbreken, hervatten, schalen met REST in Azure SQL Data Warehouse | Microsoft Docs
description: De rekencapaciteit in SQL Data Warehouse via REST API's beheren.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: kfile
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 02/13/2018
ms.author: barbkess
ms.openlocfilehash: cb5b6221a5fc1d02ed1d93d56fd3db4858923307
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
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
Zie voor andere beheertaken [overzicht](./sql-data-warehouse-overview-manage.md).

