---
title: Web-activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u Web-activiteit, een van de controlestroomactiviteiten ondersteund door Data Factory kunt gebruiken om aan te roepen een REST-eindpunt van een pijplijn.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 04b542bf1f77b75c1c92b147b578df630b86d0ac
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="web-activity-in-azure-data-factory"></a>Webactiviteit in Azure Data Factory
De WebActivity kan worden gebruikt om een aangepast REST-eindpunt aan te roepen vanaf een Data Factory-pijplijn. U kunt gegevenssets en gekoppelde services doorgeven die moten worden verbruikt door en die toegankelijk zijn voor de activiteit. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).

## <a name="syntax"></a>Syntaxis

```json
{  
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{  
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{  
         "Content-Type":"application/json"
      },
      "authentication":{  
         "type":"ClientCertificate",  
         "pfx":"****",
         "password":"****"
      },
      "datasets":[  
         {  
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{  
               ...
            }
         }
      ],
      "linkedServices":[  
         {  
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de webactiviteit | Tekenreeks | Ja
type | Moet worden ingesteld op **WebActivity**. | Tekenreeks | Ja
methode | Rest-API-methode voor het doel-eindpunt. | De tekenreeks. <br/><br/>Ondersteunde typen: 'GET', 'Posten', 'PUT' | Ja
url | Doel-eindpunt en het pad | Tekenreeks (of een expressie met een resultType van een tekenreeks) | Ja
headers | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, om de taal en type ingesteld op een aanvraag: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Tekenreeks (of een expressie met een resultType van een tekenreeks) | Ja, de header Content-type is vereist. `"headers":{ "Content-Type":"application/json"}`
hoofdtekst | Hiermee geeft u de nettolading dat wordt verzonden naar het eindpunt. Vereist voor de methoden POST/PUT.  | Tekenreeks (of een expressie met een resultType van een tekenreeks). <br/><br/>Zie het schema van de nettolading van de aanvraag in [aanvraag nettolading schema](#request-payload-schema) sectie. | Nee
verificatie | De verificatiemethode die wordt gebruikt voor het aanroepen van het eindpunt. Ondersteunde typen zijn "Basic of ClientCertificate." Zie voor meer informatie [verificatie](#authentication) sectie. Als verificatie niet vereist is, sluit u deze eigenschap. | Tekenreeks (of een expressie met een resultType van een tekenreeks) | Nee
Gegevenssets | Lijst met gegevenssets doorgegeven aan het eindpunt. | Matrix van verwijzingen van de gegevensset. Een lege matrix kan zijn. | Ja
linkedServices | Lijst met gekoppelde services doorgegeven aan eindpunt. | Matrix van verwijzingen van de gekoppelde service. Een lege matrix kan zijn. | Ja

> [!NOTE]
> REST-eindpunten die de activiteit webpagina roept moeten een antwoord van het type JSON retourneren.

## <a name="authentication"></a>Verificatie

### <a name="none"></a>None
Als verificatie niet vereist is, bevatten geen de eigenschap 'verificatie'.

### <a name="basic"></a>Basic
Geef de gebruikersnaam en wachtwoord voor gebruik met de basisverificatie. 

```json
"authentication":{  
   "type":"Basic,
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Clientcertificaat
Base64-gecodeerde inhoud van een PFX-bestand en het wachtwoord opgeven. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```
## <a name="request-payload-schema"></a>Aanvraag nettolading schema
Wanneer u de POST/PUT-methode gebruikt, vertegenwoordigt de eigenschap body de nettolading dat wordt verzonden naar het eindpunt. U kunt de gekoppelde services en gegevenssets doorgeven als onderdeel van de nettolading. Hier wordt het schema voor de nettolading van de: 

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
} 
```

## <a name="example"></a>Voorbeeld
In dit voorbeeld roept de webactiviteit in de pipeline een REST-eindpunt. Het wordt een Azure SQL gekoppelde service en een Azure SQL-gegevensset doorgegeven aan het eindpunt. De REST-eindpunt maakt gebruik van de Azure SQL-verbindingsreeks verbinding maken met de Azure SQL-server en retourneert de naam van het exemplaar van SQL server. 

### <a name="pipeline-definition"></a>Pipeline-definitie

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>Pipeline-parameterwaarden

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Web service-eindpunt-code

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
