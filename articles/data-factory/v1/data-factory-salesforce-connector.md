---
title: Gegevens verplaatsen van Salesforce worden met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens van Salesforce worden met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: a9dba65591479033a892615ff053eebd0862851e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125667"
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Gegevens verplaatsen van Salesforce worden met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-salesforce-connector.md)
> * [Versie 2 (huidige versie)](../connector-salesforce.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Salesforce-connector in V2](../connector-salesforce.md).


In dit artikel bevat een overzicht van hoe u Copy Activity in een Azure data factory kunt gebruiken om gegevens te kopiëren uit Salesforce aan een gegevensopslag die wordt vermeld onder de Sink-kolom in de [ondersteunde bronnen en sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de Kopieeractiviteit en combinaties van ondersteunde data store geeft.

Azure Data Factory ondersteunt momenteel alleen gegevens te verplaatsen van Salesforce op [sinkgegevensopslag ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats), maar biedt geen ondersteuning voor het verplaatsen van gegevens van andere gegevens worden opgeslagen met Salesforce.

## <a name="supported-versions"></a>Ondersteunde versies
Deze connector ondersteunt de volgende versies van Salesforce: Developer Edition, Professional Edition, Enterprise Edition of onbeperkte Edition. En het kopiëren van Salesforce-productie, sandbox- en aangepaste domein ondersteunt.

## <a name="prerequisites"></a>Vereisten
* API-machtiging moet zijn ingeschakeld. Zie [hoe schakel ik API-toegang in Salesforce door machtigingenset?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Als u wilt kopiëren van gegevens van Salesforce worden naar on-premises gegevensarchieven, hebt u ten minste Data Management Gateway 2.0 is geïnstalleerd in uw on-premises omgeving.

## <a name="salesforce-request-limits"></a>SalesForce-aanvraaglimieten
SalesForce heeft limieten voor totaal aantal API-aanvragen en gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen van de limiet overschrijdt, beperking optreedt en worden er willekeurig mislukken.
- Als het totale aantal aanvragen heeft de limiet overschrijdt, wordt de Salesforce-account wordt geblokkeerd voor 24 uur.

Mogelijk ontvangt u ook de fout 'REQUEST_LIMIT_EXCEEDED' in beide scenario's. Zie de sectie 'API aanvraaglimieten' in de [Salesforce Developer limieten](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens van Salesforce worden verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon** , **.NET API**, en **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt voor het kopiëren van gegevens uit Salesforce, [JSON-voorbeeld: gegevens kopiëren van Salesforce naar Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Salesforce: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de service Salesforce die zijn gekoppeld zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Salesforce**. |Ja |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br><br> -De standaardwaarde is "https://login.salesforce.com'. <br> -Om gegevens te kopiëren van sandbox, Geef 'https://test.salesforce.com'. <br> -Om gegevens te kopiëren uit aangepaste domein, opgeven, bijvoorbeeld 'https://[domain].my.salesforce.com'. |Nee |
| gebruikersnaam |Geef een gebruikersnaam voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount. |Ja |
| securityToken |Geef een beveiligingstoken voor het gebruikersaccount. Zie [security-token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen/ophalen van een beveiligingstoken. Zie voor meer informatie over beveiligingstokens het in het algemeen, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle gegevenssettypen (Azure SQL, Azure-blob, Azure-tabel, enzovoort).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties voor een gegevensset van het type sectie **RelationalTable** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel in Salesforce. |Nee (als een **query** van **RelationalSource** is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is nodig voor een aangepaste object.

![Naam van de Data Factory - verbinding met Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [het maken van pijplijnen](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en verschillende beleidsregels zijn beschikbaar voor alle soorten activiteiten.

De eigenschappen die beschikbaar in de sectie typeProperties van de activiteit zijn, wordt aan de andere kant variëren met elk activiteitstype. Ze verschillen voor de Kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

In de kopieeractiviteit, wanneer de bron van het type **RelationalSource** (waaronder Salesforce), de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |Een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Bijvoorbeeld `select * from MyTable__c`. |Nee (als de **tableName** van de **gegevensset** is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is nodig voor een aangepaste object.

![Naam van de Data Factory - verbinding met Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Querytips
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Bij het ophalen van gegevens met behulp van waar u component voor de datum/tijd-kolom
Wanneer de SOQL of SQL-query, let u op het verschil van datum/tijd-indeling opgeven. Bijvoorbeeld:

* **Voorbeeld van SOQL**: `$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL-voorbeeld**:
    * **Geef de query met behulp van de wizard kopiëren:** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Met behulp van JSON bewerken om op te geven van de query (char correct escape):** `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Bij het ophalen van gegevens uit Salesforce-rapport
U kunt gegevens uit Salesforce-rapporten ophalen door het opgeven van de query als `{call "<report name>"}`, bijvoorbeeld. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Bij het ophalen van records verwijderd uit de Prullenbak van Salesforce
Om te vragen de voorlopig verwijderde records uit de Prullenbak van Salesforce, kunt u **"IsDeleted = 1"** in uw query. Bijvoorbeeld:

* Om te vragen alleen de verwijderde records, Geef "Selecteer * uit MyTable__c **waar IsDeleted = 1**"
* Opgeven om te vragen alle records met inbegrip van de bestaande en de verwijderde, "Selecteer * uit MyTable__c **waar IsDeleted = 0 of IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a>Voorbeeld van JSON: gegevens kopiëren van Salesforce naar Azure Blob
Het volgende voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens uit Salesforce kopiëren naar Azure Blob Storage. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.   

Hier vindt u de Data Factory-artefacten die u moet maken om het scenario te implementeren. De secties die volgen op de lijst vindt u informatie over deze stappen.

* Een gekoppelde service van het type [Salesforce](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**SalesForce gekoppelde service**

In dit voorbeeld wordt de **Salesforce** gekoppelde service. Zie de [Salesforce gekoppelde service](#linked-service-properties) sectie voor de eigenschappen die worden ondersteund door deze gekoppelde service.  Zie [security-token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen/ophalen van het beveiligingstoken.

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Een gekoppelde Azure Storage-service**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
    }
}
```
**SalesForce-invoergegevensset**

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"  
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Instellen van **externe** naar **waar** informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is nodig voor een aangepaste object.

![Naam van de Data Factory - verbinding met Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**De Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pijplijn met Kopieeractiviteit**

De pijplijn bevat Copy-activiteit, die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets, en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource**, en de **sink** type is ingesteld op **BlobSink**.

Zie [RelationalSource typeproperties](#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door de RelationalSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"                
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```
> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is nodig voor een aangepaste object.

![Naam van de Data Factory - verbinding met Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Toewijzing van het type voor Salesforce
| Salesforce-type | . NET op basis van type |
| --- | --- |
| Automatisch nummer |Reeks |
| Selectievakje |Booleaans |
| Valuta |decimaal |
| Date |DateTime |
| Datum/tijd |DateTime |
| Email |Reeks |
| Id |Reeks |
| Opzoekrelatie |Reeks |
| Met meerdere keuzemogelijkheden |Reeks |
| Aantal |decimaal |
| Procent |decimaal |
| Telefoon |Reeks |
| Selectielijst |Reeks |
| Tekst |Reeks |
| Tekstgebied |Reeks |
| Tekstgebied (lang) |Reeks |
| Tekstgebied (uitgebreid) |Reeks |
| Tekst (versleuteld) |Reeks |
| URL |Reeks |

> [!NOTE]
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
