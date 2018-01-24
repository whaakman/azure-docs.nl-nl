---
title: Transformeer gegevens met Hadoop-streamingactiviteit in Azure Data Factory | Microsoft Docs
description: Legt uit hoe het gebruik van Hadoop-Streamingactiviteit in Azure Data Factory om gegevens te transformeren met Hadoop-Streaming programma's uitvoeren op een Hadoop-cluster.
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
ms.openlocfilehash: 449b322089ed3881df6d87276c3461d18d697edf
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Transformeer gegevens met Hadoop-streamingactiviteit in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-hadoop-streaming-activity.md)
> * [Versie 2 - Preview](transform-data-using-hadoop-streaming.md)

De HDInsight Streaming-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) Hadoop-Streaming programma's worden uitgevoerd op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten toont.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Hadoop-Streamingactiviteit in V1](v1/data-factory-hadoop-streaming-activity.md).

Als u niet bekend met Azure Data Factory bent, Lees [Inleiding tot Azure Data Factory](introduction.md) en voer de [zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat u dit artikel leest. 

## <a name="json-sample"></a>JSON-voorbeeld
```json
{
    "name": "Streaming Activity",
    "description": "Description",
    "type": "HDInsightStreaming",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mapper": "MyMapper.exe",
        "reducer": "MyReducer.exe",
        "combiner": "MyCombiner.exe",
        "fileLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "filePaths": [
            "<containername>/example/apps/MyMapper.exe",
            "<containername>/example/apps/MyReducer.exe",
            "<containername>/example/apps/MyCombiner.exe"
        ],
        "input": "wasb://<containername>@<accountname>.blob.core.windows.net/example/input/MapperInput.txt",
        "output": "wasb://<containername>@<accountname>.blob.core.windows.net/example/output/ReducerOutput.txt",
        "commandEnvironment": [
            "CmdEnvVarName=CmdEnvVarValue"
        ],
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
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
| type              | Voor Hadoop-Streamingactiviteit is het activiteitstype HDInsightStreaming | Ja      |
| linkedServiceName | Verwijzing naar het HDInsight-cluster geregistreerd als een gekoppelde service in de Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| toewijzen            | Geeft de naam van de uitvoerbare toewijzen | Ja      |
| reducer           | Geeft de naam van de uitvoerbare reducer | Ja      |
| combiner          | Geeft de naam van de uitvoerbare combiner | Nee       |
| fileLinkedService | Verwijzing naar een Azure Storage Linked Service gebruikt voor het opslaan van de programma's toewijzen, Combiner en Reducer moet worden uitgevoerd. Als u deze gekoppelde Service niet opgeeft, wordt de Azure Storage Linked Service gedefinieerd in de gekoppelde HDInsight-Service wordt gebruikt. | Nee       |
| filePath          | Geef een matrix van pad tot Mapper Combiner, en Reducer programma's die zijn opgeslagen in Azure Storage waarnaar wordt verwezen door fileLinkedService. Het pad is hoofdlettergevoelig. | Ja      |
| Invoer             | Hiermee geeft u het WASB pad naar het bestand voor invoer voor de toewijzing. | Ja      |
| output            | Hiermee geeft u het pad WASB naar het uitvoerbestand voor de Reducer. | Ja      |
| getDebugInfo      | Geeft aan wanneer de logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door de scriptLinkedService. Toegestane waarden: None, altijd of fout. Standaardwaarde: geen. | Nee       |
| Argumenten         | Hiermee geeft u een matrix van de argumenten voor een Hadoop-taak. De argumenten zijn doorgegeven als opdrachtregelargumenten voor elke taak. | Nee       |
| Hiermee worden gedefinieerd           | Geef parameters op als sleutel-waardeparen voor verwijzende binnen het Hive-script. | Nee       | 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchuitvoering activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
