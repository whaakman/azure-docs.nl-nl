---
title: Aanroepen van de opgeslagen procedure uit Azure Data Factory Copy Activity | Microsoft Docs
description: Informatie over het aanroepen van een opgeslagen procedure in Azure SQL Database of SQL Server vanuit een kopieeractiviteit in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 35e9347039a7b9939ab4d2719f9738429dec168c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016063"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Aanroepen van de opgeslagen procedure van kopieeractiviteit in Azure Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van opgeslagen procedure-activiteit in Data Factory](../transform-data-using-stored-procedure.md).


Bij het kopiëren van gegevens in [SQL Server](data-factory-sqlserver-connector.md) of [Azure SQL Database](data-factory-azure-sql-connector.md), kunt u de **SqlSink** in de kopieeractiviteit om aan te roepen een opgeslagen procedure. U kunt gebruik van de opgeslagen procedure voor het uitvoeren van een extra verwerking (samenvoegen kolommen, opzoeken van waarden invoegen in meerdere tabellen, enzovoort) is vereist voor het invoegen van gegevens in de doeltabel. Deze functie maakt gebruik van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx). 

Het volgende voorbeeld ziet u hoe u een opgeslagen procedure in een SQL Server-database van een Data Factory-pijplijn (kopieeractiviteit) aanroepen:  

## <a name="output-dataset-json"></a>JSON-uitvoergegevensset
In de uitvoergegevensset JSON, stelt u de **type** aan: **SqlServerTable**. Stel deze in op **AzureSqlTable** voor gebruik met een Azure SQL database. De waarde voor **tableName** eigenschap moet overeenkomen met de naam van de eerste parameter van de opgeslagen procedure.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>De sectie SqlSink in copy activity in JSON
Definieer de **SqlSink** sectie in het copy activity in JSON als volgt. Voor het aanroepen van een opgeslagen procedure bij het invoegen van gegevens in de sink/doel-database, kunt u waarden opgeven voor zowel **SqlWriterStoredProcedureName** en **SqlWriterTableType** eigenschappen. Zie voor beschrijvingen van deze eigenschappen [SqlSink sectie in het SQL Server-connector-artikel](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Definitie van de opgeslagen procedure 
In de database, definieert u de opgeslagen procedure met dezelfde naam als **SqlWriterStoredProcedureName**. De opgeslagen procedure invoergegevens uit de brongegevensopslag worden verwerkt, en voegt de gegevens in een tabel in de doeldatabase. De naam van de eerste parameter van de opgeslagen procedure moet overeenkomen met de tabelnaam gedefinieerd in de gegevensset JSON (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>De definitie van de tabel-type
In de database, definieert het tabeltype met dezelfde naam als **SqlWriterTableType**. Het schema van het tabeltype moet overeenkomen met het schema van de invoergegevensset.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Volgende stappen
Lees de volgende connector artikelen die voor volledige JSON-voorbeelden: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
