---
title: Transformeer gegevens met behulp van U-SQL-script - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformatie gegevens door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics compute-service.
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
ms.openlocfilehash: 7800329e7f56d604c7911d3997fa76a0fac91664
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Transformeer gegevens door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-usql-activity.md)
> * [Versie 2 - Preview](transform-data-using-data-lake-analytics.md)

Een pijplijn in een Azure data factory verwerkt gegevens in gekoppelde storage-services met behulp van gekoppelde compute services. Het bevat een reeks activiteiten, waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel beschrijft de **Data Lake Analytics U-SQL-activiteit** die wordt uitgevoerd een **U-SQL** script op een **Azure Data Lake Analytics** berekenen van de gekoppelde service. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [USQL activiteit in V1](v1/data-factory-usql-activity.md).

Een Azure Data Lake Analytics-account maken voordat u een pijplijn maakt met een Data Lake Analytics U-SQL-activiteit. Zie voor meer informatie over Azure Data Lake Analytics, [aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).


## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute een Azure data factory-service. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat beschrijvingen van de algemene eigenschappen die in de JSON-definitie. 

| Eigenschap                 | Beschrijving                              | Vereist                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja                                      |
| **accountName**          | Azure Data Lake Analytics-accountnaam.  | Ja                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics-URI.           | Nee                                       |
| **abonnements-id**       | Azure-abonnement-ID                    | Nee (als niet wordt opgegeven, abonnement van de gegevensfactory wordt gebruikt). |
| **resourceGroupName**    | Naam van een Azure-resourcegroep                | Nee (als niet wordt opgegeven, brongroep van de gegevensfactory wordt gebruikt). |

### <a name="service-principal-authentication"></a>Verificatie van service-principal
De service Azure Data Lake Analytics gekoppeld vereist een service-principal-verificatie verbinding maken met de Azure Data Lake Analytics-service. Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) en het het toegang geven tot zowel de Data Lake Analytics en Data Lake Store wordt gebruikt voor het gebruik van verificatie van de service-principal. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Sleutel van toepassing 
* Tenant-id

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap                | Beschrijving                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Geef de toepassing client-ID.     | Ja      |
| **servicePrincipalKey** | De sleutel van de toepassing opgeven.           | Ja      |
| **tenant**              | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja      |

**Voorbeeld: Service-principal-verificatie**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }       
    }
}
```

Zie voor meer informatie over de gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md).

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-activiteit
De volgende JSON-fragment definieert een pijplijn met een Data Lake Analytics U-SQL-activiteit. De definitie van de activiteit bevat een verwijzing naar de Azure Data Lake Analytics gekoppelde service die u eerder hebt gemaakt. Voor het uitvoeren van een Data Lake Analytics U-SQL-script, Data Factory verzendt het script dat u hebt opgegeven voor het Data Lake Analytics en de vereiste invoer en uitvoer is gedefinieerd in het script voor Data Lake Analytics om te halen en de uitvoer. 

```json
{
    "name": "ADLA U-SQL Activity",
    "description": "description",
    "type": "DataLakeAnalyticsU-SQL",
    "linkedServiceName": {
        "referenceName": "AzureDataLakeAnalyticsLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "LinkedServiceofAzureBlobStorageforscriptPath",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
        "degreeOfParallelism": 3,
        "priority": 100,
        "parameters": {
            "in": "/datalake/input/SearchLog.tsv",
            "out": "/datalake/output/Result.tsv"
        }
    }   
}
```

De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn. 

| Eigenschap            | Beschrijving                              | Vereist |
| :------------------ | :--------------------------------------- | :------- |
| naam                | Naam van de activiteit in de pijplijn     | Ja      |
| description         | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                | Voor Data Lake Analytics U-SQL-activiteit, het type hoofdactiviteit is **DataLakeAnalyticsU SQL**. | Ja      |
| linkedServiceName   | Gekoppelde Service met Azure Data Lake Analytics. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.  |Ja       |
| scriptPath          | Pad naar de map waarin de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. | Ja      |
| scriptLinkedService | Gekoppelde service die is gekoppeld aan de opslag die het script aan de gegevensfactory bevat | Ja      |
| degreeOfParallelism | Het maximum aantal knooppunten dat tegelijk worden gebruikt voor het uitvoeren van de taak. | Nee       |
| prioriteit            | Hiermee wordt bepaald welke taken uit in de wachtrij moeten eerst worden geselecteerd. Hoe lager het getal, hoe hoger de prioriteit. | Nee       |
| parameters          | Parameters voor de U-SQL-script          | Nee       |
| runtimeVersion      | Runtime-versie van de U-SQL-engine gebruiken | Nee       |
| compilationMode     | <p>Compilatiemodus van U-SQL. U moet een van deze waarden: **semantische:** alleen uitvoeren semantische controles en de benodigde bevestigingen, **volledige:** uitvoeren van de volledige compilatie, met inbegrip van syntaxiscontrole, optimalisatie, genereren van code, enz., **SingleBox:** de volledige compilatie, met TargetType instelling SingleBox uitvoeren. Als u een waarde voor deze eigenschap niet opgeeft, wordt door de server de optimale compilatiemodus bepaalt. | Nee |

Data Factory verzendt het Zie [SearchLogProcessing.txt Script definitie](#sample-u-sql-script) voor de definitie van het script. 

## <a name="sample-u-sql-script"></a>U-SQL-voorbeeldscript

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

In bovenstaande script bijvoorbeeld de invoer en uitvoer naar het script is gedefinieerd in  **@in**  en  **@out**  parameters. De waarden voor  **@in**  en  **@out**  parameters in de U-SQL-script worden doorgegeven dynamisch door Data Factory met behulp van de sectie 'parameters'. 

U kunt ook andere eigenschappen zoals degreeOfParallelism en prioriteit opgeven in de definitie van de pijplijn voor de taken die worden uitgevoerd op de Azure Data Lake Analytics-service.

## <a name="dynamic-parameters"></a>Dynamische parameters
In de definitie van de pijplijn voorbeeld zijn en afmelden parameters toegewezen met vastgelegde waarden. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Het is mogelijk in plaats daarvan dynamische parameters gebruiken. Bijvoorbeeld: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

In dit geval invoerbestanden zijn nog steeds opgehaald uit de map /datalake/input en uitvoerbestanden worden gegenereerd in de map /datalake/output. De bestandsnamen zijn dynamisch, op basis van de begintijd segment.  

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe voor het transformeren van gegevens op andere manieren: 

* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchuitvoering activiteit](transform-data-using-machine-learning.md)
* [De activiteit opgeslagen procedure](transform-data-using-stored-procedure.md)
