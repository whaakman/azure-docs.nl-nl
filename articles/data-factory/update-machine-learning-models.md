---
title: Bijwerken van machine learning-modellen met behulp van Azure Data Factory | Microsoft Docs
description: Beschrijft hoe u voorspellende pijplijnen met behulp van Azure Data Factory maakt en machine learning
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shlo
ms.openlocfilehash: d4f7df5ceadf8c6219901af6e516453a88aeece7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="update-azure-machine-learning-models-by-using-update-resource-activity"></a>Azure Machine Learning-modellen worden bijgewerkt met behulp van de activiteit van de Update-Resource
In dit artikel is een aanvulling op de belangrijkste Azure Data Factory - artikel voor Azure Machine Learning-integratie: [voorspellende pijplijnen met behulp van Azure Machine Learning en Azure Data Factory maken](transform-data-using-machine-learning.md). Als u dit nog niet hebt gedaan, raadpleegt u de belangrijkste artikel voordat gelezen in dit artikel. 

## <a name="overview"></a>Overzicht
Uw model is getraind en opgeslagen als onderdeel van het proces van het operationele Azure Machine Learning-modellen. U vervolgens worden gebruikt voor het maken van een predicative webservice. De webservice kan vervolgens worden gebruikt in websites, dashboards en mobiele apps.

U maakt met behulp van Machine Learning modellen zijn meestal niet statisch. Zodra er nieuwe gegevens beschikbaar, of wanneer de gebruiker van de API hun eigen gegevens heeft moet het model worden retrained. Raadpleeg [opnieuw een Machine Learning-Model te trainen](../machine-learning/machine-learning-retrain-machine-learning-model.md) voor meer informatie over hoe u een model in Azure Machine Learning kunt retrain. 

Retraining kan vaak optreden. Met Batchuitvoering en activiteit van de Update-Resource, kunt u het model van Azure Machine Learning retraining en bijwerken van de voorspellende Web Service met behulp van de Data Factory operationeel. 

De volgende afbeelding ziet u de relatie tussen de training en voorspellende Web-Services. 

![Webservices](./media/update-machine-learning-models/web-services.png)

## <a name="azure-machine-learning-update-resource-activity"></a>Resourceactiviteit voor Azure Machine Learning-update 

De volgende JSON-fragment definieert een Batchuitvoering van Azure Machine Learning-activiteit.

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




| Eigenschap                      | Beschrijving                              | Vereist |
| :---------------------------- | :--------------------------------------- | :------- |
| naam                          | Naam van de activiteit in de pijplijn     | Ja      |
| description                   | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                          | Voor Azure Machine Learning Update Resource activiteit, het type hoofdactiviteit is **AzureMLUpdateResource**. | Ja      |
| linkedServiceName             | Azure Machine Learning gekoppelde service die de eigenschap updateResourceEndpoint bevat. | Ja      |
| trainedModelName              | Naam van de Trained Model-module in het webservice-experiment worden bijgewerkt | Ja      |
| trainedModelLinkedServiceName | De naam van gekoppelde Azure Storage-service van het ilearner-bestand dat door de updatebewerking is geüpload | Ja      |
| trainedModelFilePath          | Het relatieve bestandspad in trainedModelLinkedService vertegenwoordigt de ilearner-bestand dat door de updatebewerking is geüpload | Ja      |


## <a name="end-to-end-workflow"></a>End-to-end werkstroom

Het hele proces van het operationele retraining een model en update de voorspellende webservices omvat de volgende stappen: 

- Aanroepen de **training webservice** met behulp van de **Batchuitvoering activiteit**. Een training webservice wordt aangeroepen, is hetzelfde als het aanroepen van een Voorspellend webservice wordt beschreven in [voorspellende pijplijnen met behulp van Azure Machine Learning- en Data Factory-Batchuitvoering activiteit maken](transform-data-using-machine-learning.md). De uitvoer van de training webservice is een iLearner-bestand waarmee u kunt de voorspellende Web Service bijwerken. 
- Aanroepen de **resource eindpunt bijgewerkt** van de **voorspellende webservice** met behulp van de **bijwerken resourceactiviteit** de Web Service bijwerken met het nieuwe getrainde model. 

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service

