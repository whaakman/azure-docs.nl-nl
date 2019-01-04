---
title: Web-activiteit in Azure Data Factory | Microsoft Docs
description: Lees hoe u Web-activiteit, een van de controlestroomactiviteiten ondersteund door Data Factory, kunt gebruiken om aan te roepen een REST-eindpunt van een pijplijn.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: shlo
ms.openlocfilehash: 1c657fd7b3059dcf46a371b133a5d078e02f0599
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014196"
---
# <a name="web-activity-in-azure-data-factory"></a>Webactiviteit in Azure Data Factory
De WebActivity kan worden gebruikt om een aangepast REST-eindpunt aan te roepen vanaf een Data Factory-pijplijn. U kunt gegevenssets en gekoppelde services doorgeven die moten worden verbruikt door en die toegankelijk zijn voor de activiteit. 

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

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
naam | Naam van de webactiviteit | Reeks | Ja
type | Moet worden ingesteld op **WebActivity**. | Reeks | Ja
method | De rest-API-methode voor het doel-eindpunt. | tekenreeks. <br/><br/>Ondersteunde typen: 'OPHALEN', 'POST', 'PLAATS' | Ja
url | Doel-eindpunt en het pad | Tekenreeks (of expressie resultType van de tekenreeks). De activiteit wordt een time-out op 1 minuut met een fout als het heeft geen reactie ontvangen van het eindpunt. | Ja
Headers | Headers die worden verzonden naar de aanvraag. Bijvoorbeeld, de taal en het type instellen op een aanvraag: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Tekenreeks (of expressie resultType van de tekenreeks) | Ja, de Content-type-header is vereist. `"headers":{ "Content-Type":"application/json"}`
hoofdtekst | Hiermee geeft u de nettolading die wordt verzonden naar het eindpunt.  | Tekenreeks (of expressie resultType van de tekenreeks). <br/><br/>Het schema van de nettolading van de aanvraag in [verzoek nettolading schema](#request-payload-schema) sectie. | Vereist voor POST/PUT-methoden.
verificatie | De verificatiemethode die wordt gebruikt voor het aanroepen van het eindpunt. Ondersteunde typen zijn "Basic of ClientCertificate." Zie voor meer informatie, [verificatie](#authentication) sectie. Als verificatie niet vereist is, sluit u deze eigenschap. | Tekenreeks (of expressie resultType van de tekenreeks) | Nee
gegevenssets | Lijst met gegevenssets doorgegeven aan het eindpunt. | Matrix van verwijzingen van de gegevensset. Geen lege matrix kan zijn. | Ja
linkedServices | Lijst met gekoppelde services doorgegeven aan eindpunt. | Matrix van gekoppelde service verwijst naar. Geen lege matrix kan zijn. | Ja

> [!NOTE]
> REST-eindpunten die de webactiviteit aanroept moeten een antwoord van het type JSON. De activiteit wordt een time-out op 1 minuut met een fout als het heeft geen reactie ontvangen van het eindpunt.

De volgende tabel ziet u de vereisten voor JSON-inhoud:

| Waardetype | Aanvraagbody | De hoofdtekst van antwoord |
|---|---|---|
|JSON-object | Ondersteund | Ondersteund |
|JSON-matrix | Ondersteund <br/>(Op dit moment JSON-matrices werken niet als gevolg van een bug. Een oplossing wordt uitgevoerd.) | Niet-ondersteund |
| JSON-waarde | Ondersteund | Niet-ondersteund |
| Niet-JSON-type | Niet-ondersteund | Niet-ondersteund |
||||

## <a name="authentication"></a>Verificatie

### <a name="none"></a>Geen
Als verificatie niet vereist is, omvatten niet de eigenschap 'verificatie'.

### <a name="basic"></a>Basic
Geef de gebruikersnaam en wachtwoord voor gebruik met de basisverificatie. 

```json
"authentication":{  
   "type":"Basic",
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Clientcertificaat
Met base64 gecodeerde inhoud van een PFX-bestand en het wachtwoord opgeven. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```

### <a name="managed-identity"></a>Beheerde identiteit

Hiermee geeft u de resource-uri waarvoor het toegangstoken wordt aangevraagd met behulp van de beheerde identiteit voor de data factory. Gebruiken voor het aanroepen van de Azure Resource Management API `https://management.azure.com/`.

```json
"authentication": {
    "type": "MSI",
    "resource": "https://management.azure.com/"
}
```

## <a name="request-payload-schema"></a>Verzoek nettolading schema
Wanneer u de POST/PUT-methode gebruikt, vertegenwoordigt de van de hoofdteksteigenschap de nettolading die wordt verzonden naar het eindpunt. U kunt de gekoppelde services en gegevenssets als onderdeel van de nettolading doorgeven. Dit is het schema voor de nettolading van de: 

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
In dit voorbeeld wordt de webactiviteit in de pijplijn een REST-eindpunt. Deze doorstuurt een gekoppelde Azure SQL-service en een Azure SQL-gegevensset naar het eindpunt. De REST-eindpunt maakt gebruik van de Azure SQL-verbindingsreeks verbinding maken met de Azure SQL-server en retourneert de naam van het exemplaar van SQL server. 

### <a name="pipeline-definition"></a>De pijplijndefinitie van de

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

### <a name="pipeline-parameter-values"></a>Pijplijn parameterwaarden

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
