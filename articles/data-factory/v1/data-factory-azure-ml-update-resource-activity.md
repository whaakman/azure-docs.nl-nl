---
title: Bijwerken van Machine Learning-modellen met behulp van Azure Data Factory | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen maken met Azure Data Factory en Azure Machine Learning
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 7a5c0866bc08c5a73888d9baca41980106a62ae2
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954930"
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Bijwerken van Azure Machine Learning-modellen met behulp van de activiteit resources bijwerken

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


> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [bijwerken van machine learning-modellen in Data Factory](../update-machine-learning-models.md).

In dit artikel is een aanvulling op de belangrijkste versie van Azure Data Factory - artikel voor Azure Machine Learning-integratie: [voorspellende pijplijnen maken met Azure Machine Learning en Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md). Als u dit nog niet hebt gedaan, lees dan het belangrijkste artikel voordat u lezen in dit artikel. 

## <a name="overview"></a>Overzicht
De voorspellende modellen in de Azure ML experimenten scoren moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. Als u klaar bent met het opnieuw trainen, die u wilt de scoringwebservice bijwerken met het retrained ML-model. De gebruikelijke stappen voor het inschakelen van Azure ML-modellen voor retraining en bij te werken via webservices zijn:

1. Maken van een experiment in [Azure ML Studio](https://studio.azureml.net).
2. Wanneer u tevreden met het model bent, Azure ML Studio gebruiken voor het publiceren van webservices voor zowel de **trainingsexperiment** en scoren /**Voorspellend experiment**.

De volgende tabel beschrijft de webservices die in dit voorbeeld gebruikt.  Zie [opnieuw trainen Machine Learning-modellen programmatisch](../../machine-learning/machine-learning-retrain-models-programmatically.md) voor meer informatie.

- **Training-webservice** - trainingsgegevens ontvangt en produceert getrainde modellen. De uitvoer van de retraining is een .ilearner-bestand in een Azure Blob-opslag. De **standaard eindpunt** wordt automatisch gemaakt voor u wanneer u de training publiceert als een webservice experimenteren. U kunt meer eindpunten maken maar het voorbeeld wordt alleen het standaardeindpunt.
- **Scoring-webservice** - voorbeelden van niet-gelabelde gegevens ontvangt en Hiermee worden voorspellingen gedaan. De uitvoer van de voorspelling kan verschillende vormen hebben, zoals een CSV-bestand of de rijen in een Azure SQL-database, afhankelijk van de configuratie van het experiment hebben. Het standaardeindpunt is automatisch voor u gemaakt wanneer u de Voorspellend experiment als een webservice publiceren. 

De volgende afbeelding ziet u de relatie tussen training en scoring-eindpunten in Azure ML.

![Webservices](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

U kunt aanroepen de **training webservice** met behulp van de **Azure ML-Batchuitvoeringsactiviteit**. Aanroepen van een webservice training is hetzelfde als het aanroepen van een Azure ML webservice (webservice scoren) voor scoring gegevens. De voorgaande secties beschreven hoe u een Azure ML webservice van een Azure Data Factory-pijplijn in detail aanroepen. 

U kunt aanroepen de **scoring-webservice** met behulp van de **Resourceactiviteit voor Azure ML bijwerken** het bijwerken van de webservice met het zojuist getrainde model. De volgende voorbeelden ziet u de definities van de gekoppelde service: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Scoring-webservice is een klassieke webservice
Als de scoring-webservice is een **klassieke webservice**, maken van de tweede **niet-standaard en bij te werken eindpunt** met behulp van de Azure-portal. Zie [eindpunten maken](../../machine-learning/machine-learning-create-endpoint.md) artikel voor stappen. Nadat u het bij te werken niet-standaard-eindpunt hebt gemaakt, voert u de volgende stappen uit:

* Klik op **BATCHUITVOERING** om op te halen van de URI-waarde voor de **mlEndpoint** JSON-eigenschap.
* Klik op **resources bijwerken** koppeling naar de URI-waarde voor de **updateResourceEndpoint** JSON-eigenschap. De API-sleutel is op de pagina met endpoint zelf (in de rechterbenedenhoek).

![eindpunt bij te werken](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

Het volgende voorbeeld biedt een voorbeeld-JSON-definitie voor de AzureML gekoppelde service. De gekoppelde service maakt gebruik van de apiKey voor verificatie.  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Scoring-webservice is Azure Resource Manager-webservice 
Als de webservice voor het nieuwe type van de webservice die wordt aangegeven dat een Azure Resource Manager-eindpunt is, hoeft u niet om toe te voegen van de tweede **niet-standaard** eindpunt. De **updateResourceEndpoint** is in de gekoppelde service van de indeling: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

U kunt waarden ophalen voor de tijdelijke aanduidingen in de URL bij het opvragen van de webservice op de [Azure Machine Learning Web Services-Portal](https://services.azureml.net/). Het nieuwe type update resource-eindpunt vereist een token van AAD (Azure Active Directory). Geef **servicePrincipalId** en **servicePrincipalKey**in AzureML gekoppelde service. Zie [service-principal maken en toewijzen van machtigingen voor het beheren van Azure-resource](../../active-directory/develop/howto-create-service-principal-portal.md). Hier volgt een voorbeeld van een definitie van AzureML gekoppelde service: 

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

De volgende scenario biedt meer informatie. Er is een voorbeeld voor het opnieuw trainen en het bijwerken van Azure ML-modellen uit een Azure Data Factory-pijplijn.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenario: opnieuw trainen en het bijwerken van een Azure ML-model
Deze sectie bevat een voorbeeldpijplijn met behulp van de **Azure ML-Batch Execution-activiteit** een model te trainen. De pijplijn gebruikt ook de **Azure ML bijwerken Resource activity** om bij te werken het model in de scoring-webservice. Het bevat ook JSON-fragmenten voor alle gekoppelde services, gegevenssets en pijplijn in het voorbeeld.

Hier volgt de diagramweergave van de voorbeeldpijplijn. Zoals u ziet, wordt de Azure ML-Batchuitvoeringsactiviteit de training-invoer en produceert een training uitvoer (iLearner-bestand). De Resourceactiviteit voor Azure ML bijwerken wordt de uitvoer van deze training en updates van het model in het scoring web service-eindpunt. De Update Resourceactiviteit geen uitvoer wordt geproduceerd. De placeholderBlob is alleen een dummy uitvoergegevensset die door de Azure Data Factory-service is vereist voor het uitvoeren van de pijplijn.

![overzichtsdiagram van gegevenspijplijn](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Gekoppelde Azure Blob storage-service:
Azure Storage bevat de volgende gegevens:

* trainingsgegevens. De ingevoerde gegevens voor de webservice van de Azure ML-training.  
* iLearner-bestand. De uitvoer van de webservice van de Azure ML-training. Dit bestand is ook de invoer voor de activiteit resources bijwerken.  

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

### <a name="training-input-dataset"></a>De invoergegevensset training:
De volgende gegevensset vertegenwoordigt de invoer trainingsgegevens voor de webservice van de Azure ML-training. De Azure ML-Batch Execution-activiteit wordt deze gegevensset gebruikt als invoer.

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

### <a name="training-output-dataset"></a>De uitvoergegevensset training:
De volgende gegevensset vertegenwoordigt het iLearner-bestand voor uitvoer van de webservice van de Azure ML-training. De Azure ML-Batchuitvoeringsactiviteit produceert deze gegevensset. Deze gegevensset is ook de invoer voor de resources van Azure ML bijwerken-activiteit.

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

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Gekoppelde service voor Azure ML-eindpunt voor training
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het eindpunt van de webservice training verwijst.

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

In **Azure ML Studio**, doet u het volgende waarden voor ophalen **mlEndpoint** en **apiKey**:

1. Klik op **WEBSERVICES** in het menu links.
2. Klik op de **training webservice** in de lijst van webservices.
3. Klik op kopiëren naast **API-sleutel** in het tekstvak. Plak de sleutel in het Klembord in de JSON van Data Factory-editor.
4. In de **Azure ML studio**, klikt u op **BATCHUITVOERING** koppeling.
5. Kopieer de **aanvraag-URI** uit de **aanvragen** sectie en plak deze in de JSON van Data Factory-editor.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Gekoppelde Service voor Azure ML-eindpunt voor bijwerkbare scoring:
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het bij te werken niet-standaard-eindpunt van de scoring-webservice verwijst.  

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

### <a name="placeholder-output-dataset"></a>Tijdelijke aanduiding uitvoergegevensset:
De activiteit resources bijwerken van Azure ML is geen uitvoer gegenereerd. Azure Data Factory vereist echter een uitvoergegevensset om de planning van een pijplijn te stimuleren. Daarom gebruiken we een dummy/tijdelijke aanduiding gegevensset in dit voorbeeld.  

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
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De Azure ML-Batch Execution-activiteit neemt de trainingsgegevens als invoer en produceert een iLearner-bestand als uitvoer. De activiteit roept de training webservice (trainingsexperiment weergegeven als een webservice) met de invoer trainingsgegevens en ontvangt het ilearner-bestand van de webservice. De placeholderBlob is alleen een dummy uitvoergegevensset die door de Azure Data Factory-service is vereist voor het uitvoeren van de pijplijn.

![overzichtsdiagram van gegevenspijplijn](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

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
