---
title: Bijwerken van machine learning-modellen met behulp van Azure Data Factory | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen maken met Azure Data Factory en machine learning
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: f6d5f2d7df483e0884779c3eac6a77f976e173c3
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567029"
---
# <a name="update-azure-machine-learning-models-by-using-update-resource-activity"></a>Bijwerken van Azure Machine Learning-modellen met behulp van de activiteit resources bijwerken
In dit artikel is een aanvulling op de belangrijkste versie van Azure Data Factory - artikel voor Azure Machine Learning-integratie: [Voorspellende pijplijnen maken met Azure Machine Learning en Azure Data Factory](transform-data-using-machine-learning.md). Als u dit nog niet hebt gedaan, lees dan het belangrijkste artikel voordat u lezen in dit artikel.

## <a name="overview"></a>Overzicht
Uw model is getraind en opgeslagen als onderdeel van het proces voor het tot het operationaliseren van Azure Machine Learning-modellen. U vervolgens worden gebruikt om een voorspellende webservice te maken. De webservice kan vervolgens worden gebruikt in web sites, dashboards en mobiele apps.

U maakt gebruik van Machine Learning modellen zijn doorgaans niet statisch. Zodra er nieuwe gegevens beschikbaar is of wanneer de gebruiker van de API hun eigen gegevens is moet het model opnieuw worden getraind. Raadpleeg [opnieuw trainen van een Machine Learning-Model](../machine-learning/machine-learning-retrain-machine-learning-model.md) voor meer informatie over hoe u een model in Azure Machine Learning opnieuw kunt trainen.

Opnieuw trainen kan vaak optreden. U kunt de Azure Machine Learning-model opnieuw trainen en het bijwerken van de voorspellende webservice met behulp van Data Factory kunt uitvoeren met Batch Execution-activiteit en activiteit resources bijwerken.

De volgende afbeelding ziet u de relatie tussen de training en voorspellende webservices.

![Webservices](./media/update-machine-learning-models/web-services.png)

## <a name="azure-machine-learning-update-resource-activity"></a>Azure Machine Learning update resource-activiteit

De volgende JSON-fragment definieert een Azure Machine Learning Batch Execution-activiteit.

```json
{
    "name": "amlUpdateResource",
    "type": "AzureMLUpdateResource",
    "description": "description",
    "linkedServiceName": {
        "type": "LinkedServiceReference",
        "referenceName": "updatableScoringEndpoint2"
    },
    "typeProperties": {
        "trainedModelName": "ModelName",
        "trainedModelLinkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "StorageLinkedService"
                },
        "trainedModelFilePath": "ilearner file path"
    }
}
```

| Eigenschap                      | Description                              | Vereist |
| :---------------------------- | :--------------------------------------- | :------- |
| naam                          | Naam van de activiteit in de pijplijn     | Ja      |
| description                   | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                          | Voor Azure Machine Learning Update Resource-activiteit, het activiteitstype is **AzureMLUpdateResource**. | Ja      |
| linkedServiceName             | Azure Machine Learning gekoppelde service met de eigenschap updateResourceEndpoint. | Ja      |
| trainedModelName              | Naam van de Trained Model-module in het webservice-experiment worden bijgewerkt | Ja      |
| trainedModelLinkedServiceName | De naam van gekoppelde Azure Storage-service beschikt over het ilearner-bestand dat is geüpload door de updatebewerking | Ja      |
| trainedModelFilePath          | Het relatieve bestandspad in trainedModelLinkedService om weer te geven van het ilearner-bestand dat is geüpload door de updatebewerking | Ja      |

## <a name="end-to-end-workflow"></a>End-to-end werkstroom

Het hele proces van tot het operationaliseren van een model en update de voorspellende webservices retraining omvat de volgende stappen uit:

- Aanroepen van de **training webservice** met behulp van de **Batch Execution-activiteit**. Aanroepen van een training-webservice is hetzelfde als het aanroepen van een voorspellende webservice wordt beschreven in [voorspellende pijplijnen maken met Azure Machine Learning en Data Factory Batch Execution-activiteit](transform-data-using-machine-learning.md). De uitvoer van de training-webservice is een iLearner-bestand dat u gebruiken kunt om bij te werken van de voorspellende webservice.
- Aanroepen van de **-resource-eindpunt bijwerken** van de **voorspellende webservice** met behulp van de **activiteit resources bijwerken** het bijwerken van de webservice met het zojuist getrainde model.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service

Voor de hierboven genoemde end-to-end werkstroom om te werken, moet u twee gekoppelde van Azure Machine Learning-services maken:

1. Een Azure Machine Learning-gekoppelde service met de webservice voor de training, deze gekoppelde service wordt gebruikt door de Batch Execution-activiteit op dezelfde manier als wat wordt genoemd in [voorspellende pijplijnen maken met Azure Machine Learning en Data Factory-Batch Uitvoering van activiteit](transform-data-using-machine-learning.md). Verschil is dat de uitvoer van de webservice training is een iLearner-bestand dat vervolgens wordt gebruikt door activiteit resources bijwerken voor het bijwerken van de voorspellende webservice.
2. Een Azure Machine Learning gekoppelde service naar het resource-eindpunt van de update van de voorspellende webservice. Deze gekoppelde service wordt gebruikt door de activiteit resources bijwerken om bij te werken van de voorspellende webservice met behulp van het iLearner-bestand dat is geretourneerd in het navolgende stap.

