---
title: Gegevens transformeren met behulp van U-SQL-script - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformeren door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics compute-service.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: abnarain
ms.openlocfilehash: 9918dd55181eb82257f23f8974159ed5e762fedd
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268074"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Gegevens transformeren met U-SQL-scripts uitgevoerd op Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-usql-activity.md)
> * [Huidige versie](transform-data-using-data-lake-analytics.md)

Een pijplijn in een Azure data factory worden gegevens in gekoppelde storage-services met behulp van gekoppelde compute services verwerkt. Het bevat een reeks activiteiten waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel beschrijft de **Data Lake Analytics U-SQL-activiteit** die wordt uitgevoerd een **U-SQL** script op een **Azure Data Lake Analytics** compute gekoppelde service. 

Een Azure Data Lake Analytics-account maken voordat u een pijplijn maakt met een Data Lake Analytics U-SQL-activiteit. Zie voor meer informatie over Azure Data Lake Analytics, [aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md).


## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute-service aan een Azure data factory. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat beschrijvingen van de algemene eigenschappen in de JSON-definitie gebruikt. 

| Eigenschap                 | Description                              | Vereist                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja                                      |
| **accountName**          | Azure Data Lake Analytics-accountnaam.  | Ja                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics-URI.           | Nee                                       |
| **subscriptionId**       | Azure-abonnement-ID                    | Nee                                       |
| **resourceGroupName**    | Naam van Azure-resourcegroep                | Nee                                       |

### <a name="service-principal-authentication"></a>Verificatie van service-principal
De Azure Data Lake Analytics gekoppelde service is een service-principal-verificatie verbinding maken met de Azure Data Lake Analytics-service. Voor het gebruik van service-principal verificatie, de Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) en de toegang tot zowel de Data Lake Analytics en de Data Lake Store wordt verleend. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

* Toepassings-id
* Toepassingssleutel 
* Tenant-id

Service principal toestemming voor het gebruik van uw Azure Data Lake Anatlyics geven de [Wizard gebruiker toevoegen](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#add-a-new-user).

Gebruik verificatie van service-principal door de volgende eigenschappen op te geven:

| Eigenschap                | Description                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Opgeven van de toepassing client-ID.     | Ja      |
| **servicePrincipalKey** | Geef de sleutel van de toepassing.           | Ja      |
| **tenant**              | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja      |

**Voorbeeld: Service-principal verificatie**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "<azure data lake analytics URI>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }       
    }
}
```

Zie voor meer informatie over de gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md).

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-activiteit
De volgende JSON-fragment definieert een pijplijn met een Data Lake Analytics U-SQL-activiteit. De definitie van de activiteit bevat een verwijzing naar de Azure Data Lake Analytics gekoppelde service die u eerder hebt gemaakt. Voor het uitvoeren van een Data Lake Analytics U-SQL-script, dient de Data Factory het script dat u hebt opgegeven met de Data Lake Analytics, en de vereiste invoer en uitvoer is gedefinieerd in het script voor Data Lake Analytics worden opgehaald en uitgevoerd. 

```json
{
    "name": "ADLA U-SQL Activity",
    "description": "description",
    "type": "DataLakeAnalyticsU-SQL",
    "linkedServiceName": {
        "referenceName": "<linked service name of Azure Data Lake Analytics>",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "<linked service name of Azure Data Lake Store or Azure Storage which contains the U-SQL script>",
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

| Eigenschap            | Description                              | Vereist |
| :------------------ | :--------------------------------------- | :------- |
| naam                | Naam van de activiteit in de pijplijn     | Ja      |
| description         | Tekst die beschrijft wat de activiteit doet.  | Nee       |
| type                | Voor Data Lake Analytics U-SQL-activiteit, het activiteitstype is **DataLakeAnalyticsU SQL**. | Ja      |
| linkedServiceName   | Azure Data Lake Analytics gekoppelde Service. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel.  |Ja       |
| scriptPath          | Pad naar map met de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. | Ja      |
| scriptLinkedService | Gekoppelde service die is gekoppeld aan de **Azure Data Lake Store** of **Azure Storage** waarin het script aan de data factory | Ja      |
| degreeOfParallelism | Het maximale aantal knooppunten dat tegelijk wordt gebruikt voor het uitvoeren van de taak. | Nee       |
| priority            | Hiermee bepaalt u welke taken uit in de wachtrij moeten worden geselecteerd moeten eerst worden uitgevoerd. Des te lager het nummer, hoe hoger de prioriteit. | Nee       |
| parameters          | Parameters moeten worden doorgegeven in de U-SQL-script.    | Nee       |
| runtimeVersion      | Runtime-versie van de U-SQL-engine te gebruiken. | Nee       |
| compilationMode     | <p>Compilatiemodus van U-SQL. Moet een van deze waarden: **Semantische:** Alleen uitvoeren semantische controles en nodig bevestigingen, **volledige:** Uitvoeren van de volledige compilatie, met inbegrip van syntaxiscontrole, optimalisatie, genereren van code, enz., **SingleBox:** De volledige compilatie, met TargetType instelling SingleBox uitvoeren. Als u een waarde voor deze eigenschap niet opgeeft, betekent dit dat de server de optimale compilatiemodus bepaalt. | Nee |

Zie [SearchLogProcessing.txt](#sample-u-sql-script) voor de script-definitie. 

## <a name="sample-u-sql-script"></a>Voorbeeld van een U-SQL script

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

In bovenstaande voorbeeld van een script, de invoer en uitvoer naar het script is gedefinieerd in **@in** en **@out** parameters. De waarden voor **@in** en **@out** parameters in de U-SQL-script worden doorgegeven dynamisch door Data Factory met behulp van het gedeelte 'parameters'. 

U kunt ook andere eigenschappen zoals degreeOfParallelism en prioriteit opgeven in de pijplijndefinitie van de voor de taken die worden uitgevoerd op de Azure Data Lake Analytics-service.

## <a name="dynamic-parameters"></a>Dynamische parameters
In het definitie van de pijplijn voorbeeld worden in en uit parameters toegewezen met vastgelegde waarden. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Het is mogelijk in plaats daarvan dynamische parameters gebruiken. Bijvoorbeeld: 

```json
"parameters": {
    "in": "/datalake/input/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/data.tsv",
    "out": "/datalake/output/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/result.tsv"
}
```

In dit geval invoerbestanden worden nog steeds opgehaald uit de map /datalake/input en uitvoerbestanden worden gegenereerd in de map /datalake/output. De bestandsnamen zijn dynamisch op basis van de begintijd wordt doorgegeven in de pijplijn wordt geactiveerd.  

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren: 

* [Hive-activiteit](transform-data-using-hadoop-hive.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
