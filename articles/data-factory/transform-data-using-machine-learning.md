---
title: Voorspellende pijplijnen met behulp van Azure Data Factory maken | Microsoft Docs
description: Leer hoe u een Voorspellend pijplijn maken met behulp van Azure Machine Learning - Batchuitvoeringsactiviteit in Azure Data Factory.
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
ms.openlocfilehash: 052839c679fc3efa61fca612b5139ede1991890c
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55080239"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Voorspellende pijplijnen maken met Azure Machine Learning en Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Huidige versie](transform-data-using-machine-learning.md)

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) kunt bouwen, testen en implementeren van predictive analytics-oplossingen. Uit een oogpunt van de op hoog niveau is voltooid in drie stappen:

1. **Maken van een opleidingsexperiment**. U kunt deze stap uitvoert met behulp van de Azure Machine Learning studio. Azure Machine Learning studio is een samenwerkingsverband visual ontwikkelomgeving die u gebruiken om te trainen en testen van een predictive analytics-model met behulp van de trainingsgegevens.
2. **Converteren naar een Voorspellend experiment**. Zodra uw model is getraind met bestaande gegevens en u bent klaar om te gebruiken voor het scoren van nieuwe gegevens, kunt u voorbereidt en stroomlijnen van uw experiment voor het scoren.
3. **Als een webservice implementeren**. U kunt uw scoring experiment publiceren als een Azure-web-service. U kunt gegevens aan uw model via deze web service-eindpunt verzenden en ontvangen van resultaat voorspellingen uit het model.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory en Machine Learning samen
Azure Data Factory kunt u eenvoudig pijplijnen die gebruikmaken van een gepubliceerde [Azure Machine Learning] [azure-machine-learning] webservice voor voorspellende analyses te maken. Met behulp van de **Batchuitvoeringsactiviteit** in een Azure Data Factory-pijplijn, kunt u een Azure Machine Learning studio-webservice om voorspellingen te maken van de gegevens in batch aanroepen.

De voorspellende modellen in de Azure Machine Learning studio scoren experimenten moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. U kunt een opnieuw trainen van een model van een Data Factory-pijplijn door de volgende stappen:

1. Het trainingsexperiment (niet Voorspellend experiment) als een webservice publiceren. U voert deze stap in de Azure Machine Learning studio, net als Voorspellend experiment als een webservice in het voorgaande scenario beschikbaar wilt maken.
2. U Azure Machine Learning studio Batch Execution-activiteit gebruikt voor het aanroepen van de webservice voor de trainingsexperiment. In feite, kunt u de Azure Machine Learning studio Batch Execution-activiteit om aan te roepen zowel webservice training en scoring-webservice.

Als u klaar bent met het opnieuw trainen, bijwerken van de webservice scoring (Voorspellend experiment weergegeven als een webservice) met het zojuist getrainde model met behulp van de **Azure Machine Learning studio activiteit resources bijwerken**. Zie [bijwerken van modellen met behulp van de activiteit resources bijwerken](update-machine-learning-models.md) artikel voor meer informatie.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service

U maakt een **Azure Machine Learning** gekoppelde service om te koppelen van een Azure Machine Learning-webservice aan een Azure data factory. De gekoppelde Service wordt gebruikt door Azure Machine Learning Batch Execution-activiteit en [activiteit resources bijwerken](update-machine-learning-models.md).

