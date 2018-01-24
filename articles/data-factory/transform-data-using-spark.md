---
title: Transformeer gegevens met behulp van de activiteit Spark in Azure Data Factory | Microsoft Docs
description: Informatie over het om gegevens te transformeren door het uitvoeren van Spark programma's in een Azure data factory-pijplijn met behulp van de Spark-activiteit.
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: 4aed91696b5853b56ab17d69753d20081c79cdf7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-spark-activity-in-azure-data-factory"></a>Transformeer gegevens met behulp van de activiteit Spark in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-spark.md)
> * [Versie 2 - Preview](transform-data-using-spark.md)

De Spark-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) Hiermee voert u een Spark-programma op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont. Wanneer u een gekoppelde Spark-service op aanvraag, wordt Data Factory maakt automatisch een Spark-cluster voor u just-in-time om de gegevens te verwerken en vervolgens het cluster wordt verwijderd zodra de verwerking voltooid is. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Spark-activiteit in V1](v1/data-factory-spark.md).

> [!IMPORTANT]
> Spark activiteit biedt geen ondersteuning voor HDInsight Spark-clusters die gebruikmaken van een Azure Data Lake Store als primaire opslag.

## <a name="spark-activity-properties"></a>Spark-activiteitseigenschappen
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
        "arguments": [ "arg1", "arg2" ],
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

De volgende tabel beschrijft de JSON-eigenschappen die in de JSON-definitie:

| Eigenschap              | Beschrijving                              | Vereist |
| --------------------- | ---------------------------------------- | -------- |
| naam                  | De naam van de activiteit in de pijplijn.    | Ja      |
| description           | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                  | Het activiteitstype is HDInsightSpark voor Spark-activiteit. | Ja      |
| linkedServiceName     | Naam van de Spark gekoppelde HDInsight-Service op het Spark-programma wordt uitgevoerd. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| SparkJobLinkedService | Azure Storage gekoppelde service die de Spark taakbestand, afhankelijkheden en Logboeken bevat.  Als u een waarde op voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan de HDInsight-cluster gebruikt. | Nee       |
| rootPath              | De Azure Blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. Raadpleeg de mapstructuur sectie (volgende sectie) voor meer informatie over de structuur van deze map. | Ja      |
| entryFilePath         | Relatief pad naar de hoofdmap van het Spark/codepakket. | Ja      |
| className             | Belangrijkste Java/Spark-klasse van de toepassing      | Nee       |
| Argumenten             | Een lijst met opdrachtregelargumenten aan het programma Spark. | Nee       |
| proxyUser             | De account van de gebruiker te imiteren voor het uitvoeren van het Spark-programma | Nee       |
| sparkConfig           | Geef waarden voor Spark configuratie-eigenschappen die worden vermeld in het onderwerp: [Spark-configuratie - eigenschappen van Application](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Nee       |
| getDebugInfo          | Geeft aan wanneer de Spark-logboekbestanden worden gekopieerd naar de Azure-opslag door HDInsight-cluster gebruikt (of) opgegeven door sparkJobLinkedService. Toegestane waarden: None, altijd of fout. Standaardwaarde: geen. | Nee       |

## <a name="folder-structure"></a>Mapstructuur
Spark-taken zijn beter dan Pig/Hive-taken worden uitgebreid. Voor Spark taken, kunt u meerdere afhankelijkheden, zoals opgeven jar-pakketten (geplaatst in de java CLASSPATH), python-bestanden (geplaatst op de PYTHONPATH) en andere bestanden.

De volgende mapstructuur maken in Azure Blob storage waarnaar wordt verwezen door de gekoppelde HDInsight-service. Vervolgens afhankelijke bestanden uploaden naar de juiste submappen in de hoofdmap dat wordt vertegenwoordigd door **entryFilePath**. Bijvoorbeeld, python-bestanden naar de submap pyFiles en jar-bestanden uploaden naar de submap potten van de hoofdmap. Tijdens runtime verwacht Data Factory-service de volgende mapstructuur in de Azure Blob-opslag:     

| Pad                  | Beschrijving                              | Vereist | Type   |
| --------------------- | ---------------------------------------- | -------- | ------ |
| `.`(root)            | Het pad naar de hoofdmap van de taak Spark in gekoppelde storage-service | Ja      | Map |
| &lt;de gebruiker gedefinieerde&gt; | Het pad verwijst naar het bestand vermelding van de Spark-taak | Ja      | File   |
| . / jars                | Alle bestanden onder deze map worden geüpload en op de java-klassenpad van het cluster geplaatst | Nee       | Map |
| . / pyFiles             | Alle bestanden onder deze map worden geüpload en op de PYTHONPATH van het cluster geplaatst | Nee       | Map |
| . / bestanden               | Alle bestanden onder deze map worden geüpload en in de werkmap executor geplaatst | Nee       | Map |
| . / archiveert            | Alle bestanden onder deze map zijn gecomprimeerd | Nee       | Map |
| . / Logboeken                | De map met Logboeken van het Spark-cluster. | Nee       | Map |

Hier volgt een voorbeeld van een opslagruimte met twee Spark taakbestanden in de Azure-blobopslag waarnaar wordt verwezen door de gekoppelde HDInsight-service.

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
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchuitvoering activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
