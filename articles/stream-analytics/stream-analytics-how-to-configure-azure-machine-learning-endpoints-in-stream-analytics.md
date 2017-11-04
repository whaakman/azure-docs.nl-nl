---
title: Gebruik van Azure Machine Learning-eindpunten in Stream Analytics | Microsoft Docs
description: Taal van de machine door de gebruiker gedefinieerde functies in Stream Analytics
keywords: 
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: ca656ce11f267340872f4b8566d0ee21791f29d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a>Machine Learning-integratie in Stream Analytics
Stream Analytics ondersteunt de gebruiker gedefinieerde functies met Azure Machine Learning-eindpunten roepen. REST-API-ondersteuning voor deze functie wordt beschreven in de [REST-API voor Stream Analytics-bibliotheek](https://msdn.microsoft.com/library/azure/dn835031.aspx). In dit artikel biedt extra informatie die nodig zijn voor een succesvolle implementatie van deze mogelijkheid in Stream Analytics. Een zelfstudie is ook geplaatst en is beschikbaar [hier](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Overzicht: Azure Machine Learning-terminologie
Microsoft Azure Machine Learning biedt een gezamenlijke slepen en neerzetten hulpprogramma die kunt u om te bouwen, testen en implementeren van predictive analytics-oplossingen voor uw gegevens. Dit programma heet de *Azure Machine Learning Studio*. De studio wordt gebruikt om te communiceren met de Machine Learning-bronnen en eenvoudig bouwen, testen en te herhalen uw ontwerp. Deze resources en de bijbehorende definities zijn hieronder.

* **Werkruimte**: de *werkruimte* is een container met alle andere bronnen van Machine Learning samen in een container voor beheer en controle.
* **Experiment**: *experimenten* zijn gemaakt door gegevenswetenschappers tot gegevenssets te gebruiken en een machine learning-model te trainen.
* **Eindpunt**: *eindpunten* het Azure Machine Learning-object is gebruikt voor functies als invoer ophaalt, een opgegeven machine learning-model en retourneert scored uitvoer zijn.
* **Score berekenen voor Webservice**: A *score berekenen voor webservice* is een verzameling van eindpunten zoals hierboven vermeld.

Elk eindpunt heeft API's voor Batchuitvoering en synchrone uitvoering. Stream Analytics gebruikt synchrone uitvoering. De specifieke service heet een [aanvraag/antwoord-Service](../machine-learning/studio/consume-web-services.md) in AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Machine Learning-bronnen die nodig zijn voor Stream Analytics-taken
Taak verwerken is een aanvraag/antwoord-eindpunt voor de toepassing van de Stream Analytics een [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), en een swagger-definitie voor alle nodig zijn voor uitvoering te doen slagen. Stream Analytics is een extra eindpunt dat Hiermee wordt de url voor het swagger-eindpunt, de interface worden opgezocht en retourneert een default UDF-definitie voor de gebruiker.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Configureren van een Stream Analytics en Machine Learning-UDF via REST-API
U kunt de taak voor het Azure Machine Language-functies aanroepen configureren met behulp van REST-API's. De stappen zijn als volgt:

1. Een Stream Analytics-taak maken
2. Invoer definiëren
3. Uitvoer definiëren
4. Maken van een door de gebruiker gedefinieerde functie (UDF)
5. Schrijven van een Stream Analytics-transformatie die de UDF-aanroepen
6. Start de taak

## <a name="creating-a-udf-with-basic-properties"></a>Het maken van een UDF met basiseigenschappen
Als u bijvoorbeeld de volgende voorbeeldcode maakt een scalaire UDF met de naam *newudf* die wordt gekoppeld aan een Azure Machine Learning-eindpunt. Houd er rekening mee dat de *eindpunt* (service-URI) kunt u vinden op de help-pagina van de API voor de gekozen service en de *apiKey* vindt u op de hoofdpagina van Services.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Voorbeeld van de aanvraag hoofdtekst:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Eindpunt van de RetrieveDefaultDefinition-aanroep voor standaard UDF
Zodra het basisproject UDF is gemaakt is de volledige definitie van de UDF nodig. Het eindpunt RetreiveDefaultDefinition krijgt u de default-definitie voor een scalaire functie die is gebonden aan een Azure Machine Learning-eindpunt. De nettolading van de onderstaande moet u de standaard UDF-definitie voor een scalaire functie die is gebonden aan een Azure Machine Learning-eindpunt. Het werkelijke eindpunt wordt niet gedefinieerd als deze al is opgegeven tijdens de PUT-aanvraag. Stream Analytics roept het eindpunt dat is opgegeven in de aanvraag als expliciet is opgegeven. Als deze wordt gebruikt met een oorspronkelijk waarnaar wordt verwezen. Hier tekenreeks het UDF vindt één parameter (een zin) en retourneert één uitvoer van het type string die aangeeft dat het label 'gevoel' voor de die zin.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Voorbeeld van de aanvraag hoofdtekst:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Een voorbeeld van uitvoer van deze zoekt u naar iets wilt hieronder.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Patch UDF met de reactie
De UDF moet nu worden gevuld met het vorige antwoord, zoals hieronder wordt weergegeven.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Aanvraagtekst (uitvoer van de RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Transformatie van de Stream Analytics om aan te roepen de UDF implementeren
Nu de UDF (scoreTweet hier de naam) voor elke gebeurtenis invoer vragen en in een antwoord voor die gebeurtenis geschreven naar uitvoer.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Help opvragen
Voor verdere hulp kunt u mogelijk terecht op het [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
