---
title: Maak voorspellende gegevenspijplijnen met behulp van Azure Data Factory | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen met behulp van Azure Data Factory en Azure Machine Learning maken
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 05ae7cdc78e909c9aaa2b690d03eff8da09b6242
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Maak voorspellende pijplijnen met behulp van Azure Machine Learning en Azure Data Factory

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
> * [Aangepaste activiteit .NET](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Inleiding
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van machine learning in Gegevensfactory versie 2](../transform-data-using-machine-learning.md).


### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt u bouwen, testen en implementeren van predictive analytics-oplossingen. Het is voltooid uit op hoog niveau oogpunt in drie stappen:

1. **Maken van een trainingsexperiment**. U kunt deze stap doen met behulp van de Azure ML Studio. De ML studio is een gezamenlijke visual ontwikkelingsomgeving waarmee u trainen en te testen van een predictive analytics-model met trainingsgegevens.
2. **Converteren naar een Voorspellend experiment**. Nadat uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het beoordelen van nieuwe gegevens, kunt u voorbereiden en uw experiment voor score berekenen stroomlijnen.
3. **Als een webservice implementeren**. U kunt uw scoreprofiel experiment publiceren als een Azure-web-service. U kunt gegevens verzenden naar uw model via deze web service-eindpunt en ontvangen resultaat voorspellingen van het model.  

### <a name="azure-data-factory"></a>Azure Data Factory
Een Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee de **verplaatsing** en **transformatie** van gegevens wordt beheerd en geautomatiseerd. U kunt gegevens integratieoplossingen maken gebruik van Azure Data Factory kunnen opnemen van gegevens uit verschillende gegevensarchieven, transformatieproces de gegevens en de result-gegevens publiceren naar de gegevensarchieven.

Met Data Factory-service kunt u gegevenspijplijnen maken die gegevens verplaatsen en transformeren, en de pijplijnen vervolgens uitvoeren volgens een opgegeven schema (per uur, dagelijks, wekelijks enz.). U vindt uitgebreide visualisaties om de afkomst en afhankelijkheden tussen uw gegevenspijplijnen weer te geven en al uw gegevenspijplijnen te controleren vanuit één centrale weergave zodat u eenvoudig problemen kunt detecteren en bewakingswaarschuwingen kunt instellen.

Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) en [bouwen van uw eerste pijplijn](data-factory-build-your-first-pipeline.md) artikelen die u kunt snel aan de slag met de Azure Data Factory-service.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory en Machine Learning samen
Azure Data Factory kunt u gemakkelijk maken pijplijnen die gebruikmaken van een gepubliceerde [Azure Machine Learning] [ azure-machine-learning] voor predictive analytics-webservice. Met behulp van de **Batchuitvoeringsactiviteit** in een Azure Data Factory-pijplijn, kunt u een webservice Azure ML zodat voorspellingen op de gegevens in batch aanroept. Zie [aanroepen van een Azure ML-webservice met behulp van de Batchuitvoeringsactiviteit](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) sectie voor meer informatie.

Na verloop van tijd moeten de voorspellende modellen in de Azure ML experimenten score berekenen met behulp van de nieuwe invoergegevenssets worden retrained. U kunt een Azure ML-model van een Data Factory-pijplijn retrain als volgt u de volgende stappen uit:

1. Publiceer het trainingsexperiment (geen Voorspellend experiment) als een webservice. Deze stap in de Azure ML Studio doet u als hiervoor toen u Voorspellend experiment weergeven als een webservice in het voorgaande scenario.
2. Gebruik de Azure ML-Batchuitvoeringsactiviteit aanroepen van de webservice voor de trainingsexperiment. In principe kunt u de Azure ML-Batchuitvoering activiteit webservice training zowel scoreprofiel webservice aanroepen.

Nadat u klaar bent met de retraining, de scoreprofiel webservice bijwerken (Voorspellend experiment weergegeven als een webservice) met het nieuwe getrainde model met behulp van de **Azure ML-Resourceactiviteit**. Zie [bijwerken met behulp van de Update Resourceactiviteit modellen](data-factory-azure-ml-update-resource-activity.md) artikel voor meer informatie.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Batchuitvoeringsactiviteit met een webservice wordt aangeroepen
U Azure Data Factory gebruiken voor het indelen van de verplaatsing van gegevens en verwerking en vervolgens met behulp van Azure Machine Learning-Batchuitvoering uit te voeren. Hier volgen de stappen op het hoogste niveau:

