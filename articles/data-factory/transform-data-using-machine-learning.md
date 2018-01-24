---
title: Maak voorspellende gegevenspijplijnen met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een Voorspellend pijplijn met behulp van Azure Machine Learning - Batchuitvoeringsactiviteit in Azure Data Factory.
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
ms.openlocfilehash: fa493a6d7b4cf775f64b87c1d5cc21ff4a138609
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Maak voorspellende pijplijnen met behulp van Azure Machine Learning en Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Versie 2 - Preview](transform-data-using-machine-learning.md)

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt u bouwen, testen en implementeren van predictive analytics-oplossingen. Het is voltooid uit op hoog niveau oogpunt in drie stappen:

1. **Maken van een trainingsexperiment**. U kunt deze stap doen met behulp van de Azure ML Studio. De ML studio is een gezamenlijke visual ontwikkelingsomgeving waarmee u trainen en te testen van een predictive analytics-model met trainingsgegevens.
2. **Converteren naar een Voorspellend experiment**. Nadat uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het beoordelen van nieuwe gegevens, kunt u voorbereiden en uw experiment voor score berekenen stroomlijnen.
3. **Als een webservice implementeren**. U kunt uw scoreprofiel experiment publiceren als een Azure-web-service. U kunt gegevens verzenden naar uw model via deze web service-eindpunt en ontvangen resultaat voorspellingen van het model.  

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Machine Learning-Batchuitvoeringsactiviteit in V1](v1/data-factory-azure-ml-batch-execution-activity.md).


### <a name="data-factory-and-machine-learning-together"></a>Data Factory en Machine Learning samen
Azure Data Factory kunt u gemakkelijk maken pijplijnen die gebruikmaken van een gepubliceerde [Azure Machine Learning] [azure machine learning] webservice voor predictive analytics. Met behulp van de **Batchuitvoeringsactiviteit** in een Azure Data Factory-pijplijn, kunt u een webservice Azure ML zodat voorspellingen op de gegevens in batch aanroept. 

Na verloop van tijd moeten de voorspellende modellen in de Azure ML experimenten score berekenen met behulp van de nieuwe invoergegevenssets worden retrained. U kunt een Azure ML-model van een Data Factory-pijplijn retrain als volgt u de volgende stappen uit:

1. Publiceer het trainingsexperiment (geen Voorspellend experiment) als een webservice. Deze stap in de Azure ML Studio doet u als hiervoor toen u Voorspellend experiment weergeven als een webservice in het voorgaande scenario.
2. Gebruik de Azure ML-Batchuitvoeringsactiviteit aanroepen van de webservice voor de trainingsexperiment. In principe kunt u de Azure ML-Batchuitvoering activiteit webservice training zowel scoreprofiel webservice aanroepen.

Nadat u klaar bent met de retraining, de scoreprofiel webservice bijwerken (Voorspellend experiment weergegeven als een webservice) met het nieuwe getrainde model met behulp van de **Azure ML-Resourceactiviteit**. Zie [bijwerken met behulp van de Update Resourceactiviteit modellen](update-machine-learning-models.md) artikel voor meer informatie.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service

U maakt een **Azure Machine Learning** gekoppelde service een Azure Machine Learning-webservice koppelen aan een Azure data factory. De gekoppelde Service wordt gebruikt door Azure Machine Learning-Batchuitvoeringsactiviteit en [Update Resourceactiviteit](update-machine-learning-models.md). 


```JSON
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Zie [gekoppelde services berekenen](compute-linked-services.md) artikel voor beschrijvingen over de eigenschappen in de JSON-definitie. 

Azure Machine Learning ondersteunt zowel klassieke Web Services als nieuwe Web-Services voor uw Voorspellend experiment. U kunt de juiste is van de Data Factory te gebruiken. Als u de informatie die is vereist voor het maken van de gekoppelde Service van Azure Machine Learning, gaat u naar https://services.azureml.net, waarbij alle (nieuw) Web Services en klassieke Web-Services worden vermeld. Klik op de webservice die u wilt openen, en op **verbruiken** pagina. Kopiëren **primaire sleutel** voor **apiKey** -eigenschap en **batchaanvragen** voor **mlEndpoint** eigenschap. 

![Azure Machine Learning-webservices](./media/transform-data-using-machine-learning/web-services.png)

##<a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning-Batchuitvoering activiteit

De volgende JSON-fragment definieert een Batchuitvoering van Azure Machine Learning-activiteit. De definitie van de activiteit bevat een verwijzing naar de Azure Machine Learning gekoppelde service die u eerder hebt gemaakt. 

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "<web service input name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                }, 
                "FilePath":"path1"
            }, 
            "<web service input name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference" 
                }, 
                "FilePath":"path2"
            }        
        },
        "webServiceOutputs": {
            "<web service output name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"path3"
            }, 
            "<web service output name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"path4"
            }         
        },
        "globalParameters": {
            "<Parameter 1 Name>": "<parameter value>",
            "<parameter 2 name>": "<parameter 2 value>"
        }
    }
}
```



