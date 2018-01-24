---
title: Transformeer gegevens met Hadoop-MapReduce-activiteit in Azure Data Factory | Microsoft Docs
description: Informatie over het verwerken van gegevens met Hadoop MapReduce programma's uitvoeren op een Azure HDInsight-cluster uit een Azure data factory.
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
ms.openlocfilehash: c1fbb6864629874ef116cdf81d48df4a9ed5af1f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Transformeer gegevens met Hadoop-MapReduce-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-map-reduce.md)
> * [Versie 2 - Preview](transform-data-using-hadoop-map-reduce.md)


De HDInsight MapReduce-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) MapReduce programma roept op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [MapReduce-activiteit in V1](v1/data-factory-map-reduce.md).


Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](introduction.md) en voer de zelfstudie: [zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat u dit artikel leest. 

Zie [Pig](transform-data-using-hadoop-pig.md) en [Hive](transform-data-using-hadoop-hive.md) voor meer informatie over het uitvoeren van Pig/Hive scripts op een HDInsight-cluster van een pijplijn met behulp van HDInsight Pig en Hive-activiteiten. 

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "Map Reduce Activity",
    "description": "Description",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.myorg.SampleClass",
        "jarLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "MyAzureStorage/jars/sample.jar",
        "getDebugInfo": "Failure",
        "arguments": [
          "-SampleHadoopJobArgument1"
        ],
        "defines": {
          "param1": "param1Value"
        }
    }
}
```

## <a name="syntax-details"></a>Details van de syntaxis

| Eigenschap          | Beschrijving                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| naam              | Naam van de activiteit                     | Ja      |
| description       | Beschrijving van wat de activiteit wordt gebruikt | Nee       |
| type              | Voor MapReduce-activiteit wordt het activiteitstype HDinsightMapReduce | Ja      |
| linkedServiceName | Verwijzing naar het HDInsight-cluster geregistreerd als een gekoppelde service in de Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| className         | Naam van de klasse om te worden uitgevoerd         | Ja      |
| jarLinkedService  | Verwijzing naar een Azure Storage Linked Service gebruikt voor het opslaan van de Jar-bestanden. Als u deze gekoppelde Service niet opgeeft, wordt de Azure Storage Linked Service gedefinieerd in de gekoppelde HDInsight-Service wordt gebruikt. | Nee       |
| jarFilePath       | Geef het pad naar het Jar-bestanden opgeslagen in Azure Storage waarnaar wordt verwezen door jarLinkedService. De bestandsnaam is hoofdlettergevoelig. | Ja      |
| jarlibs           | Matrix van het pad naar het Jar-bibliotheekbestanden waarnaar wordt verwezen door de taak die is opgeslagen in de Azure-opslag dat is gedefinieerd in jarLinkedService tekenreeks. De bestandsnaam is hoofdlettergevoelig. | Nee       |
| getDebugInfo      | Geeft aan wanneer de logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door jarLinkedService. Toegestane waarden: None, altijd of fout. Standaardwaarde: geen. | Nee       |
| Argumenten         | Hiermee geeft u een matrix van de argumenten voor een Hadoop-taak. De argumenten zijn doorgegeven als opdrachtregelargumenten voor elke taak. | Nee       |
| Hiermee worden gedefinieerd           | Geef parameters op als sleutel-waardeparen voor verwijzende binnen het Hive-script. | Nee       |



## <a name="example"></a>Voorbeeld
U kunt de HDInsight-MapReduce-activiteit gebruiken een MapReduce jar-bestand op een HDInsight-cluster uit te voeren. In het volgende voorbeeld JSON-definitie van een pijplijn, worden de HDInsight-activiteit is geconfigureerd voor het uitvoeren van een Mahout JAR-bestand.

```json   
{
    "name": "MapReduce Activity for Mahout",
    "description": "Custom MapReduce to generate Mahout result",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
        "jarLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
        "arguments": [
            "-s",
            "SIMILARITY_LOGLIKELIHOOD",
            "--input",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
            "--output",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
            "--maxSimilaritiesPerItem",
            "500",
            "--tempDir",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
        ]
    }
}
```
U kunt opgeven dat geen argumenten voor de MapReduce-programma in de **argumenten** sectie. Tijdens runtime, ziet u enkele extra argumenten (bijvoorbeeld: mapreduce.job.tags) van het MapReduce-framework. Overweeg het gebruik van zowel de optie als de waarde als argumenten zoals weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--invoer,--uitvoer enz., zijn opties onmiddellijk wordt gevolgd door hun waarden).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchuitvoering activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
