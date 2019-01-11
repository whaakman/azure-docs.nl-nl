---
title: Gegevens transformeren met behulp van de opgeslagen Procedure-activiteit in Azure Data Factory | Microsoft Docs
description: Wordt uitgelegd hoe u SQL Server opgeslagen Procedure-activiteit gebruikt om aan te roepen een opgeslagen procedure in een Azure SQL Database/Data Warehouse vanuit een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: douglasl
ms.openlocfilehash: 9a724f8e319e652b85941810a6312c35a5036120
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025722"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van de SQL Server opgeslagen Procedure-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-stored-proc-activity.md)
> * [Huidige versie](transform-data-using-stored-procedure.md)

Gebruik van activiteiten voor gegevenstransformatie in een Data Factory [pijplijn](concepts-pipelines-activities.md) transformeren en verwerken van onbewerkte gegevens in voorspellingen en inzichten. De activiteit opgeslagen Procedure is een van de activiteiten voor gegevenstransformatie die ondersteuning biedt voor Data Factory. In dit artikel is gebaseerd op de [gegevens transformeren](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten in Data Factory geeft.

> [!NOTE]
> Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](introduction.md) en de zelfstudie: [Zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat het lezen van dit artikel. 

U kunt de activiteit opgeslagen Procedure gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevensarchieven in uw onderneming of op een Azure-machine (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server-Database.  Als u SQL Server gebruikt, installeert u zelf-hostende integratieruntime op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer die toegang tot de database heeft. Zelf-Hostende integratieruntime is een onderdeel waarmee gegevens verbonden gegevensbronnen on-premises/op virtuele Azure-machine met cloudservices op een veilige, beheerde manier. Zie [zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

> [!IMPORTANT]
> Het kopiëren van gegevens in Azure SQL Database of SQL Server, kunt u de **SqlSink** in de kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Voor meer informatie over de eigenschap, naar het volgende connector artikelen: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). Aanroepen van een opgeslagen procedure tijdens het kopiëren van gegevens in een Azure SQL Data Warehouse met behulp van een kopieeractiviteit wordt niet ondersteund. Maar u kunt de opgeslagen procedure-activiteit gebruiken voor het aanroepen van een opgeslagen procedure in een SQL Data Warehouse. 
>
> Het kopiëren van gegevens uit Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in de kopieeractiviteit om aan te roepen een opgeslagen procedure voor het lezen van gegevens vanuit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure SQL datawarehouse](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Syntaxis van de details
Dit is de JSON-indeling voor het definiëren van een activiteit opgeslagen Procedure:

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

| Eigenschap                  | Description                              | Vereist |
| ------------------------- | ---------------------------------------- | -------- |
| naam                      | Naam van de activiteit                     | Ja      |
| description               | Beschrijving van het doel waarvoor de activiteit wordt gebruikt | Nee       |
| type                      | Voor de opgeslagen Procedureactiviteit, het activiteitstype is **SqlServerStoredProcedure** | Ja      |
| linkedServiceName         | De verwijzing naar de **Azure SQL Database** of **Azure SQL Data Warehouse** of **SQL Server** geregistreerd als een gekoppelde service in Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| storedProcedureName       | Geef de naam van de opgeslagen procedure om aan te roepen. | Ja      |
| storedProcedureParameters | Geef de waarden voor parameters van de opgeslagen procedure. Gebruik `"param1": { "value": "param1Value","type":"param1Type" }` doorgeven van parameterwaarden en hun type wordt ondersteund door de gegevensbron. Als u moet null zijn voor een parameter door te geven, gebruikt `"param1": { "value": null }` (alleen kleine letters). | Nee       |

## <a name="error-info"></a>Foutgegevens

Wanneer een opgeslagen procedure mislukt en retourneert de foutdetails, kunt u de foutgegevens rechtstreeks in de uitvoer van de activiteit niet kan vastleggen. Data Factory pumps echter alle van de activiteit die wordt uitgevoerd van gebeurtenissen naar Azure Monitor. Tussen de gebeurtenissen die Data Factory pompen naar Azure Monitor, gepusht foutdetails er. U kunt bijvoorbeeld e-mailwaarschuwingen instellen van deze gebeurtenissen. Zie voor meer informatie, [waarschuwen en bewaken van data factory's met behulp van Azure Monitor](monitor-using-azure-monitor.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-Streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Bach tot uitvoering van activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