| Eigenschap          | Beschrijving                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| naam              | Naam van de activiteit in de pijplijn     | Ja      |
| description       | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type              | Voor Data Lake Analytics U-SQL-activiteit, het type hoofdactiviteit is **AzureMLBatchExecution**. | Ja      |
| linkedServiceName | Gekoppelde Services naar de Azure Machine Learning gekoppelde Service. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| webServiceInputs  | Sleutel, waarde-paren, de namen van Azure Machine Learning Web Service invoerwaarden toewijzing. Sleutel moet overeenkomen met de invoerparameters gedefinieerd in de gepubliceerde Azure Machine Learning-webservice. De waarde is een gekoppelde Azure Storage-Services FilePath eigenschappen sleutelpaar en het opgeven van de invoer Blob-locaties. | Nee       |
| webServiceOutputs | Sleutel, waarde-paren, toewijzing van de namen van Azure Machine Learning Web Service uitvoer. Sleutel moet overeenkomen met de uitvoerparameters zijn gedefinieerd in de gepubliceerde Azure Machine Learning-webservice. Waarde is een gekoppelde Azure Storage-Services en FilePath eigenschappen paar opgeven van de uitvoer van de Blob-locaties. | Nee       |
| globalParameters  | Sleutel, waarde-paren worden doorgegeven aan het Azure ML Batch uitvoering Service-eindpunt. Sleutels moeten overeenkomen met de namen van de web service gedefinieerde parameters in de gepubliceerde Azure ML webservice. Waarden worden doorgegeven in de eigenschap GlobalParameters van de Azure ML-aanvraag voor het uitvoeren van batch | Nee       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario 1: Experimenten met behulp van Web service invoer/uitvoer die naar gegevens in Azure Blob-opslag verwijzen

In dit scenario wordt de Azure Machine Learning-webservice maakt voorspellingen met gegevens uit een bestand in een Azure blob storage en de resultaten van de voorspelling opslaat in de blobopslag. De volgende JSON definieert een Data Factory-pijplijn met een AzureMLBatchExecution-activiteit. De invoer- en -gegevens in Azure Blog van Storage naar wordt verwezen met behulp van een combinatie van LinkedName en FilePath. In de steekproef gekoppelde Service van het in- en uitgangen zijn verschillend, kunt u verschillende gekoppelde Services voor elk van uw invoer/uitvoer voor Data Factory kunnen de juiste bestanden kunnen worden opgepikt en verzenden naar Azure ML Web Service. 

> [!IMPORTANT]
> Poorten en globale parameters hebben in uw Azure ML-experiment, web service invoer en uitvoer standaardnamen ('input1', 'input2') die u kunt aanpassen. De namen die u voor webServiceInputs en webServiceOutputs globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de pagina Batch-uitvoering Help voor uw Azure ML-eindpunt om te controleren of de verwachte toewijzing.
>
> 

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "input1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                }, 
                "FilePath":"amltest/input/in1.csv"
            }, 
            "input2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference" 
                }, 
                "FilePath":"amltest/input/in2.csv"
            }        
        },
        "webServiceOutputs": {
            "outputName1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"amltest2/output/out1.csv"
            }, 
            "outputName2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"   
                }, 
                "FilePath":"amltest2/output/out2.csv"
            }         
        }
    }
}
```
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario 2: Experimenten lezer/schrijver-Modules gebruiken om te verwijzen naar gegevens in verschillende gelijksoortig
Bij het maken van Azure ML-experimenten een ander gebruikelijk scenario is het gebruik van gegevens van de invoer en uitvoer modules. De gegevens importeren-module wordt gebruikt om gegevens te laden in een experiment en de uitvoergegevens-module is het opslaan van gegevens vanuit uw experimenten. Zie voor meer informatie over het importeren en uitvoergegevens modules [importgegevens](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [uitvoergegevens](https://msdn.microsoft.com/library/azure/dn905984.aspx) onderwerpen op MSDN-bibliotheek.     

Wanneer u de gegevens van de invoer en uitvoer modules, is het raadzaam om een Web service-parameter gebruiken voor elke eigenschap van deze modules. Deze web-parameters kunnen u voor het configureren van de waarden tijdens runtime. U kunt bijvoorbeeld een experiment maken met een module gegevens importeren die gebruikmaakt van een Azure SQL Database: XXX.database.windows.net. Nadat de web-service is geïmplementeerd, wilt u de consumenten van de web-service om op te geven van een Azure SQL-Server genoemd inschakelen `YYY.database.windows.net`. Een parameter van Web service kunt u toestaan dat deze waarde moet worden geconfigureerd.

> [!NOTE]
> Web service invoer en uitvoer verschillen van de parameters van Web service. In het eerste scenario hebt u gezien hoe een invoer en uitvoer voor een webservice Azure ML kunnen worden opgegeven. In dit scenario kunt u parameters voor een webservice die overeenkomen met doorgeven aan de eigenschappen van gegevens-en uitvoergegevens importeren modules.
>
> 

We bekijken een scenario voor het gebruik van parameters van Web service. U hebt een geïmplementeerde Azure Machine Learning-webservice die gebruikmaakt van een module reader gegevens lezen uit een van de gegevensbronnen die wordt ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL Database). Nadat de batchuitvoering is uitgevoerd, kan de resultaten zijn geschreven met behulp van een module Writer (Azure SQL Database).  Er is geen web-service in- en uitgangen zijn gedefinieerd in de experimenten. In dit geval is het raadzaam dat u de parameters van de relevante web service voor de lezer en -schrijver modules configureert. Deze configuratie kunt de reader/writer modules worden geconfigureerd wanneer u de activiteit AzureMLBatchExecution. U Web serviceparameters opgeven in de **globalParameters** sectie als volgt in de JSON van de activiteit.

```JSON
"typeProperties": {
    "globalParameters": {
        "Database server name": "<myserver>.database.windows.net",
        "Database name": "<database>",
        "Server user account name": "<user name>",
        "Server user account password": "<password>"
    }
}
```


> [!NOTE]
> Parameters van de webservice zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>

Nadat u klaar bent met de retraining, de scoreprofiel webservice bijwerken (Voorspellend experiment weergegeven als een webservice) met het nieuwe getrainde model met behulp van de **Azure ML-Resourceactiviteit**. Zie [bijwerken met behulp van de Update Resourceactiviteit modellen](update-machine-learning-models.md) artikel voor meer informatie.



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
