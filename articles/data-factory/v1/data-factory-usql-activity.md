---
title: Gegevens transformeren met behulp van U-SQL-script - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformeren door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics compute-service.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/01/2017
ms.author: douglasl
robots: noindex
ms.openlocfilehash: 7631b103d6d14cceb2c320d56e9f68d9ea57e4d8
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020843"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Gegevens transformeren met U-SQL-scripts uitgevoerd op Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-usql-activity.md)
> * [Versie 2 (huidige versie)](../transform-data-using-data-lake-analytics.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [U-SQL-activiteit in V2](../transform-data-using-data-lake-analytics.md).

Een pijplijn in een Azure data factory worden gegevens in gekoppelde storage-services met behulp van gekoppelde compute services verwerkt. Het bevat een reeks activiteiten waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel beschrijft de **Data Lake Analytics U-SQL-activiteit** die wordt uitgevoerd een **U-SQL** script op een **Azure Data Lake Analytics** compute gekoppelde service. 

Een Azure Data Lake Analytics-account maken voordat u een pijplijn maakt met een Data Lake Analytics U-SQL-activiteit. Zie voor meer informatie over Azure Data Lake Analytics, [aan de slag met Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

Controleer de [bouw uw eerste pijplijn zelfstudie](data-factory-build-your-first-pipeline.md) voor gedetailleerde stappen voor het maken van een data factory, gekoppelde services, gegevenssets en een pijplijn. JSON-fragmenten met Data Factory-Editor of Visual Studio of Azure PowerShell gebruiken om te maken van Data Factory-entiteiten.

## <a name="supported-authentication-types"></a>Ondersteunde verificatietypen
U-SQL-activiteit ondersteunt hieronder verificatietypen op basis van Data Lake Analytics:
* Verificatie van service-principal
* Verificatie van de gebruiker referenties (OAuth) 

U wordt aangeraden gebruik te maken van service-principal verificatie, met name voor een geplande U-SQL-uitvoering. Geldigheidsduur van het token gedrag kan optreden met verificatie van gebruikersreferenties. Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#azure-data-lake-analytics-linked-service) sectie.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde Service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute-service aan een Azure data factory. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat beschrijvingen van de algemene eigenschappen in de JSON-definitie gebruikt. U kunt verder tussen service-principal en verificatie van gebruikersreferenties.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| **type** |De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. |Ja |
| **accountName** |Azure Data Lake Analytics-accountnaam. |Ja |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics-URI. |Nee |
| **Abonnements-id** |Azure-abonnement-id |Geen (indien niet opgegeven, abonnement van de data factory wordt gebruikt). |
| **resourceGroupName** |Naam van Azure-resourcegroep |Geen (indien niet opgegeven, de resourcegroep van de data factory wordt gebruikt). |

### <a name="service-principal-authentication-recommended"></a>Service-principal verificatie (aanbevolen)
Voor het gebruik van service-principal verificatie, de Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) en verleen deze de toegang tot Data Lake Store. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Toepassingssleutel 
* Tenant-id

Gebruik verificatie van service-principal door de volgende eigenschappen op te geven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **servicePrincipalId** | Opgeven van de toepassing client-ID. | Ja |
| **servicePrincipalKey** | Geef de sleutel van de toepassing. | Ja |
| **tenant** | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja |

**Voorbeeld: Service-principal verificatie**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Verificatie van gebruikersreferenties
U kunt ook gebruikersverificatie referentie voor Data Lake Analytics gebruiken door de volgende eigenschappen op te geven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| **Autorisatie** | Klik op de **autoriseren** knop in de Data Factory-Editor en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja |
| **sessie-id** | OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u de Data Factory-Editor gebruiken. | Ja |

**Voorbeeld: Verificatie van gebruikersreferenties**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Geldigheidsduur van het token
De autorisatiecode die u hebt gegenereerd met behulp van de **autoriseren** knop verloopt na enige tijd opnieuw uit. Zie de volgende tabel voor de vervaldatum tijden voor verschillende typen gebruikersaccounts. Mogelijk ziet u de volgende fout weergegeven wanneer de verificatie **token verloopt**: Referentie-bewerkingsfout: invalid_grant - AADSTS70002: Fout bij het valideren van referenties. AADSTS70008: De opgegeven toegang verlenen, is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 15-12-2015 21:09:31Z

| Gebruikerstype | Verloopt na |
|:--- |:--- |
| Gebruikersaccounts die niet worden beheerd door Azure Active Directory (@hotmail.com, @live.com, enz.) |12 uur |
| Gebruikersaccounts die worden beheerd door Azure Active Directory (AAD) |14 dagen na het laatste segment worden uitgevoerd. <br/><br/>90 dagen, als een segment op basis van de gekoppelde service op basis van OAuth ten minste eenmaal per 14 dagen wordt uitgevoerd. |

Als u deze fout voorkomen/oplossen, wilt autoriseren met behulp van de **autoriseren** knop wanneer de **token verloopt** en implementeer de gekoppelde service opnieuw. U kunt ook de waarden voor genereren **sessionId** en **autorisatie** eigenschappen met code via een programma als volgt:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Zie [AzureDataLakeStoreLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), en [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen voor meer informatie informatie over de Data Factory-klassen in de code gebruikt. Voeg een verwijzing naar: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de klasse WindowsFormsWebAuthenticationDialog. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-activiteit
De volgende JSON-fragment definieert een pijplijn met een Data Lake Analytics U-SQL-activiteit. De definitie van de activiteit bevat een verwijzing naar de Azure Data Lake Analytics gekoppelde service die u eerder hebt gemaakt.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

De volgende tabel beschrijft de namen en beschrijvingen van eigenschappen die specifiek voor deze activiteit zijn. 

| Eigenschap            | Description                              | Vereist                                 |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| type                | De eigenschap type moet worden ingesteld op **DataLakeAnalyticsU SQL**. | Ja                                      |
| linkedServiceName   | Verwijzing naar de Azure Data Lake Analytics geregistreerd als een gekoppelde service in Data Factory | Ja                                      |
| scriptPath          | Pad naar map met de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. | Nee (als u een script gebruiken)                   |
| scriptLinkedService | Gekoppelde service die is gekoppeld aan de opslag met het script aan de data factory | Nee (als u een script gebruiken)                   |
| script              | Geef inline script in plaats van scriptPath en scriptLinkedService op te geven. Bijvoorbeeld: `"script": "CREATE DATABASE test"`. | Nee (als u gebruik scriptPath en scriptLinkedService) |
| degreeOfParallelism | Het maximale aantal knooppunten dat tegelijk wordt gebruikt voor het uitvoeren van de taak. | Nee                                       |
| priority            | Hiermee bepaalt u welke taken uit in de wachtrij moeten worden geselecteerd moeten eerst worden uitgevoerd. Des te lager het nummer, hoe hoger de prioriteit. | Nee                                       |
| parameters          | Parameters voor de U-SQL-script          | Nee                                       |
| runtimeVersion      | Runtime-versie van de U-SQL-engine te gebruiken | Nee                                       |
| compilationMode     | <p>Compilatiemodus van U-SQL. Moet een van deze waarden:</p> <ul><li>**Semantische:** Alleen uitvoeren semantische controles en nodig bevestigingen.</li><li>**Volledige:** Voer de volledige compilatie, met inbegrip van syntaxiscontrole, optimalisatie, genereren van code, enzovoort.</li><li>**SingleBox:** De volledige compilatie, met TargetType instelling SingleBox uitvoeren.</li></ul><p>Als u een waarde voor deze eigenschap niet opgeeft, betekent dit dat de server de optimale compilatiemodus bepaalt. </p> | Nee                                       |

Zie [SearchLogProcessing.txt Script-definitie](#sample-u-sql-script) voor de script-definitie. 

## <a name="sample-input-and-output-datasets"></a>Voorbeeld van invoer- en uitvoergegevenssets
### <a name="input-dataset"></a>Invoergegevensset
In dit voorbeeld bevindt de invoergegevens zich in een Azure Data Lake Store (bestand SearchLog.tsv in de map datalake/input). 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Uitvoergegevensset
In dit voorbeeld wordt de uitvoergegevens die worden geproduceerd door de U-SQL-script opgeslagen in een Azure Data Lake Store (datalake/output-map). 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>Voorbeeld van Data Lake Store gekoppelde Service
Hier volgt de definitie van het voorbeeld van dat Azure Data Lake Store service die wordt gebruikt door de i/o-gegevenssets gekoppelde. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

Zie [gegevens verplaatsen naar en van Azure Data Lake Store](data-factory-azure-datalake-connector.md) artikel voor beschrijvingen van JSON-eigenschappen. 

## <a name="sample-u-sql-script"></a>Voorbeeld van een U-SQL Script

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
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

De waarden voor **@in** en **@out** parameters in de U-SQL-script worden doorgegeven dynamisch door ADF met behulp van het gedeelte 'parameters'. Zie de sectie 'parameters' in het pijplijndefinitie van de.

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
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

In dit geval invoerbestanden worden nog steeds opgehaald uit de map /datalake/input en uitvoerbestanden worden gegenereerd in de map /datalake/output. De bestandsnamen zijn dynamisch op basis van de starttijd van segment.  


