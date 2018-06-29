---
title: Transformeer gegevens met behulp van de activiteit opgeslagen Procedure in Azure Data Factory | Microsoft Docs
description: Legt uit hoe u de activiteit opgeslagen Procedure van SQL Server gebruiken om aan te roepen, een opgeslagen procedure in een Azure SQL Database/datawarehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: e8e0f8352404892ea8af6a0fa176c336dd2c1659
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054021"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Transformeer gegevens met behulp van de activiteit opgeslagen Procedure van SQL Server in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1](v1/data-factory-stored-proc-activity.md)
> * [Huidige versie](transform-data-using-stored-procedure.md)

U activiteiten voor gegevenstransformatie gebruiken in een Data Factory [pijplijn](concepts-pipelines-activities.md) transformeren en verwerken van onbewerkte gegevens in de voorspellingen en inzichten. De activiteit opgeslagen Procedure is een van de activiteiten voor gegevenstransformatie die ondersteuning biedt voor Data Factory. In dit artikel is gebaseerd op de [gegevens transformeren](transform-data.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten in de Data Factory toont.

> [!NOTE]
> Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](introduction.md) en voer de zelfstudie: [zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat u dit artikel leest. 

U kunt de activiteit opgeslagen Procedure gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevens worden opgeslagen in uw onderneming of op Azure een virtuele machine (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server-Database.  Als u van SQL Server gebruikmaakt, host zichzelf integratie runtime installeren op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer die toegang heeft tot de database. Host zichzelf integratie runtime is een onderdeel dat wordt verbinding gemaakt op-premises/op virtuele machine van Azure met cloudservices gegevensbronnen op een manier veilig en beheerd. Zie [zelf gehost integratie runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

> [!IMPORTANT]
> Wanneer gegevens worden gekopieerd naar Azure SQL Database of SQL Server, kunt u de **SqlSink** in een kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Zie voor meer informatie over de eigenschap connector artikelen te volgen: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). Een opgeslagen procedure aanroepen tijdens het kopiëren van gegevens in een Azure SQL Data Warehouse met behulp van een kopieeractiviteit wordt niet ondersteund. Maar u kunt de activiteit opgeslagen procedure gebruiken om aan te roepen, een opgeslagen procedure in een SQL Data Warehouse. 
>
> Wanneer het kopiëren van gegevens van Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in een kopieeractiviteit om aan te roepen, een opgeslagen procedure voor het lezen van gegevens uit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Details van de syntaxis
Hier volgt de JSON-indeling voor het definiëren van een activiteit opgeslagen Procedure:

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

De volgende tabel beschrijft deze JSON-eigenschappen:

| Eigenschap                  | Beschrijving                              | Vereist |
| ------------------------- | ---------------------------------------- | -------- |
| naam                      | Naam van de activiteit                     | Ja      |
| description               | Beschrijving van wat de activiteit wordt gebruikt | Nee       |
| type                      | Voor de activiteit opgeslagen Procedure, het activiteitstype is **SqlServerStoredProcedure** | Ja      |
| linkedServiceName         | Verwijzing naar de **Azure SQL Database** of **Azure SQL Data Warehouse** of **SQL Server** geregistreerd als een gekoppelde service in de Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| storedProcedureName       | Geef de naam van de opgeslagen procedure aan te roepen. | Ja      |
| storedProcedureParameters | Geef de waarden voor parameters van opgeslagen procedure. Gebruik `"param1": { "value": "param1Value","type":"param1Type" }` doorgeven parameterwaarden en hun type dat wordt ondersteund door de gegevensbron. Als u null zijn voor een parameter doorgeven moet, gebruikt u `"param1": { "value": null }` (alle kleine letter). | Nee       |

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Bach uitvoering van activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
