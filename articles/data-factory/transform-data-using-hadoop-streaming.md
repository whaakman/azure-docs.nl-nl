---
title: Gegevens transformeren met behulp van Hadoop-Streaming-activiteit in Azure Data Factory | Microsoft Docs
description: Wordt uitgelegd hoe u Hadoop-Streamingactiviteit in Azure Data Factory om gegevens te transformeren door het uitvoeren van Hadoop-Streaming-programma's op een Hadoop-cluster.
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
ms.openlocfilehash: b498e09e53f8b0844470bf3948a664d8ad4337b7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022220"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Hadoop-Streaming-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-hadoop-streaming-activity.md)
> * [Huidige versie](transform-data-using-hadoop-streaming.md)

De HDInsight Streaming-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) uitvoeren van Hadoop-Streaming-programma's op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](introduction.md) en voer de [zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat het lezen van dit artikel. 

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

## <a name="syntax-details"></a>Syntaxis van de details

| Eigenschap          | Description                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| naam              | Naam van de activiteit                     | Ja      |
| description       | Beschrijving van het doel waarvoor de activiteit wordt gebruikt | Nee       |
| type              | Voor Hadoop-Streamingactiviteit is het activiteitstype HDInsightStreaming | Ja      |
| linkedServiceName | Verwijzing naar het HDInsight-cluster geregistreerd als een gekoppelde service in Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| toewijzen            | Hiermee geeft u de naam van het toewijzen van de uitvoerbare | Ja      |
| reducer           | Hiermee geeft u de naam van het uitvoerbare bestand reducer | Ja      |
| combiner          | Hiermee geeft u de naam van het uitvoerbare bestand combiner | Nee       |
| fileLinkedService | Verwijzing naar een gekoppelde Azure Storage-Service gebruikt voor het opslaan van de programma's toewijzen, Combiner en Reducer moet worden uitgevoerd. Als u deze gekoppelde Service niet opgeeft, wordt de Azure Storage gekoppelde Service gedefinieerd in de gekoppelde HDInsight-Service wordt gebruikt. | Nee       |
| filePath          | Een matrix van pad naar de Mapper, Combiner, bieden en Reducer programma's die zijn opgeslagen in Azure Storage waarnaar wordt verwezen door fileLinkedService. Het pad is hoofdlettergevoelig. | Ja      |
| invoer             | Hiermee geeft u het WASB-pad naar het invoerbestand voor het toewijzen van de. | Ja      |
| output            | Hiermee geeft u het WASB-pad naar het uitvoerbestand voor de Reducer. | Ja      |
| getDebugInfo      | Geeft aan wanneer de logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door scriptLinkedService. Toegestane waarden: Geen altijd of fout. Standaardwaarde: Geen. | Nee       |
| argumenten         | Hiermee geeft u een matrix van de argumenten voor een Hadoop-taak. De argumenten worden doorgegeven als opdrachtregelargumenten aan elke taak. | Nee       |
| Hiermee worden gedefinieerd           | Geef parameters op als sleutel/waarde-paren voor verwijzende binnen het Hive-script. | Nee       | 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
