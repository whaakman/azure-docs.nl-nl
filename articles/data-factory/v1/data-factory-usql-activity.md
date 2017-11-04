---
title: Transformeer gegevens met behulp van U-SQL-script - Azure | Microsoft Docs
description: Meer informatie over verwerken of transformatie gegevens door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics compute-service.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: ff91a3da978fd027605b3674eae14d1d74b309cd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Transformeer gegevens door het U-SQL-scripts uitvoeren op Azure Data Lake Analytics 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-usql-activity.md)
> * [Versie 2 - Preview](../transform-data-using-data-lake-analytics.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [U-SQL-activiteit in V2](../transform-data-using-data-lake-analytics.md).

Een pijplijn in een Azure data factory verwerkt gegevens in gekoppelde storage-services met behulp van gekoppelde compute services. Het bevat een reeks activiteiten, waarbij elke activiteit uitvoert voor een specifieke verwerking. In dit artikel beschrijft de **Data Lake Analytics U-SQL-activiteit** die wordt uitgevoerd een **U-SQL** script op een **Azure Data Lake Analytics** berekenen van de gekoppelde service. 

Een Azure Data Lake Analytics-account maken voordat u een pijplijn maakt met een Data Lake Analytics U-SQL-activiteit. Zie voor meer informatie over Azure Data Lake Analytics, [aan de slag met Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

Controleer de [bouwen van uw eerste pijplijn-zelfstudie](data-factory-build-your-first-pipeline.md) voor gedetailleerde stappen voor het maken van een gegevensfactory, gekoppelde services, gegevenssets en een pijplijn. JSON-codefragmenten gebruiken met Data Factory-Editor of Visual Studio of Azure PowerShell Data Factory-entiteiten maken.

## <a name="supported-authentication-types"></a>Ondersteunde verificatietypen
U-SQL-activiteit ondersteunt hieronder verificatietypen tegen Data Lake Analytics:
* Verificatie van service-principal
* Verificatie van gebruikersreferenties (OAuth) 

Het is raadzaam dat u service-principal verificatie, met name voor een geplande U-SQL-uitvoering. Verlopen van het token kan gebeuren met verificatie van gebruikersreferenties. Zie voor configuratiedetails de [gekoppelde service-eigenschappen](#azure-data-lake-analytics-linked-service) sectie.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde Service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute een Azure data factory-service. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat beschrijvingen van de algemene eigenschappen die in de JSON-definitie. U kunt verder tussen service-principal en verificatie van gebruikersreferenties.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **type** |De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. |Ja |
| **Accountnaam** |Azure Data Lake Analytics-accountnaam. |Ja |
| **dataLakeAnalyticsUri** |Azure Data Lake Analytics-URI. |Nee |
| **abonnements-id** |Azure-abonnement-id |Nee (als niet wordt opgegeven, abonnement van de gegevensfactory wordt gebruikt). |
| **resourceGroupName** |Naam van een Azure-resourcegroep |Nee (als niet wordt opgegeven, brongroep van de gegevensfactory wordt gebruikt). |

### <a name="service-principal-authentication-recommended"></a>Verificatie van de service principal (aanbevolen)
Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) voor het gebruik van verificatie van de service-principal en wordt de toegang verlenen tot Data Lake Store. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Sleutel van toepassing 
* Tenant-id

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **servicePrincipalId** | Geef de toepassing client-ID. | Ja |
| **servicePrincipalKey** | De sleutel van de toepassing opgeven. | Ja |
| **tenant** | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja |

**Voorbeeld: Service-principal-verificatie**
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
U kunt ook referentie gebruikersverificatie voor Data Lake Analytics gebruiken door te geven van de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| **autorisatie** | Klik op de **autoriseren** in de Data Factory-Editor en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja |
| **sessie-id** | OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u de Data Factory-Editor. | Ja |

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

#### <a name="token-expiration"></a>Verlopen van het token
De autorisatiecode die u hebt gegenereerd met de **autoriseren** knop verloopt na enige tijd opnieuw. Zie de volgende tabel voor de vervaldatum tijdstippen voor verschillende soorten gebruikersaccounts. Mogelijk ziet u de volgende fout weergegeven wanneer de verificatie **-token verloopt**: referentie-bewerkingsfout: invalid_grant - AADSTS70002: fout bij het valideren van referenties. AADSTS70008: De toegewezen toegangsmachtiging is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21:09:31Z

| Gebruikerstype | Verloopt na |
|:--- |:--- |
| Gebruikersaccounts die niet worden beheerd door Azure Active Directory (@hotmail.com, @live.com, enz.) |12 uur |
| Gebruikersaccounts die worden beheerd door Azure Active Directory (AAD) |het is 14 dagen na het laatste segment worden uitgevoerd. <br/><br/>negentig dagen weergegeven, als een segment op basis van de gekoppelde service op basis van OAuth ten minste eenmaal in de 14 dagen wordt uitgevoerd. |

Autoriseren om deze fout voorkomen/oplossen, met behulp van de **autoriseren** knop wanneer de **-token verloopt** en de gekoppelde service implementeren. U kunt ook de waarden voor genereren **sessionId** en **autorisatie** eigenschappen programmatisch met code als volgt:

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

Zie [AzureDataLakeStoreLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), en [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen voor meer informatie over de Data Factory-klassen die in de code wordt gebruikt. Voeg een verwijzing naar: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de klasse WindowsFormsWebAuthenticationDialog. 

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

| Eigenschap            | Beschrijving                              | Vereist                                 |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| type                | De eigenschap type moet worden ingesteld op **DataLakeAnalyticsU SQL**. | Ja                                      |
| linkedServiceName   | Verwijzing naar de Azure Data Lake Analytics geregistreerd als een gekoppelde service in de Data Factory | Ja                                      |
| scriptPath          | Pad naar de map waarin de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. | Nee (als u een script gebruiken)                   |
| scriptLinkedService | Gekoppelde service die is gekoppeld aan de opslag die het script aan de gegevensfactory bevat | Nee (als u een script gebruiken)                   |
| Script              | Geef in plaats van het opgeven van scriptPath en scriptLinkedService inline-script. Bijvoorbeeld: `"script": "CREATE DATABASE test"`. | Nee (als u scriptPath en scriptLinkedService gebruiken) |
| degreeOfParallelism | Het maximum aantal knooppunten dat tegelijk worden gebruikt voor het uitvoeren van de taak. | Nee                                       |
| Prioriteit            | Hiermee wordt bepaald welke taken uit in de wachtrij moeten eerst worden geselecteerd. Hoe lager het getal, hoe hoger de prioriteit. | Nee                                       |
| parameters          | Parameters voor de U-SQL-script          | Nee                                       |
| runtimeVersion      | Runtime-versie van de U-SQL-engine gebruiken | Nee                                       |
| compilationMode     | <p>Compilatiemodus van U-SQL. Moet een van de volgende waarden:</p> <ul><li>**Semantische:** alleen uitvoeren semantische controles en de benodigde bevestigingen.</li><li>**Volledige:** uitvoeren van de volledige compilatie, met inbegrip van syntaxiscontrole, optimalisatie, genereren van code, enzovoort.</li><li>**SingleBox:** de volledige compilatie, met TargetType instelling SingleBox uitvoeren.</li></ul><p>Als u een waarde voor deze eigenschap niet opgeeft, wordt door de server de optimale compilatiemodus bepaalt. </p> | Nee                                       |

Zie [SearchLogProcessing.txt Script definitie](#sample-u-sql-script) voor de definitie van het script. 

## <a name="sample-input-and-output-datasets"></a>Voorbeeld van invoer- en uitvoergegevenssets
### <a name="input-dataset"></a>Invoergegevensset
In dit voorbeeld bevindt de invoergegevens zich in een Azure Data Lake Store (SearchLog.tsv bestanden in de map datalake/invoer). 

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
In dit voorbeeld wordt de uitvoergegevens die wordt geproduceerd door de U-SQL-script opgeslagen in een Azure Data Lake Store (datalake/uitvoermap). 

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

### <a name="sample-data-lake-store-linked-service"></a>Sample Data Lake Store gekoppelde Service
Hier volgt de definitie van het voorbeeld dat Azure Data Lake Store service die wordt gebruikt door de i/o-gegevenssets gekoppelde. 

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

Zie [gegevens verplaatsen naar en van Azure Data Lake Store](data-factory-azure-datalake-connector.md) artikel voor een beschrijving van de JSON-eigenschappen. 

## <a name="sample-u-sql-script"></a>U-SQL-voorbeeldscript

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

De waarden voor  **@in**  en  **@out**  parameters in de U-SQL-script worden doorgegeven dynamisch door ADF met behulp van de sectie 'parameters'. Zie de sectie 'parameters' in de definitie van de pijplijn.

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


