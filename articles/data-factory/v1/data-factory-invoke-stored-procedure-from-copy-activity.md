---
title: Aanroepen van opgeslagen procedure uit Azure Data Factory-Kopieeractiviteit | Microsoft Docs
description: Informatie over het aanroepen van een opgeslagen procedure in Azure SQL Database of SQL Server van een kopieeractiviteit Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 109ebc511ff33da5c8dcab97cd2c8332075265a5
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Aanroepen van opgeslagen procedure uit kopieeractiviteit in Azure Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van de activiteit opgeslagen procedure in Gegevensfactory versie 2](../transform-data-using-stored-procedure.md).


Bij het kopiëren van gegevens in [SQL Server](data-factory-sqlserver-connector.md) of [Azure SQL Database](data-factory-azure-sql-connector.md), kunt u de **SqlSink** in een kopieeractiviteit om aan te roepen, een opgeslagen procedure. U kunt gebruiken de opgeslagen procedure voor het uitvoeren van extra verwerking (samenvoegen kolommen, opzoeken van waarden, invoegen in meerdere tabellen, enz.) is vereist voordat u gegevens in de doeltabel. Deze functie wordt gebruikgemaakt van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx). 

Het volgende voorbeeld ziet u hoe aan te roepen, een opgeslagen procedure in een SQL Server-database van een Data Factory-pijplijn (kopieeractiviteit):  

## <a name="output-dataset-json"></a>Uitvoergegevensset JSON
In de uitvoergegevensset JSON stelt u de **type** naar: **SqlServerTable**. Stel deze in op **AzureSqlTable** voor gebruik met een Azure SQL database. De waarde voor **tableName** eigenschap moet overeenkomen met de naam van de eerste parameter van de opgeslagen procedure.  

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

## <a name="sqlsink-section-in-copy-activity-json"></a>De sectie SqlSink in kopieeractiviteit JSON
Definieer de **SqlSink** sectie als volgt in de JSON van de kopieeractiviteit. Als u wilt een opgeslagen procedure aanroepen tijdens het invoegen van gegevens in de doel-sink-database, kunt u waarden opgeven voor zowel **SqlWriterStoredProcedureName** en **SqlWriterTableType** eigenschappen. Zie voor beschrijvingen van deze eigenschappen [SqlSink-sectie in het SQL Server-connector artikel](data-factory-sqlserver-connector.md#sqlsink).

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
Definieer de opgeslagen procedure met dezelfde naam als in uw database **SqlWriterStoredProcedureName**. De opgeslagen procedure verwerkt invoergegevens uit de gegevensopslag van de bron en voegt de gegevens in een tabel in de doeldatabase. De naam van de eerste parameter van de opgeslagen procedure moet overeenkomen met de tabelnaam die wordt gedefinieerd in de gegevensset JSON (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>De typedefinitie van de tabel
Definieer het tabeltype met dezelfde naam als in uw database **SqlWriterTableType**. Het schema van het type moet overeenkomen met het schema van de invoergegevensset.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende connector artikelen die voor volledige JSON-voorbeelden: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
