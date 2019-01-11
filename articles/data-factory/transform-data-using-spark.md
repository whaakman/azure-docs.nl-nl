---
title: Gegevens transformeren met behulp van Spark-activiteit in Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens transformeren met Spark actieve programma's van een Azure data factory-pijplijn met behulp van de Spark-activiteit.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: douglasl
ms.openlocfilehash: a25505a976be9d9ae38f562591d86ca9b56b8859
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025605"
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Spark-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-spark.md)
> * [Huidige versie](transform-data-using-spark.md)

De Spark-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) voert u een Spark-programma op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft. Wanneer u een gekoppelde Spark-service op aanvraag, wordt in Data Factory maakt automatisch een Spark-cluster voor u just-in-time voor het verwerken van de gegevens en vervolgens het cluster worden verwijderd nadat de verwerking voltooid is. 

> [!IMPORTANT]
> Spark-activiteit biedt geen ondersteuning voor HDInsight Spark-clusters die gebruikmaken van een Azure Data Lake Store als primaire opslag.

## <a name="spark-activity-properties"></a>Eigenschappen van de Spark-activiteit
Hier volgt de voorbeeld-JSON-definitie van een Spark-activiteit:    

```json
{
    "name": "Spark Activity",
    "description": "Description",
    "type": "HDInsightSpark",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "sparkJobLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "rootPath": "adfspark\\pyFiles",
        "entryFilePath": "test.py",
        "sparkConfig": {
            "ConfigItem1": "Value"
        },
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ]
    }
}
```

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie gebruikt:

| Eigenschap              | Description                              | Vereist |
| --------------------- | ---------------------------------------- | -------- |
| naam                  | Naam van de activiteit in de pijplijn.    | Ja      |
| description           | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                  | Voor Spark-activiteit is het activiteitstype HDInsightSpark. | Ja      |
| linkedServiceName     | De naam van de HDInsight Spark gekoppelde Service waarop het Spark-programma wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| SparkJobLinkedService | Azure Storage gekoppelde service waarin de Spark taakbestand, afhankelijkheden en Logboeken.  Als u een waarde voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan het HDInsight-cluster gebruikt. De waarde van deze eigenschap kan alleen worden voor een gekoppelde Azure Storage-service. | Nee       |
| rootPath              | Het Azure Blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. Verwijzen naar de mapstructuur sectie (volgende sectie) voor meer informatie over de structuur van deze map. | Ja      |
| entryFilePath         | Relatief pad naar de hoofdmap van de Spark-code of pakket. De post-bestand moet een Python-bestand of een JAR-bestand. | Ja      |
| Klassenaam             | Java/Spark-hoofdklasse van de toepassing      | Nee       |
| argumenten             | Een lijst met opdrachtregelargumenten op het Spark-programma. | Nee       |
| proxyUser             | De account van de gebruiker te imiteren voor het uitvoeren van het Spark-programma | Nee       |
| sparkConfig           | Geef waarden op voor Spark configuratie-eigenschappen die worden vermeld in het onderwerp: [Spark-configuratie - eigenschappen voor de toepassing](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nee       |
| getDebugInfo          | Hiermee geeft u aan bij de Spark-logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door sparkJobLinkedService. Toegestane waarden: Geen altijd of fout. Standaardwaarde: Geen. | Nee       |

## <a name="folder-structure"></a>mapstructuur
Spark-taken zijn beter worden uitgebreid dan Pig of Hive-taken. Voor Spark-taken, kunt u meerdere afhankelijkheden zoals bieden jar-pakketten (in de java KLASSEPAD geplaatst), python-bestanden (geplaatst op de PYTHONPATH) en andere bestanden.

De volgende mapstructuur maken in de Azure Blob-opslag waarnaar wordt verwezen door de gekoppelde HDInsight-service. Vervolgens kunt u afhankelijke bestanden uploaden naar de juiste submappen in de hoofdmap wordt vertegenwoordigd door **entryFilePath**. Bijvoorbeeld, python-bestanden naar de submap pyFiles en jar-bestanden uploaden naar de submap JAR-bestanden van de hoofdmap. Tijdens runtime verwacht Data Factory-service de volgende mapstructuur in de Azure Blob-opslag:     

| Pad                  | Description                              | Vereist | Type   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.` (root)            | Het pad naar de hoofdmap van de Spark-taak in de gekoppelde storage-service | Ja      | Map |
| &lt;door de gebruiker gedefinieerde &gt; | Het pad dat verwijst naar het bestand vermelding van de Spark-taak | Ja      | File   |
| . / jars                | Alle bestanden onder deze map worden geüpload en in het klassepad java van het cluster geplaatst | Nee       | Map |
| . / pyFiles             | Alle bestanden onder deze map worden geüpload en op de PYTHONPATH van het cluster geplaatst | Nee       | Map |
| . / -bestanden               | Alle bestanden onder deze map worden geüpload en geplaatst op de executor-werkmap | Nee       | Map |
| . / archieven            | In deze map alle bestanden zijn gecomprimeerd | Nee       | Map |
| . / -Logboeken                | De map met Logboeken van het Spark-cluster. | Nee       | Map |

Hier volgt een voorbeeld van een opslagruimte met twee bestanden van Spark-taak in de Azure Blob-opslag waarnaar wordt verwezen door de gekoppelde HDInsight-service.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
