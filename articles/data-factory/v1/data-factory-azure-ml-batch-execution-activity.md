---
title: Voorspellende pijplijnen met behulp van Azure Data Factory maken | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen maken met Azure Data Factory en Azure Machine Learning
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4093febd19d71512e3c80704e88f9d5cf669d7d9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122060"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Voorspellende pijplijnen maken met Azure Machine Learning en Azure Data Factory

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md)
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning-batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md)
> * [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [Aangepaste .NET-activiteit](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Inleiding
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van machine learning in Data Factory](../transform-data-using-machine-learning.md).


### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt bouwen, testen en implementeren van predictive analytics-oplossingen. Uit een oogpunt van de op hoog niveau is voltooid in drie stappen:

1. **Maken van een opleidingsexperiment**. U kunt deze stap uitvoert met behulp van de Azure Machine Learning studio. Azure Machine Learning studio is een samenwerkingsverband visual ontwikkelomgeving die u gebruiken om te trainen en testen van een predictive analytics-model met behulp van de trainingsgegevens.
2. **Converteren naar een Voorspellend experiment**. Zodra uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het scoren van nieuwe gegevens, kunt u voorbereidt en stroomlijnen van uw experiment voor het scoren.
3. **Als een webservice implementeren**. U kunt uw scoring experiment publiceren als een Azure-web-service. U kunt gegevens aan uw model via deze web service-eindpunt verzenden en ontvangen resultaat voorspellingen van het model.

### <a name="azure-data-factory"></a>Azure Data Factory
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de **verplaatsing** en **transformatie** van gegevens wordt beheerd en geautomatiseerd. U kunt oplossingen voor gegevensintegratie maken met Azure Data Factory die u kunnen gegevens uit verschillende gegevensarchieven opnemen, de gegevens transformeren en verwerken, en de resulterende gegevens naar de gegevensarchieven publiceren.

Met Data Factory-service kunt u gegevenspijplijnen maken die gegevens verplaatsen en transformeren, en de pijplijnen vervolgens uitvoeren volgens een opgegeven schema (per uur, dagelijks, wekelijks enz.). U vindt uitgebreide visualisaties om de afkomst en afhankelijkheden tussen uw gegevenspijplijnen weer te geven en al uw gegevenspijplijnen te controleren vanuit één centrale weergave zodat u eenvoudig problemen kunt detecteren en bewakingswaarschuwingen kunt instellen.

Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) en [uw eerste pijplijn bouwen](data-factory-build-your-first-pipeline.md) artikelen snel aan de slag met Azure Data Factory-service.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory en Machine Learning samen
Azure Data Factory kunt u eenvoudig maken pijplijnen die gebruikmaken van een gepubliceerde [Azure Machine Learning] [ azure-machine-learning] webservice voor voorspellende analyses. Met behulp van de **Batchuitvoeringsactiviteit** in een Azure Data Factory-pijplijn, kunt u een Azure Machine Learning studio-webservice om voorspellingen te maken van de gegevens in batch aanroepen. Zie aanroepen van een Azure Machine Learning studio-webservice met behulp van de Batch Execution-activiteit-gedeelte voor meer informatie.

De voorspellende modellen in de Azure Machine Learning studio scoren experimenten moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. U kunt een opnieuw trainen van een Azure Machine Learning studio-model van een Data Factory-pijplijn door de volgende stappen:

1. Het trainingsexperiment (niet Voorspellend experiment) als een webservice publiceren. U voert deze stap in de Azure Machine Learning studio, net als Voorspellend experiment als een webservice in het voorgaande scenario beschikbaar wilt maken.
2. U Azure Machine Learning studio Batch Execution-activiteit gebruikt voor het aanroepen van de webservice voor de trainingsexperiment. In feite, kunt u de Azure Machine Learning studio Batch Execution-activiteit om aan te roepen zowel webservice training en scoring-webservice.

Als u klaar bent met het opnieuw trainen, bijwerken van de webservice scoring (Voorspellend experiment weergegeven als een webservice) met het zojuist getrainde model met behulp van de **Azure Machine Learning studio activiteit resources bijwerken**. Zie [bijwerken van modellen met behulp van de activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md) artikel voor meer informatie.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Aanroepen van een webservice met behulp van Batch Execution-activiteit
U Azure Data Factory gebruiken voor het indelen van gegevensverplaatsing en -verwerking en vervolgens uitvoeren met behulp van Azure Machine Learning-batchuitvoering. Hier volgen de stappen op het hoogste niveau:

1. Maak een Azure Machine Learning gekoppelde service. U moet de volgende waarden:

   1. **Aanvraag-URI** voor het uitvoeren van Batch API. U kunt de aanvraag-URI vinden door te klikken op de **BATCHUITVOERING** koppeling in de webpagina van de services.
   2. **API-sleutel** voor de gepubliceerde Azure Machine Learning-webservice. U kunt de API-sleutel vinden door te klikken op de webservice die u hebt gepubliceerd.
   3. Gebruik de **AzureMLBatchExecution** activiteit.

      ![Machine Learning Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch-URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario: Met behulp van Web service invoer/uitvoer die naar gegevens in Azure Blob Storage verwijzen-experimenten
In dit scenario, de Azure Machine Learning-webservice Hiermee worden voorspellingen gedaan met behulp van gegevens uit een bestand in een Azure blob-opslag en worden de voorspellingsresultaten opgeslagen in de blob-opslag. De volgende JSON wordt een Data Factory-pijplijn met een activiteit AzureMLBatchExecution gedefinieerd. De activiteit heeft de gegevensset **DecisionTreeInputBlob** als invoer en **DecisionTreeResultBlob** als de uitvoer. De **DecisionTreeInputBlob** wordt doorgegeven als invoer voor de webservice met behulp van de **webServiceInput** JSON-eigenschap. De **DecisionTreeResultBlob** wordt doorgegeven als een uitvoer aan de Web-service met behulp van de **webServiceOutputs** JSON-eigenschap.

> [!IMPORTANT]
> Als de webservice meerdere invoergegevens heeft, gebruikt u de **webServiceInputs** in plaats van de eigenschap **webServiceInput**. Zie de [webservice moet meerdere invoergegevens](#web-service-requires-multiple-inputs) sectie voor een voorbeeld van het gebruik van de eigenschap webServiceInputs.
>
> Gegevenssets waarnaar wordt verwezen door de **webServiceInput**/**webServiceInputs** en **webServiceOutputs** eigenschappen (in  **typeProperties**) moet ook zijn opgenomen in de activiteit **invoer** en **levert**.
>
> In de Azure Machine Learning studio-experiment hebben web service invoer en uitvoerpoorten en globale parameters standaardnamen ('input1', 'input2') die u kunt aanpassen. De namen die u voor het webServiceInputs webServiceOutputs en globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de Help-pagina voor het eindpunt van de Azure Machine Learning studio om te controleren of de verwachte toewijzing.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Alleen de invoer en uitvoer van de activiteit AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In de bovenstaande JSON-codefragment is DecisionTreeInputBlob bijvoorbeeld invoer voor de activiteit AzureMLBatchExecution, die wordt doorgegeven als invoer voor de webservice via webServiceInput-parameter.
>
>

### <a name="example"></a>Voorbeeld
In dit voorbeeld maakt gebruik van Azure Storage voor het opslaan van de invoer- en gegevens.

Het is raadzaam dat u doorloopt de [uw eerste pijplijn met Data Factory maken] [ adf-build-1st-pipeline] zelfstudie voordat u verdergaat met dit voorbeeld. De Data Factory-Editor gebruiken voor het maken van Data Factory-artefacten (gekoppelde services, gegevenssets, pijplijn) in dit voorbeeld.

1. Maak een **gekoppelde service** voor uw **Azure Storage**. Als de invoer en uitvoer bestanden zich in verschillende opslagaccounts, moet u twee gekoppelde services. Hier volgt een voorbeeld van JSON:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Maak de **invoer** Azure Data Factory **gegevensset**. In tegenstelling tot sommige andere Data Factory-gegevenssets, deze gegevenssets, zowel moet bevatten **folderPath** en **fileName** waarden. U kunt partitioneren ervoor zorgen dat elke batch niet uitvoeren (elke data slice) om te verwerken of produceren unieke invoer en uitvoer bestanden. Mogelijk moet u enkele upstream-activiteit voor het omzetten van de invoer in de CSV-bestandsindeling en plaats deze in de storage-account voor elk segment bevatten. In dat geval kunt u niet wilt opnemen de **externe** en **externalData** instellingen die worden weergegeven in het volgende voorbeeld, en uw DecisionTreeInputBlob zou de uitvoergegevensset van een andere activiteit.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    Uw invoer csv-bestand moet de veldnamenrij kolom hebben. Als u de **Kopieeractiviteit** als u wilt maken of verwijderen de csv naar de blob-opslag, moet u de sink-eigenschap instellen **blobWriterAddHeader** naar **waar**. Bijvoorbeeld:

    ```JSON
    sink:
    {
        "type": "BlobSink",
        "blobWriterAddHeader": true
    }
    ```

    Als het csv-bestand is geen rij met koppen, ziet u mogelijk de volgende fout: **Fout in de activiteit: Fout bij lezen tekenreeks. Onverwacht token: StartObject. Pad '', regel 1, positie 1**.
3. Maak de **uitvoer** Azure Data Factory **gegevensset**. In dit voorbeeld maakt gebruik van partitionering om te maken van een unieke uitvoerpad voor elk segment worden uitgevoerd. Zonder de partitionering, zou de activiteit bestand overschreven.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Maak een **gekoppelde service** van het type: **AzureMLLinkedService**, waarbij de API-sleutel en model van de volgende URL.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Ten slotte ontwerpen van een pijplijn met een **AzureMLBatchExecution** activiteit. Tijdens runtime, voert de pijplijn uit de volgende stappen uit:

   1. Hiermee haalt u de locatie van het invoerbestand van uw invoergegevenssets.
   2. De Batchuitvoering van Azure Machine Learning API wordt aangeroepen
   3. De uitvoer van de batch-uitvoering kopieert naar de blob die is opgegeven in de uitvoergegevensset.

      > [!NOTE]
      > AzureMLBatchExecution activiteit kan nul of meer invoer en uitvoer van een of meer hebben.
      >
      >

      ```JSON
      {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
      }
      ```

      Beide **start** en **end** datum/tijd moet zich in [ISO-indeling](https://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. De **end** is optioneel. Als u geen waarde voor de **end** eigenschap, wordt berekend als '**start + 48 uur.**" Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**. Zie de [naslaginformatie voor JSON-scriptverwerking](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de JSON-eigenschappen.

      > [!NOTE]
      > Activiteit is op te geven voor de AzureMLBatchExecution invoer optioneel.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario: Experimenten Reader/Writer-Modules gebruiken om te verwijzen naar gegevens in verschillende opslagruimten
Er is een ander gebruikelijk scenario bij het maken van Azure Machine Learning studio-experimenten met lees- en schrijftoegang modules. De reader-module wordt gebruikt om gegevens te laden in een experiment en de module writer is bij het opslaan van gegevens vanuit uw experimenten. Zie voor meer informatie over lees- en schrijftoegang modules [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [schrijver](https://msdn.microsoft.com/library/azure/dn905984.aspx) -onderwerpen op MSDN-bibliotheek.

Wanneer u de lees- en schrijftoegang modules, is het verstandig aan een Web service-parameter gebruiken voor elke eigenschap van deze reader/writer-modules. Deze web-parameters kunnen u de waarden worden geconfigureerd tijdens runtime. U kunt bijvoorbeeld een experiment maken met een reader-module die gebruikmaakt van een Azure SQL Database: XXX.database.windows.net. Nadat de web-service is geïmplementeerd, die u wilt de consumenten van de webservice om op te geven van een Azure SQL-Server met de naam YYY.database.windows.net inschakelen. Een Web service-parameter kunt u toestaan dat deze waarde moet worden geconfigureerd.

> [!NOTE]
> Web service invoer en uitvoer verschillen van de parameters van de Web service. In het eerste scenario, hebt u gezien hoe een invoer en uitvoer voor een Azure Machine Learning studio-webservice kunnen worden opgegeven. In dit scenario kunt u parameters voor een webservice die overeenkomen met doorgeven aan de eigenschappen van de reader/writer-modules.
>
>

We bekijken een scenario voor het gebruik van parameters van de Web service. U hebt een geïmplementeerde Azure Machine Learning-webservice die gebruikmaakt van een module reader gegevens lezen uit een van de gegevensbronnen ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL Database). Nadat de batchuitvoering is uitgevoerd, kan de resultaten worden geschreven met behulp van een schrijfmodule (Azure SQL Database).  Er zijn geen web service invoer en uitvoer worden gedefinieerd in de experimenten. In dit geval is het raadzaam dat u de relevante webserviceparameters voor lees- en schrijftoegang modules configureert. Deze configuratie kan de reader/writer modules worden geconfigureerd wanneer u de activiteit AzureMLBatchExecution. U webserviceparameters in de **globalParameters** sectie in de activiteits-JSON als volgt.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

U kunt ook [Data Factory-functies](data-factory-functions-variables.md) in het doorgeven van waarden voor de Web service-parameters zoals weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> De Web service-parameters zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Met behulp van een module Reader gegevens lezen uit meerdere bestanden in Azure Blob
BIG data-pijplijnen met activiteiten zoals Pig en Hive kan produceren van een of meer uitvoerbestanden met er worden geen extensies. Bijvoorbeeld, wanneer u een externe Hive-tabel opgeeft, kan de gegevens voor de externe Hive-tabel kunnen worden opgeslagen in Azure blob-opslag met de volgende naam 000000_0. U kunt de reader-module gebruiken in een experiment om meerdere bestanden te lezen, en ze gebruiken voor voorspellingen.

Wanneer u de reader-module in een Azure Machine Learning-experiment, kunt u Azure Blob als invoer. De bestanden in de Azure blob-opslag kunnen worden de uitvoerbestanden (voorbeeld: 000000_0) die worden geproduceerd door een Pig en Hive-script uitvoeren in HDInsight. De reader-module kunt u lezen-bestanden (met er worden geen extensies) door het configureren van de **pad naar de container, map/blob**. De **pad naar de container** verwijst naar de container en **directory/blob** verwijst naar de map waarin de bestanden, zoals wordt weergegeven in de volgende afbeelding. Het sterretje dat wil zeggen, \*) **geeft aan dat alle bestanden in de container of de map (dat wil zeggen gegevens/aggregateddata/jaar 2014/maand-6 = /\*)** worden gelezen als onderdeel van het experiment.

![Azure Blob-eigenschappen](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Voorbeeld
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pijplijn met AzureMLBatchExecution activiteit met de Parameters van de Web Service

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure Machine Learning studio model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

In de bovenstaande JSON-voorbeeld:

* De geïmplementeerde Azure Machine Learning-webservice gebruikt een lezer en een schrijfmodule voor lezen/schrijven van gegevens van/naar een Azure SQL Database. Deze webservice wordt aangegeven dat de volgende vier parameters:  Database-servernaam, databasenaam, gebruikersnaam voor account Server en Server het wachtwoord voor gebruikersaccount.
* Beide **start** en **end** datum/tijd moet zich in [ISO-indeling](https://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. De **end** is optioneel. Als u geen waarde voor de **end** eigenschap, wordt berekend als '**start + 48 uur.**" Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**. Zie de [naslaginformatie voor JSON-scriptverwerking](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de JSON-eigenschappen.

### <a name="other-scenarios"></a>Andere scenario's
#### <a name="web-service-requires-multiple-inputs"></a>Web-service vereist meerdere invoergegevens
Als de webservice meerdere invoergegevens heeft, gebruikt u de **webServiceInputs** in plaats van de eigenschap **webServiceInput**. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moet ook zijn opgenomen in de activiteit **invoer**.

In de Azure Machine Learning studio-experiment hebben web service invoer en uitvoerpoorten en globale parameters standaardnamen ('input1', 'input2') die u kunt aanpassen. De namen die u voor het webServiceInputs webServiceOutputs en globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de Help-pagina voor het eindpunt van de Azure Machine Learning studio om te controleren of de verwachte toewijzing.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Web-Service vereist geen invoer
Azure Machine Learning studio-batch tot uitvoering van web-services kunnen worden gebruikt om uit te voeren geen werkstromen, voor de voorbeeld-R of Python, scripts, die mogelijk niet alle invoer. Of het experiment kan worden geconfigureerd met een Reader-module die u maakt een GlobalParameters niet beschikbaar. In dat geval wordt zou de activiteit AzureMLBatchExecution als volgt worden geconfigureerd:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web-Service vereist geen invoer/uitvoer
De webservice van de uitvoering van Azure Machine Learning studio hebben mogelijk niet alle webservice-uitvoer geconfigureerd. In dit voorbeeld is er geen webservice invoer of uitvoer, noch een GlobalParameters zijn geconfigureerd. Er is nog steeds een uitvoer die is geconfigureerd op de activiteit, maar deze niet is opgegeven als een webServiceOutput.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web Service maakt gebruik van lezers en schrijvers en uitvoeringen van activiteit alleen wanneer andere activiteiten zijn geslaagd
De Azure Machine Learning studio web service lees- en schrijftoegang-modules kunnen worden geconfigureerd om te worden uitgevoerd met of zonder een GlobalParameters. U wilt echter serviceaanroepen insluiten in een pijplijn met behulp van gegevenssetafhankelijkheden om aan te roepen van de service alleen wanneer sommige upstream verwerking is voltooid. U kunt ook een andere actie activeren nadat de batch niet uitvoeren met behulp van deze benadering is voltooid. In dat geval kunt u de afhankelijkheden met behulp van invoer en uitvoer, zonder dat ze als webservice invoer of uitvoer naming express.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

De **takeaways** zijn:

* Als uw eindpunt experiment maakt gebruik van een webServiceInput: deze wordt vertegenwoordigd door een blob-gegevensset en is opgenomen in de invoer voor de activiteit en de webServiceInput-eigenschap. Anders wordt wordt de eigenschap webServiceInput weggelaten.
* Als uw eindpunt experiment maakt gebruik van webServiceOutput(s): ze worden vertegenwoordigd door de blob-gegevenssets en zijn opgenomen in de uitvoer voor activiteiten en in de eigenschap webServiceOutputs. De uitvoer van de activiteit en webServiceOutputs zijn toegewezen door de naam van elke uitvoer in het experiment. Anders wordt wordt de eigenschap webServiceOutputs weggelaten.
* Als uw experiment-eindpunt wordt aangegeven dat globalParameter(s), wordt verstrekt in de eigenschap globalParameters van activiteit als sleutel-waardeparen. Anders wordt wordt de eigenschap globalParameters weggelaten. De sleutels zijn hoofdlettergevoelig. [Azure Data Factory-functies](data-factory-functions-variables.md) kan worden gebruikt in de waarden.
* Extra gegevenssets kan worden opgenomen in de eigenschappen van de invoer en uitvoer voor activiteit, zonder waarnaar wordt verwezen in de activiteit typeProperties. Deze gegevenssets worden uitgevoerd met behulp van segment afhankelijkheden te beheren, maar anders worden genegeerd door de activiteit AzureMLBatchExecution.


## <a name="updating-models-using-update-resource-activity"></a>Bijwerken van modellen met behulp van de activiteit resources bijwerken
Als u klaar bent met het opnieuw trainen, bijwerken van de webservice scoring (Voorspellend experiment weergegeven als een webservice) met het zojuist getrainde model met behulp van de **Azure Machine Learning studio activiteit resources bijwerken**. Zie [bijwerken van modellen met behulp van de activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md) artikel voor meer informatie.

### <a name="reader-and-writer-modules"></a>Lezer en schrijver Modules
Een veelvoorkomend scenario voor het gebruik van webserviceparameters is het gebruik van Azure SQL-lezers en schrijvers. De reader-module wordt gebruikt om gegevens te laden in een experiment van data management-services buiten Azure Machine Learning Studio. De module writer is op te slaan van de gegevens van uw experimenten in gegevensbeheerservices buiten Azure Machine Learning Studio.

Zie voor meer informatie over Azure-Blob/Azure SQL-reader/writer [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [schrijver](https://msdn.microsoft.com/library/azure/dn905984.aspx) -onderwerpen op MSDN-bibliotheek. Het voorbeeld in de vorige sectie gebruikt de Azure Blob-lezer en Azure Blob-schrijver. In deze sectie beschrijft het gebruik van Azure SQL-reader en Azure SQL writer.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**V:** Ik heb meerdere bestanden die worden gegenereerd door mijn big data-pijplijnen. Kan ik de AzureMLBatchExecution-activiteit gebruiken om te werken op alle bestanden?

**A:** Ja. Zie de **met behulp van een module Reader gegevens lezen uit meerdere bestanden in Azure Blob** sectie voor meer informatie.

## <a name="azure-machine-learning-studio-batch-scoring-activity"></a>Azure Machine Learning studio Batch Scoring-activiteit
Als u de **AzureMLBatchScoring** activiteit om te integreren met Azure Machine Learning, raden wij aan dat u de meest recente **AzureMLBatchExecution** activiteit.

De activiteit AzureMLBatchExecution is geïntroduceerd in de augustus 2015-release van Azure SDK en Azure PowerShell.

Als u doorgaan met de activiteit AzureMLBatchScoring wilt, blijven lezen via deze sectie.

### <a name="azure-machine-learning-studio-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure Machine Learning studio Batch Scoring activity in Azure Storage voor invoer/uitvoer

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Webserviceparameters
Waarden voor de Web serviceparameters wilt opgeven, Voeg een **typeProperties** sectie aan de **AzureMLBatchScoringActivity** sectie in de pijplijn-JSON, zoals wordt weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
U kunt ook [Data Factory-functies](data-factory-functions-variables.md) in het doorgeven van waarden voor de Web service-parameters zoals weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> De Web service-parameters zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>
>

## <a name="see-also"></a>Zie ook
* [Azure-blog-bericht: Aan de slag met Azure Data Factory en Azure Machine Learning](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: https://azure.microsoft.com/services/machine-learning/