Voor de tweede service van Azure Machine Learning is gekoppeld is de configuratie van de verschillende wanneer uw Azure Machine Learning-webservice een klassieke webservice of een nieuwe webservice wordt. De verschillen worden afzonderlijk in de volgende secties worden besproken.

## <a name="web-service-is-new-azure-resource-manager-web-service"></a>Web-service is een nieuwe Azure Resource Manager-webservice

Als de webservice voor het nieuwe type van de webservice die wordt aangegeven dat een Azure Resource Manager-eindpunt is, hoeft u niet om toe te voegen van de tweede **niet-standaard** eindpunt. De **updateResourceEndpoint** is in de gekoppelde service van de indeling:

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview
```

U kunt waarden ophalen voor de tijdelijke aanduidingen in de URL bij het opvragen van de webservice op de [Azure Machine Learning Web Services-Portal](https://services.azureml.net/).

Het nieuwe type update resource-eindpunt vereist verificatie van service-principal. Service-principal verificatie gebruiken, de Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) en het geven de **Inzender** of **eigenaar** rol van het abonnement of de groep waar de webservice behoort. De Zie [service-principal maken en toewijzen van machtigingen voor het beheren van Azure-resource](../active-directory/develop/howto-create-service-principal-portal.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Toepassingssleutel
- Tenant-id

Hier volgt een voorbeelddefinitie voor gekoppelde service:

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000  000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": {
                "type": "SecureString",
                "value": "APIKeyOfEndpoint1"
            },
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "servicePrincipalKey"
            },
            "tenant": "mycompany.com"
        }
    }
}
```

De volgende scenario biedt meer informatie. Er is een voorbeeld voor het opnieuw trainen en het bijwerken van Azure Machine Learning studio-modellen uit een Azure Data Factory-pijplijn.


## <a name="sample-retraining-and-updating-an-azure-machine-learning-model"></a>Voorbeeld: Opnieuw trainen en het bijwerken van een Azure Machine Learning-model

Deze sectie bevat een voorbeeldpijplijn met behulp van de **Azure Machine Learning studio Batch Execution-activiteit** een model te trainen. De pijplijn gebruikt ook de **Azure Machine Learning studio activiteit resources bijwerken** om bij te werken het model in de scoring-webservice. Het bevat ook JSON-fragmenten voor alle gekoppelde services, gegevenssets en pijplijn in het voorbeeld.

### <a name="azure-blob-storage-linked-service"></a>Gekoppelde Azure Blob storage-service:
Azure Storage bevat de volgende gegevens:

* trainingsgegevens. De ingevoerde gegevens voor de Azure Machine Learning studio-webservice training.
* iLearner-bestand. De uitvoer van de Azure Machine Learning studio-webservice training. Dit bestand is ook de invoer voor de activiteit resources bijwerken.

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

### <a name="linked-service-for-azure-machine-learning-studio-training-endpoint"></a>Gekoppelde service voor Azure Machine Learning studio training-eindpunt
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

In **Azure Machine Learning studio**, doet u het volgende waarden voor ophalen **mlEndpoint** en **apiKey**:

1. Klik op **WEBSERVICES** in het menu links.
2. Klik op de **training webservice** in de lijst van webservices.
3. Klik op kopiëren naast **API-sleutel** in het tekstvak. Plak de sleutel in het Klembord in de JSON van Data Factory-editor.
4. In de **Azure Machine Learning studio**, klikt u op **BATCHUITVOERING** koppeling.
5. Kopieer de **aanvraag-URI** uit de **aanvragen** sectie en plak deze in de JSON van Data Factory-editor.

### <a name="linked-service-for-azure-machine-learning-studio-updatable-scoring-endpoint"></a>Gekoppelde service voor Azure Machine Learning studio bij te werken scoring-eindpunt:
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het bij te werken eindpunt van de scoring-webservice verwijst.

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

### <a name="pipeline"></a>Pijplijn
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De Batch Execution-activiteit neemt de trainingsgegevens als invoer en produceert een iLearner-bestand als uitvoer. De activiteit resources bijwerken vervolgens duurt dit iLearner-bestand en de voorspellende webservice bijwerkt.

```JSON
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "amlBEGetilearner",
                "description": "Use AML BES to get the ileaner file from training web service",
                "type": "AzureMLBatchExecution",
                "linkedServiceName": {
                    "referenceName": "trainingEndpoint",
                    "type": "LinkedServiceReference"
                },
                "typeProperties": {
                    "webServiceInputs": {
                        "input1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/input"
                        },
                        "input2": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/input"
                        }
                    },
                    "webServiceOutputs": {
                        "output1": {
                            "LinkedServiceName":{
                                "referenceName": "StorageLinkedService",
                                "type": "LinkedServiceReference"
                            },
                            "FilePath":"azuremltesting/output"
                        }
                    }
                }
            },
            {
                "name": "amlUpdateResource",
                "type": "AzureMLUpdateResource",
                "description": "Use AML Update Resource to update the predict web service",
                "linkedServiceName": {
                    "type": "LinkedServiceReference",
                    "referenceName": "updatableScoringEndpoint2"
                },
                "typeProperties": {
                    "trainedModelName": "ADFV2Sample Model [trained model]",
                    "trainedModelLinkedServiceName": {
                        "type": "LinkedServiceReference",
                        "referenceName": "StorageLinkedService"
                    },
                    "trainedModelFilePath": "azuremltesting/output/newModelForArm.ilearner"
                },
                "dependsOn": [
                    {
                        "activity": "amlbeGetilearner",
                        "dependencyConditions": [ "Succeeded" ]
                    }
                ]
            }
        ]
    }
}
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
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