1. Maak een Azure Machine Learning gekoppelde service. U moet de volgende waarden:

   1. **URI van de aanvraag** voor de Batchuitvoering API. U kunt de aanvraag-URI vinden door te klikken op de **BATCHUITVOERING** koppeling op de webpagina van de services.
   2. **API-sleutel** voor de gepubliceerde Azure Machine Learning-webservice. U kunt de API-sleutel vinden door te klikken op de webservice die u hebt gepubliceerd.
   3. Gebruik de **AzureMLBatchExecution** activiteit.

      ![Machine Learning Dashboard](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario: Experimenten met behulp van Web service invoer/uitvoer die naar gegevens in Azure Blob-opslag verwijzen
In dit scenario wordt de Azure Machine Learning-webservice maakt voorspellingen met gegevens uit een bestand in een Azure blob storage en de resultaten van de voorspelling opslaat in de blobopslag. De volgende JSON definieert een Data Factory-pijplijn met een AzureMLBatchExecution-activiteit. De activiteit heeft de gegevensset **DecisionTreeInputBlob** als invoer en **DecisionTreeResultBlob** als uitvoer. De **DecisionTreeInputBlob** wordt doorgegeven als invoer voor de webservice door met de **webServiceInput** JSON-eigenschap. De **DecisionTreeResultBlob** wordt doorgegeven als uitvoer met de webservice door met de **webServiceOutputs** JSON-eigenschap.  

> [!IMPORTANT]
> Als de web-service meerdere invoer heeft, gebruikt u de **webServiceInputs** in plaats van de eigenschap **webServiceInput**. Zie de [webservice vereist meerdere invoeritems](#web-service-requires-multiple-inputs) sectie voor een voorbeeld van het gebruik van de eigenschap webServiceInputs.
>
> Gegevenssets waarnaar wordt verwezen door de **webServiceInput**/**webServiceInputs** en **webServiceOutputs** eigenschappen (in  **typeProperties**) moet ook zijn opgenomen in de activiteit **invoer** en **levert**.
>
> In uw experiment Azure ML web service invoer of uitvoerpoorten en globale parameters standaardnamen hebben ('input1', 'input2') die u kunt aanpassen. De namen die u voor webServiceInputs en webServiceOutputs globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de pagina Batch-uitvoering Help voor uw Azure ML-eindpunt om te controleren of de verwachte toewijzing.
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
> Alleen invoer en uitvoer van de activiteit AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In de bovenstaande JSON-codefragment is DecisionTreeInputBlob bijvoorbeeld invoer voor de activiteit AzureMLBatchExecution, die wordt doorgegeven als invoer voor de webservice via webServiceInput-parameter.   
>
>

### <a name="example"></a>Voorbeeld
Dit voorbeeld wordt een Azure Storage voor de invoer- en gegevens.

Het is raadzaam dat u doorloopt de [bouwen van uw eerste pijplijn met Data Factory] [ adf-build-1st-pipeline] zelfstudie voordat u verdergaat met het volgende voorbeeld. De Data Factory-Editor gebruiken voor het maken van Data Factory-artefacten (gekoppelde services, gegevenssets, pijplijn) in dit voorbeeld.   

1. Maak een **gekoppelde service** voor uw **Azure Storage**. Als de invoer- en -bestanden zich in verschillende storage-accounts, moet u twee gekoppelde services. Hier volgt een voorbeeld van JSON:

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
2. Maak de **invoer** Azure Data Factory **gegevensset**. In tegenstelling tot een aantal andere Data Factory-gegevenssets deze gegevenssets, beide moet bevatten **folderPath** en **fileName** waarden. U kunt partitioneren ervoor zorgen dat elke Batchuitvoering (elke gegevenssegment) om te verwerken of produceren unieke invoer en uitvoer van bestanden. U moet mogelijk zijn sommige upstream-activiteit voor het transformeren van de invoer naar de CSV-bestandsindeling en plaats deze in het opslagaccount voor elk segment. In dat geval u geen omvat de **externe** en **externalData** instellingen die worden weergegeven in het volgende voorbeeld en uw DecisionTreeInputBlob zou de uitvoergegevensbron van een andere activiteit.

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

    Uw invoer csv-bestand moet de veldnamenrij kolom hebben. Als u de **Kopieeractiviteit** als u wilt maken of verplaatsen de csv in de blobopslag, moet u de eigenschap sink instellen **blobWriterAddHeader** naar **true**. Bijvoorbeeld:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    Als het csv-bestand is geen de veldnamenrij, ziet u mogelijk de volgende fout: **fout in de activiteit: fout bij het lezen van de tekenreeks. Onverwacht token: StartObject. Pad '', regel 1, positie 1**.
3. Maak de **uitvoer** Azure Data Factory **gegevensset**. In dit voorbeeld gebruikt partitioneren voor een unieke uitvoerpad voor de uitvoering van elk segment maken. Zonder de partitionering, zou de activiteit bestand overschreven.

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
4. Maak een **gekoppelde service** van het type: **AzureMLLinkedService**, bieden de API-sleutel en model van de volgende URL.

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
5. Ten slotte schrijft een pijplijn met een **AzureMLBatchExecution** activiteit. Pijplijn voert tijdens runtime, de volgende stappen uit:

   1. Hiermee haalt u de locatie van het bestand voor invoer van uw invoer gegevenssets.
   2. De Batchuitvoering van Azure Machine Learning API aanroept
   3. De uitvoer van de batch-uitvoering gekopieerd naar de blob die is opgegeven in de uitvoergegevensset.

      > [!NOTE]
      > AzureMLBatchExecution activiteit kan hebben nul of meer invoer en uitvoer van een of meer.
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

      Beide **start** en **end** tijd moeten [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. De **end** tijd is optioneel. Als u geen waarde voor de **end** eigenschap, wordt berekend als '**start + 48 uur.**' Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**. Zie de [naslaginformatie voor JSON-scriptverwerking](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de JSON-eigenschappen.

      > [!NOTE]
      > Invoer voor de AzureMLBatchExecution opgeven is activiteit optioneel.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario: Experimenten lezer/schrijver-Modules gebruiken om te verwijzen naar gegevens in verschillende gelijksoortig
Bij het maken van Azure ML-experimenten een ander gebruikelijk scenario is het gebruik van de lezer en -schrijver modules. De module reader wordt gebruikt om gegevens te laden in een experiment en de module writer is gegevens van uw experimenten worden opgeslagen. Zie voor meer informatie over de lezer en -schrijver modules [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op MSDN-bibliotheek.     

Wanneer u de lezer en -schrijver modules, is het raadzaam om een Web service-parameter voor elke eigenschap van deze modules lezer/schrijver gebruiken. Deze web-parameters kunnen u voor het configureren van de waarden tijdens runtime. U kunt bijvoorbeeld een experiment maken met een leesmodule die gebruikmaakt van een Azure SQL Database: XXX.database.windows.net. Nadat de web-service is geïmplementeerd, die u wilt inschakelen van de consument van de web-service om op te geven van een Azure SQL-Server YYY.database.windows.net aangeroepen. Een parameter van Web service kunt u toestaan dat deze waarde moet worden geconfigureerd.

> [!NOTE]
> Web service invoer en uitvoer verschillen van de parameters van Web service. In het eerste scenario hebt u gezien hoe een invoer en uitvoer voor een webservice Azure ML kunnen worden opgegeven. In dit scenario kunt u parameters voor een webservice die overeenkomen met doorgeven aan de eigenschappen van de lezer/schrijver modules.
>
>

We bekijken een scenario voor het gebruik van parameters van Web service. U hebt een geïmplementeerde Azure Machine Learning-webservice die gebruikmaakt van een module reader gegevens lezen uit een van de gegevensbronnen die wordt ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL Database). Nadat de batchuitvoering is uitgevoerd, kan de resultaten zijn geschreven met behulp van een module Writer (Azure SQL Database).  Er is geen web-service in- en uitgangen zijn gedefinieerd in de experimenten. In dit geval is het raadzaam dat u de parameters van de relevante web service voor de lezer en -schrijver modules configureert. Deze configuratie kunt de reader/writer modules worden geconfigureerd wanneer u de activiteit AzureMLBatchExecution. U Web serviceparameters opgeven in de **globalParameters** sectie als volgt in de JSON van de activiteit.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

U kunt ook [Data Factory functies](data-factory-functions-variables.md) waarden doorgeven voor de Web service-parameters zoals weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Een module Reader gebruikt om te lezen van gegevens uit meerdere bestanden in Azure Blob
BIG data pijplijnen met activiteiten zoals Pig en Hive kunt produceren van een of meer uitvoerbestanden zonder extensie. Bijvoorbeeld, wanneer u een externe Hive-tabel opgeeft, kan de gegevens voor de externe Hive-tabel kunnen worden opgeslagen in Azure blob storage met de volgende naam 000000_0. U kunt de module reader gebruiken in een experiment meerdere bestanden lezen en deze worden gebruikt voor voorspellingen.

Wanneer u de module reader in een Azure Machine Learning-experiment, kunt u Azure Blob opgeven als invoer. De bestanden in Azure blob storage kunnen worden de uitvoerbestanden (voorbeeld: 000000_0) die worden geproduceerd door een Pig en Hive-script uitgevoerd op HDInsight. De module reader kunt u lezen-bestanden (met er worden geen extensies) door het configureren van de **pad naar de container, map/blob**. De **pad naar de container** verwijst naar de container en **directory/blob** verwijst naar de map waarin de bestanden, zoals wordt weergegeven in de volgende afbeelding. Het sterretje dat wil zeggen, \*) **geeft aan dat alle bestanden in de container of de map (dat wil zeggen, aggregateddata-gegevens/jaar = maand-2014-6 /\*)** als onderdeel van het experiment worden gelezen.

![Azure Blob-eigenschappen](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Voorbeeld
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Pijplijn met AzureMLBatchExecution activiteit met de Parameters van Web Service

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
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

* De geïmplementeerde Azure Machine Learning-webservice maakt gebruik van een lezer en een schrijfmodule tot lezen/schrijven van gegevens van/naar een Azure SQL Database. Deze webservice beschrijft de volgende vier parameters: servernaam, databasenaam, servergebruikersnaam en wachtwoord voor gebruikersaccount Server-Database.  
* Beide **start** en **end** tijd moeten [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41Z. De **end** tijd is optioneel. Als u geen waarde voor de **end** eigenschap, wordt berekend als '**start + 48 uur.**' Als u de pijplijn voor onbepaalde tijd wilt uitvoeren, geeft u **9999-09-09** op als waarde voor de eigenschap **end**. Zie de [naslaginformatie voor JSON-scriptverwerking](https://msdn.microsoft.com/library/dn835050.aspx) voor meer informatie over de JSON-eigenschappen.

### <a name="other-scenarios"></a>Andere scenario's
#### <a name="web-service-requires-multiple-inputs"></a>Meerdere invoer is vereist
Als de web-service meerdere invoer heeft, gebruikt u de **webServiceInputs** in plaats van de eigenschap **webServiceInput**. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moet ook zijn opgenomen in de activiteit **invoer**.

In uw experiment Azure ML web service invoer of uitvoerpoorten en globale parameters standaardnamen hebben ('input1', 'input2') die u kunt aanpassen. De namen die u voor webServiceInputs en webServiceOutputs globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de pagina Batch-uitvoering Help voor uw Azure ML-eindpunt om te controleren of de verwachte toewijzing.

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
Azure ML-batch uitvoering webservices kunnen worden gebruikt voor het uitvoeren van alle werkstromen, voor voorbeeld R- of Python-scripts die niet alle invoer nodig. Of het experiment kan worden geconfigureerd met een lezer-module die u maakt een GlobalParameters niet beschikbaar. In dat geval wordt de activiteit AzureMLBatchExecution zou als volgt geconfigureerd:

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
De webservice voor de uitvoering van Azure ML hebben mogelijk niet alle webservice-uitvoer geconfigureerd. Er is geen webservice invoer of uitvoer in dit voorbeeld, noch een GlobalParameters zijn geconfigureerd. Er is nog steeds een uitvoer die is geconfigureerd op de activiteit zelf, maar er is geen opgegeven als een webServiceOutput.

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

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web-Service gebruikt lezers en schrijvers en de activiteit wordt uitgevoerd, alleen wanneer andere activiteiten zijn geslaagd
De Azure ML web service lezer en -schrijver modules kunnen zijn geconfigureerd om te worden uitgevoerd met of zonder eventuele GlobalParameters. U kunt echter serviceaanroepen insluiten in een pijplijn die gebruikmaakt van gegevensset afhankelijkheden aanroepen van de service alleen wanneer sommige upstream verwerking is voltooid. Nadat de Batchuitvoering van de is voltooid met deze benadering kunt u ook een andere actie activeren. In dat geval kunt u de afhankelijkheden die met behulp van activiteit in- en uitgangen, zonder een van deze als webservice in- of uitgangen uitdrukken.

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

* Als uw experiment-eindpunt maakt gebruik van een webServiceInput: er wordt vertegenwoordigd door een blob-gegevensset en is opgenomen in de activiteitinvoer en de eigenschap webServiceInput. Anders wordt is de eigenschap webServiceInput weggelaten.
* Als uw experiment-eindpunt maakt gebruik van webServiceOutput(s): ze worden vertegenwoordigd door de blob-gegevenssets en zijn opgenomen in de uitvoer voor activiteiten en in de eigenschap webServiceOutputs. Uitvoer van de activiteit en webServiceOutputs zijn toegewezen door de naam van elke uitvoer in het experiment. Anders wordt is de eigenschap webServiceOutputs weggelaten.
* Als uw experiment eindpunt globalParameter(s) beschrijft, staan ze vermeld in de eigenschap globalParameters van activiteit als sleutel-waardeparen. Anders wordt is de eigenschap globalParameters weggelaten. De sleutels zijn hoofdlettergevoelig. [Azure Data Factory-functies](data-factory-functions-variables.md) kan worden gebruikt in de waarden.
* Aanvullende gegevenssets kan worden opgenomen in de eigenschappen van de invoer en uitvoer voor activiteiten, zonder waarnaar wordt verwezen in de activiteit typeProperties. Deze gegevenssets kan worden uitgevoerd segment afhankelijkheden reguleren maar anders worden genegeerd door de activiteit AzureMLBatchExecution.


## <a name="updating-models-using-update-resource-activity"></a>Bijwerken van modellen Update Resource activiteit
Nadat u klaar bent met de retraining, de scoreprofiel webservice bijwerken (Voorspellend experiment weergegeven als een webservice) met het nieuwe getrainde model met behulp van de **Azure ML-Resourceactiviteit**. Zie [bijwerken met behulp van de Update Resourceactiviteit modellen](data-factory-azure-ml-update-resource-activity.md) artikel voor meer informatie.

### <a name="reader-and-writer-modules"></a>Lezer en Writer Modules
Een veelvoorkomend scenario voor het gebruik van parameters voor Web-service is het gebruik van Azure SQL en schrijfprogramma. De module reader wordt gebruikt voor het laden van gegevens in een experiment van data management-services buiten Azure Machine Learning Studio. De module writer is het opslaan van gegevens vanuit uw experimenten in beheerservices gegevens buiten Azure Machine Learning Studio.  

Zie voor meer informatie over Azure-Blob/Azure SQL-lezer/schrijver [lezer](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op MSDN-bibliotheek. Het voorbeeld in de vorige sectie gebruikt de Azure Blob-lezer en Azure Blob-schrijver. Deze sectie beschrijft het gebruik van Azure SQL-reader en Azure SQL writer.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**V:** ik heb meerdere bestanden die worden gegenereerd door mijn pijplijnen big data. Kan ik de activiteit AzureMLBatchExecution gebruiken om te werken op alle bestanden?

**A:** Ja. Zie de **met behulp van een module Reader gegevens lezen uit meerdere bestanden in Azure Blob** sectie voor meer informatie.

## <a name="azure-ml-batch-scoring-activity"></a>Score berekenen voor Azure ML-Batch-activiteit
Als u de **AzureMLBatchScoring** activiteit integreren met Azure Machine Learning, het is raadzaam dat u de meest recente **AzureMLBatchExecution** activiteit.

De activiteit AzureMLBatchExecution wordt geïntroduceerd in de augustus 2015-release van Azure SDK en Azure PowerShell.

Als u wilt doorgaan met het gebruik van de activiteit AzureMLBatchScoring, blijven lezen via deze sectie.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure ML-Batchscoreberekening activiteit voor het gebruik van Azure Storage voor invoer/uitvoer

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

### <a name="web-service-parameters"></a>Parameters voor Web-Service
Waarden voor de Web serviceparameters wilt opgeven, Voeg een **typeProperties** sectie aan de **AzureMLBatchScoringActivty** sectie in de JSON-pijplijn, zoals wordt weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
U kunt ook [Data Factory functies](data-factory-functions-variables.md) waarden doorgeven voor de Web service-parameters zoals weergegeven in het volgende voorbeeld:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>
>

## <a name="see-also"></a>Zie ook
* [Azure blogbericht: aan de slag met Azure Data Factory en Azure Machine Learning](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
