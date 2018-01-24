---
title: Bijwerken van Machine Learning-modellen met behulp van Azure Data Factory | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen met behulp van Azure Data Factory en Azure Machine Learning maken
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 502bf8771bf7854755ccd72c7002110f1e25bd40
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Azure Machine Learning-modellen Update Resource activiteit bijwerken

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


> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [bijwerken van machine learning-modellen in de Data Factory versie 2](../update-machine-learning-models.md).

In dit artikel is een aanvulling op de belangrijkste Azure Data Factory - artikel voor Azure Machine Learning-integratie: [voorspellende pijplijnen met behulp van Azure Machine Learning en Azure Data Factory maken](data-factory-azure-ml-batch-execution-activity.md). Als u dit nog niet hebt gedaan, raadpleegt u de belangrijkste artikel voordat gelezen in dit artikel. 

## <a name="overview"></a>Overzicht
Na verloop van tijd moeten de voorspellende modellen in de Azure ML experimenten score berekenen met behulp van de nieuwe invoergegevenssets worden retrained. Nadat u klaar bent met de retraining, die u wilt de score webservice bijwerken met de retrained ML-model. De gebruikelijke stappen voor het inschakelen van Azure ML-modellen voor retraining en bij te werken via webservices zijn:

1. Maken van een experiment in [Azure ML Studio](https://studio.azureml.net).
2. Wanneer u tevreden met het model bent, Azure ML Studio gebruiken voor het publiceren van webservices voor zowel de **trainingsexperiment** en score berekenen /**Voorspellend experiment**.

De volgende tabel beschrijft de webservices die in dit voorbeeld gebruikt.  Zie [Retrain Machine Learning-modellen programmatisch](../../machine-learning/machine-learning-retrain-models-programmatically.md) voor meer informatie.

- **Training webservice** - trainingsgegevens ontvangt en getraind modellen produceert. De uitvoer van de retraining is een bestand .ilearner in een Azure-blobopslag. De **standaard eindpunt** wordt automatisch gemaakt voor u wanneer u de training publiceert als een webservice experimenteren. U kunt meer eindpunten maken, maar het voorbeeld wordt alleen het standaardeindpunt.
- **Score berekenen voor webservice** - voorbeelden zonder label gegevens ontvangt en voorspellingen maakt. De uitvoer van de voorspelling kan verschillende vormen, zoals een CSV-bestand of rijen in een Azure SQL database, afhankelijk van de configuratie van het experiment hebben. Het standaardeindpunt is automatisch voor u gemaakt wanneer u de Voorspellend experiment als een webservice publiceert. 

De volgende afbeelding ziet u de relatie tussen de trainings- en eindpunten in de Azure ML score.

![Webservices](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

U kunt aanroepen de **training webservice** met behulp van de **Azure ML-Batchuitvoeringsactiviteit**. Aanroepen van een webservice training is hetzelfde als het aanroepen van een Azure ML webservice (score berekenen voor web-service) voor scoreprofiel gegevens. De voorgaande secties uitgelegd hoe u een webservice Azure ML van een Azure Data Factory-pijplijn in detail worden aangeroepen. 

U kunt aanroepen de **score berekenen voor webservice** met behulp van de **Azure ML-Resourceactiviteit** de webservice bijwerken met het nieuwe getrainde model. De volgende voorbeelden bevatten definities van de gekoppelde service: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Score berekenen voor web-service is een klassieke webservice
Als de score webservice is een **klassieke webservice**, maken van de tweede **niet-standaard en bij te werken eindpunt** met behulp van de Azure-portal. Zie [eindpunten maken](../../machine-learning/machine-learning-create-endpoint.md) artikel voor stappen. Nadat u het niet-standaard worden bijgewerkt eindpunt hebt gemaakt, kunt u de volgende stappen uitvoeren:

* Klik op **BATCHUITVOERING** ophalen van de URI-waarde voor de **mlEndpoint** JSON-eigenschap.
* Klik op **UPDATE RESOURCE** koppeling voor de URI-waarde voor de **updateResourceEndpoint** JSON-eigenschap. De API-sleutel is op de pagina endpoint zelf (in de rechterbenedenhoek).

![eindpunt bij te werken](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Het volgende voorbeeld bevat een voorbeeld JSON-definitie voor de AzureML gekoppelde service. De gekoppelde service gebruikt de apiKey voor authenticatie.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Score berekenen voor web-service is Azure Resource Manager-webservice 
Als de webservice voor het nieuwe type-webservice dat toegang biedt tot een Azure Resource Manager-eindpunt is, hoeft u niet om toe te voegen van de tweede **niet-standaard** eindpunt. De **updateResourceEndpoint** is in de gekoppelde service van de indeling: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

U kunt waarden voor de houders plaats in de URL krijgen bij het opvragen van de web-service op de [Azure Machine Learning Web Services-Portal](https://services.azureml.net/). Het nieuwe type update resource eindpunt vereist een token van AAD (Azure Active Directory). Geef **servicePrincipalId** en **servicePrincipalKey**in voor met AzureML gekoppelde service. Zie [het service-principal maken en toewijzen van machtigingen voor het beheren van Azure-resource](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Hier volgt een voorbeeld van een definitie van AzureML gekoppelde service: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

Het volgende scenario biedt meer details. Er is een voorbeeld voor retraining en bijwerken van Azure ML-modellen van een Azure Data Factory-pijplijn.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenario: retraining en bijwerken van een Azure ML-model
Deze sectie vindt u een voorbeeldpijplijn die gebruikmaakt van de **Azure ML-Batchuitvoering activiteit** naar opnieuw een model te trainen. De pijplijn gebruikt ook de **Azure ML bijwerken Resource activiteit** het model in de scoreprofiel webservice bijwerken. De sectie bevat ook JSON codefragmenten voor alle gekoppelde services, gegevenssets en pijplijn in het voorbeeld.

Hier volgt de diagramweergave van de voorbeeldpijplijn. Zoals u ziet, wordt de Azure ML-Batchuitvoeringsactiviteit haalt de invoer training en wordt de uitvoer van een training (iLearner-bestand). De Resourceactiviteit voor Azure ML Update duurt de uitvoer van deze training en updates van het model in het scoreprofiel web service-eindpunt. De Update Resourceactiviteit produceert geen uitvoer. De placeholderBlob is een dummy output dataset is vereist door de Azure Data Factory-service voor het uitvoeren van de pijplijn.

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Azure Blob-opslag gekoppelde service:
Azure Storage bevat de volgende gegevens:

* trainingsgegevens. De invoergegevens voor de webservice Azure ML-training.  
* iLearner-bestand. De uitvoer van de webservice Azure ML-training. Dit bestand is ook de invoer voor de activiteit van de Update-Resource.  

Hier volgt de voorbeeld-JSON-definitie van de gekoppelde service:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Training invoergegevensset:
De volgende gegevensset vertegenwoordigt de invoer trainingsgegevens voor de webservice Azure ML-training. Deze gegevensset neemt de Azure ML-Batchuitvoering activiteit als invoer.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a>Uitvoergegevensset voor training:
De volgende gegevensset vertegenwoordigt het iLearner-bestand voor uitvoer van de webservice Azure ML-training. De Azure ML-Batchuitvoeringsactiviteit produceert deze dataset. Deze gegevensset is ook de invoer voor de Azure ML Update Resource-activiteit.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Gekoppelde service voor Azure ML training eindpunt
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het standaardeindpunt van de webservice training verwijst.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

In **Azure ML Studio**, doet u het volgende als u de waarden voor **mlEndpoint** en **apiKey**:

1. Klik op **WEBSERVICES** in het menu links.
2. Klik op de **training webservice** in de lijst met web-services.
3. Klik op kopiëren naast **API-sleutel** in het tekstvak. De sleutel op het Klembord in de JSON van Data Factory-editor plakken.
4. In de **Azure ML studio**, klikt u op **BATCHUITVOERING** koppeling.
5. Kopieer de **aanvraag-URI** van de **aanvragen** sectie en plak deze in de JSON van Data Factory-editor.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Gekoppelde Service voor Azure ML bijwerkbare score-eindpunt:
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het niet-standaard worden bijgewerkt eindpunt van de score-webservice verwijst.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Tijdelijke aanduiding voor uitvoergegevensset:
Resourceactiviteit voor Azure ML-Update wordt geen uitvoer gegenereerd. Azure Data Factory vereist echter een uitvoergegevensset stimuleren van de planning van een pijplijn. Daarom gebruiken we een dummy/tijdelijke aanduiding voor gegevensset in dit voorbeeld.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>Pijplijn
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De activiteit van de Azure ML-Batchuitvoering duurt de trainingsgegevens als invoer en een iLearner-bestand als uitvoer produceert. De activiteit roept de training webservice (trainingsexperiment weergegeven als een webservice) met de invoer trainingsgegevens en ontvangt van het ilearner-bestand van de webservice. De placeholderBlob is een dummy output dataset is vereist door de Azure Data Factory-service voor het uitvoeren van de pijplijn.

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
