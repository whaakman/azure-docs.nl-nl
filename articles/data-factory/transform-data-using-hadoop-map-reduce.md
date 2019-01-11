---
title: Gegevens transformeren met behulp van Hadoop MapReduce-activiteit in Azure Data Factory | Microsoft Docs
description: Informatie over het verwerken van gegevens door het uitvoeren van Hadoop MapReduce programma's op een Azure HDInsight-cluster uit een Azure data factory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 4543982f731feb44a8f02581c11714dec2b206f9
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214501"
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Hadoop MapReduce-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-map-reduce.md)
> * [Huidige versie](transform-data-using-hadoop-map-reduce.md)

De HDInsight MapReduce-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) roept MapReduce-programma op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](introduction.md) en de zelfstudie: [Zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat het lezen van dit artikel.

Zie [Pig](transform-data-using-hadoop-pig.md) en [Hive](transform-data-using-hadoop-hive.md) voor meer informatie over het uitvoeren van Pig of Hive-scripts in een HDInsight-cluster van een pijplijn met behulp van HDInsight Pig en Hive-activiteiten.

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

## <a name="syntax-details"></a>Syntaxis van de details

| Eigenschap          | Description                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| naam              | Naam van de activiteit                     | Ja      |
| description       | Beschrijving van het doel waarvoor de activiteit wordt gebruikt | Nee       |
| type              | Voor MapReduce-activiteit wordt het activiteitstype HDinsightMapReduce | Ja      |
| linkedServiceName | Verwijzing naar het HDInsight-cluster geregistreerd als een gekoppelde service in Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| Klassenaam         | Naam van de klasse moet worden uitgevoerd         | Ja      |
| jarLinkedService  | Verwijzing naar een gekoppelde Azure Storage-Service gebruikt voor het opslaan van de Jar-bestanden. Als u deze gekoppelde Service niet opgeeft, wordt de Azure Storage gekoppelde Service gedefinieerd in de gekoppelde HDInsight-Service wordt gebruikt. | Nee       |
| jarFilePath       | Geef het pad naar het Jar-bestanden die zijn opgeslagen in Azure Storage waarnaar wordt verwezen door jarLinkedService. De bestandsnaam is hoofdlettergevoelig. | Ja      |
| jarlibs           | Matrix van het pad naar het Jar-bibliotheekbestanden waarnaar wordt verwezen door de taak die zijn opgeslagen in de Azure-opslag die is gedefinieerd in jarLinkedService de tekenreeks. De bestandsnaam is hoofdlettergevoelig. | Nee       |
| getDebugInfo      | Geeft aan wanneer de logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door jarLinkedService. Toegestane waarden: Geen altijd of fout. Standaardwaarde: Geen. | Nee       |
| argumenten         | Hiermee geeft u een matrix van de argumenten voor een Hadoop-taak. De argumenten worden doorgegeven als opdrachtregelargumenten aan elke taak. | Nee       |
| Hiermee worden gedefinieerd           | Geef parameters op als sleutel/waarde-paren voor verwijzende binnen het Hive-script. | Nee       |



## <a name="example"></a>Voorbeeld
U kunt de HDInsight MapReduce-activiteit gebruiken een MapReduce-jar-bestand op een HDInsight-cluster uit te voeren. In het volgende voorbeeld van JSON-definitie van een pijplijn, worden de HDInsight-activiteit is geconfigureerd voor het uitvoeren van een Mahout-JAR-bestand.

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
Kunt u geen argumenten op voor de MapReduce-programma in de **argumenten** sectie. Tijdens runtime, ziet u een paar extra argumenten (bijvoorbeeld: mapreduce.job.tags) van de MapReduce-framework. Overweeg het gebruik van optie en de waarde als argumenten, zoals wordt weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--input,--output enz., zijn opties onmiddellijk wordt gevolgd door hun waarden).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren:

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