Voor de bovengenoemde end-to-end werkstroom werken, moet u twee Azure Machine Learning gekoppelde services maken: 

1. Een Azure Machine Learning gekoppelde service met de webservice voor training, deze gekoppelde service wordt gebruikt door Batchuitvoering activiteit op dezelfde manier als wat wordt vermeld in [voorspellende pijplijnen met behulp van Azure Machine Learning en Data Factory Batch maken Uitvoering van activiteit](transform-data-using-machine-learning.md). Verschil is dat de uitvoer van de webservice training is een iLearner-bestand dat vervolgens wordt gebruikt door de activiteit van de Resource bijwerken de voorspellende webservice bijwerken. 
2. Een Azure Machine Learning gekoppelde service met het eindpunt van de resource update van de voorspellende webservice. Deze gekoppelde service wordt gebruikt door Resource bijwerken activiteit bij te werken de voorspellende webservice met behulp van het iLearner-bestand boven het stap geretourneerd. 

De configuratie is voor de tweede Azure Machine Learning gekoppelde service, verschillende wanneer uw Azure Machine Learning-webservice een klassieke webservice of een nieuwe Web-Service is. De verschillen worden afzonderlijk in de volgende secties worden besproken. 

## <a name="web-service-is-new-azure-resource-manager-web-service"></a>Webservice is een nieuwe Azure Resource Manager-webservice 

Als de webservice voor het nieuwe type-webservice dat toegang biedt tot een Azure Resource Manager-eindpunt is, hoeft u niet om toe te voegen van de tweede **niet-standaard** eindpunt. De **updateResourceEndpoint** is in de gekoppelde service van de indeling: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

U kunt waarden voor de houders plaats in de URL krijgen bij het opvragen van de web-service op de [Azure Machine Learning Web Services-Portal](https://services.azureml.net/). 

Het nieuwe type van het eindpunt van update resource vereist verificatie van de service-principal. Service-principal-verificatie gebruiken, het registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) en het geven de **Inzender** of **eigenaar** rol van het abonnement of de resource waar groep de webservice hoort bij. Het Zie [het service-principal maken en toewijzen van machtigingen voor het beheren van Azure-resource](../azure-resource-manager/resource-group-create-service-principal-portal.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Sleutel van toepassing 
- Tenant-id

Hier volgt een voorbeeld gekoppeld servicedefinitie: 

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
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. ",
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

Het volgende scenario biedt meer details. Er is een voorbeeld voor retraining en bijwerken van Azure ML-modellen van een Azure Data Factory-pijplijn.


## <a name="sample-retraining-and-updating-an-azure-machine-learning-model"></a>Voorbeeld: Retraining en bijwerken van een Azure Machine Learning-model

Deze sectie vindt u een voorbeeldpijplijn die gebruikmaakt van de **Azure ML-Batchuitvoering activiteit** naar opnieuw een model te trainen. De pijplijn gebruikt ook de **Azure ML bijwerken Resource activiteit** het model in de scoreprofiel webservice bijwerken. De sectie bevat ook JSON codefragmenten voor alle gekoppelde services, gegevenssets en pijplijn in het voorbeeld.

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

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Gekoppelde service voor Azure ML bijwerkbare score-eindpunt:
De volgende JSON-fragment definieert een Azure Machine Learning gekoppelde service die naar het bij te werken eindpunt van de score van de webservice verwijst.  

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
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De Batchuitvoering activiteit duurt de trainingsgegevens als invoer en een iLearner-bestand als uitvoer produceert. De activiteit bijwerken Resource vervolgens duurt dit iLearner-bestand en de voorspellende webservice bijwerkt. 

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
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