```JSON
{
    "type" : "linkedServices",
    "name": "AzureMLLinkedService",
    "apiVersion" : "2017-09-01-preview",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Zie [gekoppelde services berekenen](compute-linked-services.md) artikel voor informatie over de eigenschappen in de JSON-definitie.

Azure Machine Learning ondersteuning voor zowel klassieke webservices en nieuwe Web-Services voor uw Voorspellend experiment. U kunt de juiste is voor het gebruik van Data Factory. Als u de informatie die is vereist voor het maken van de gekoppelde Service van Azure Machine Learning, gaat u naar https://services.azureml.net, waar alle (nieuw) Web Services en klassieke webservices worden weergegeven. Klik op de webservice die u wilt openen, en klikt u op **verbruiken** pagina. Kopie **primaire sleutel** voor **apiKey** eigenschap en **batchaanvragen** voor **mlEndpoint** eigenschap.

![Azure Machine Learning Web Services](./media/transform-data-using-machine-learning/web-services.png)

## <a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning Batch Execution-activiteit

De volgende JSON-fragment definieert een Azure Machine Learning Batch Execution-activiteit. De definitie van de activiteit bevat een verwijzing naar de Azure Machine Learning gekoppelde service die u eerder hebt gemaakt.

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

| Eigenschap          | Description                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| naam              | Naam van de activiteit in de pijplijn     | Ja      |
| description       | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type              | Voor Data Lake Analytics U-SQL-activiteit, het activiteitstype is **AzureMLBatchExecution**. | Ja      |
| linkedServiceName | Gekoppelde Services in de Azure Machine Learning gekoppelde Service. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| webServiceInputs  | Sleutel, waarde-paren, toewijzing van de namen van Azure Machine Learning Web Service invoer. Sleutel moet overeenkomen met de invoerparameters die zijn gedefinieerd in de gepubliceerde Azure Machine Learning-webservice. De waarde is een gekoppelde Azure Storage-Services FilePath eigenschappen sleutelpaar en de invoer Blob-locaties op te geven. | Nee       |
| webServiceOutputs | Sleutel, waarde-paren, toewijzing van de namen van Azure Machine Learning Web Service uitvoer. Sleutel moet overeenkomen met de parameters gedefinieerd in de gepubliceerde Azure Machine Learning-webservice. Waarde is een gekoppelde Azure Storage-Services en FilePath eigenschappen-paar op te geven de uitvoer Blob-locaties. | Nee       |
| globalParameters  | Sleutel, waarde-paren moeten worden doorgegeven aan de Azure Machine Learning studio Batchuitvoeringsservice-eindpunt. Sleutels moeten overeenkomen met de namen van de webserviceparameters die zijn gedefinieerd in de gepubliceerde Azure Machine Learning studio-webservice. De waarden worden doorgegeven in de eigenschap GlobalParameters van de aanvraag voor het Azure Machine Learning studio batch uitvoeren | Nee       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Scenario 1: Met behulp van Web service invoer/uitvoer die naar gegevens in Azure Blob Storage verwijzen-experimenten

In dit scenario, de Azure Machine Learning-webservice Hiermee worden voorspellingen gedaan met behulp van gegevens uit een bestand in een Azure blob-opslag en worden de voorspellingsresultaten opgeslagen in de blob-opslag. De volgende JSON wordt een Data Factory-pijplijn met een activiteit AzureMLBatchExecution gedefinieerd. De invoer- en -gegevens in Azure BLOB-opslag wordt verwezen met een paar LinkedName en FilePath. In het voorbeeld gekoppelde Service van de invoer en uitvoer verschillen, kunt u verschillende gekoppelde Services voor elk van uw invoer/uitvoer voor Data Factory te kunnen ophalen van de juiste bestanden en verzenden naar Azure Machine Learning studio-webservice.

> [!IMPORTANT]
> Poorten en globale parameters hebben in uw Azure Machine Learning studio-experiment, web-invoer en uitvoer standaardnamen ('input1', 'input2') die u kunt aanpassen. De namen die u voor het webServiceInputs webServiceOutputs en globalParameters instellingen gebruikt moeten exact overeenkomen met de namen in de experimenten. U kunt de nettolading van de voorbeeld-aanvraag bekijken op de Help-pagina voor het eindpunt van de Azure Machine Learning studio om te controleren of de verwachte toewijzing.
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
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Scenario 2: Experimenten Reader/Writer-Modules gebruiken om te verwijzen naar gegevens in verschillende opslagruimten
Bij het maken van Azure Machine Learning studio-experimenten een ander gebruikelijk scenario is het gebruik van modules importeren en uitvoergegevens. De module gegevens importeren wordt gebruikt om gegevens te laden in een experiment en de uitvoergegevens-module is bij het opslaan van gegevens vanuit uw experimenten. Zie voor meer informatie over het importeren van gegevens- en uitvoergegevens modules [importgegevens](https://msdn.microsoft.com/library/azure/dn905997.aspx) en [uitvoergegevens](https://msdn.microsoft.com/library/azure/dn905984.aspx) -onderwerpen op MSDN-bibliotheek.

Wanneer u de modules importeren en uitvoergegevens, is het verstandig aan een Web service-parameter gebruiken voor elke eigenschap van deze modules. Deze web-parameters kunnen u de waarden worden geconfigureerd tijdens runtime. U kunt bijvoorbeeld een experiment maken met een module gegevens importeren die gebruikmaakt van een Azure SQL Database: XXX.database.windows.net. Nadat de web-service is geïmplementeerd, wilt u zodat gebruikers om op te geven van een Azure SQL-Server met de naam van de webservice `YYY.database.windows.net`. Een Web service-parameter kunt u toestaan dat deze waarde moet worden geconfigureerd.

> [!NOTE]
> Web service invoer en uitvoer verschillen van de parameters van de Web service. In het eerste scenario, hebt u gezien hoe een invoer en uitvoer voor een Azure Machine Learning studio-webservice kunnen worden opgegeven. In dit scenario kunt u parameters voor een webservice die overeenkomen met doorgeven aan de eigenschappen van gegevens-en uitvoergegevens importeren van modules.
>
>

We bekijken een scenario voor het gebruik van parameters van de Web service. U hebt een geïmplementeerde Azure Machine Learning-webservice die gebruikmaakt van een module reader gegevens lezen uit een van de gegevensbronnen ondersteund door Azure Machine Learning (bijvoorbeeld: Azure SQL Database). Nadat de batchuitvoering is uitgevoerd, kan de resultaten worden geschreven met behulp van een schrijfmodule (Azure SQL Database).  Er zijn geen web service invoer en uitvoer worden gedefinieerd in de experimenten. In dit geval is het raadzaam dat u de relevante webserviceparameters voor lees- en schrijftoegang modules configureert. Deze configuratie kan de reader/writer modules worden geconfigureerd wanneer u de activiteit AzureMLBatchExecution. U webserviceparameters in de **globalParameters** sectie in de activiteits-JSON als volgt.

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
> De Web service-parameters zijn hoofdlettergevoelig, dus zorg ervoor dat de namen die u opgeeft in de activiteits-JSON overeenkomen met de gegevenstypen die worden weergegeven door de webservice.
>

Als u klaar bent met het opnieuw trainen, bijwerken van de webservice scoring (Voorspellend experiment weergegeven als een webservice) met het zojuist getrainde model met behulp van de **Azure Machine Learning studio activiteit resources bijwerken**. Zie [bijwerken van modellen met behulp van de activiteit resources bijwerken](update-machine-learning-models.md) artikel voor meer informatie.

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
