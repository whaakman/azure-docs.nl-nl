---
title: Gegevens verplaatsen van Salesforce met behulp van de Data Factory | Microsoft Docs
description: Informatie over het verplaatsen van gegevens uit het Salesforce met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9e678e947a686b5a672af13cb0f0e60b4a272de9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Gegevens verplaatsen van Salesforce met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-salesforce-connector.md)
> * [Versie 2 - Preview](../connector-salesforce.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Salesforce-connector in V2](../connector-salesforce.md).


In dit artikel bevat een overzicht van hoe u kunt Kopieeractiviteit in een Azure data factory om gegevens van Salesforce kopiëren naar een gegevensopslag die wordt vermeld onder de Sink-kolom in de [ondersteunde gegevensbronnen en put](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, waarin Kopieeractiviteit en combinaties van ondersteunde gegevens store een algemeen overzicht van de verplaatsing van gegevens biedt.

Azure Data Factory ondersteunt momenteel alleen zwevend gegevens van Salesforce op [ondersteunde gegevensarchieven sink](data-factory-data-movement-activities.md#supported-data-stores-and-formats), maar biedt geen ondersteuning voor het verplaatsen van gegevens van andere gegevens worden opgeslagen bij Salesforce.

## <a name="supported-versions"></a>Ondersteunde versies
Deze connector ondersteunt de volgende versies van Salesforce: ontwikkelaarsversie, Professional Edition, Enterprise Edition of onbeperkte Edition. En het kopiëren van Salesforce productie, sandbox en aangepast domein ondersteunt.

## <a name="prerequisites"></a>Vereisten
* API-machtiging moet zijn ingeschakeld. Zie [hoe inschakelen API-toegang in Salesforce door machtigingenset?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Als u wilt gegevens kopiëren van Salesforce naar on-premises gegevensopslagexemplaren, hebt u ten minste Data Management Gateway 2.0 is geïnstalleerd in uw on-premises omgeving.

## <a name="salesforce-request-limits"></a>SalesForce aanvraaglimieten
SalesForce heeft limieten voor totaal aantal API-aanvragen en de gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen de limiet overschrijdt, beperking plaatsvindt en worden er willekeurige fouten.
- Als het totale aantal aanvragen de limiet overschrijdt, wordt het Salesforce-account van 24 uur geblokkeerd.

De "REQUEST_LIMIT_EXCEEDED" fout kan ook worden weergegeven in beide scenario's. Zie de sectie 'API aanvraaglimieten' in de [Salesforce Developer limieten](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens worden verplaatst van Salesforce met verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren van Salesforce [JSON-voorbeeld: gegevens kopiëren van Salesforce naar Azure Blob](#json-example-copy-data-from-salesforce-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke bij Salesforce: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de service Salesforce gekoppeld zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Salesforce**. |Ja |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br><br> -Standaard is 'https://login.salesforce.com'. <br> -Geef om gegevens te kopiëren vanuit sandbox, 'https://test.salesforce.com'. <br> -Om gegevens te kopiëren van een aangepast domein, opgeven, bijvoorbeeld 'https://[domain].my.salesforce.com'. |Nee |
| gebruikersnaam |Geef een naam op voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount. |Ja |
| securityToken |Geef een beveiligingstoken voor de gebruikersaccount. Zie [security token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen of ophalen van een beveiligingstoken. Zie voor informatie over het algemeen over beveiligingstokens, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst met eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen gegevensset (Azure blob Azure SQL Azure-tabel en enzovoort).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor een gegevensset van het type **RelationalTable** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in Salesforce. |Nee (als een **query** van **RelationalSource** is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst met eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen, zoals naam, beschrijving, invoer en uitvoer tabellen en verschillende beleidsregels zijn beschikbaar voor alle typen activiteiten.

De eigenschappen die beschikbaar in de sectie typeProperties van de activiteit zijn, worden aan de andere kant variëren met elk activiteitstype. Voor de Kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

In de kopieeractiviteit, wanneer de bron van het type **RelationalSource** (waaronder Salesforce), de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |Een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Bijvoorbeeld `select * from MyTable__c`. |Nee (als de **tableName** van de **gegevensset** is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Tips voor query
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Bij het ophalen van gegevens met behulp van waar component voor datum/tijd-kolom
Geef bij de SOQL of SQL-query, let u op het verschil van datum/tijd-indeling. Bijvoorbeeld:

* **Voorbeeld van SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL-voorbeeld**:
    * **Wizard kopiëren gebruiken om op te geven van de query:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Met een JSON bewerken om op te geven van de query (char goed escape):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Ophalen van gegevens uit het Salesforce-rapport
U kunt gegevens ophalen uit het Salesforce-rapporten door te geven van de query als `{call "<report name>"}`, bijvoorbeeld. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Bij het ophalen van records verwijderd uit Salesforce Prullenbak
Om te vragen de voorlopig verwijderde records uit Salesforce-Prullenbak, kunt u **' IsDeleted = 1 "** in uw query. Bijvoorbeeld:

* Opgeven om te vragen alleen de verwijderde records, ' Selecteer * uit MyTable__c **waar IsDeleted = 1**'
* Opgeven om te vragen alle records met inbegrip van de bestaande en de verwijderde, ' Selecteer * uit MyTable__c **waar IsDeleted = 0 of IsDeleted = 1**'

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van Salesforce naar Azure Blob
Het volgende voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren van Salesforce naar Azure Blob Storage. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.   

Hier volgen de Data Factory-artefacten die u wilt maken om het scenario te implementeren. Ga als de lijst met volgt de volgende secties vindt u informatie over deze stappen.

* Een gekoppelde service van het type [Salesforce](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)

**SalesForce gekoppelde service**

In dit voorbeeld wordt de **Salesforce** gekoppelde service. Zie de [Salesforce gekoppelde service](#linked-service-properties) sectie voor de eigenschappen die worden ondersteund door deze gekoppelde service.  Zie [security token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen of ophalen van het beveiligingstoken.

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
**SalesForce invoergegevensset**

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

Instelling **externe** naar **true** informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**De Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1).

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

**Pijplijn met de Kopieeractiviteit**

De pijplijn bevat Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **RelationalSource**, en de **sink** type is ingesteld op **BlobSink**.

Zie [RelationalSource type-eigenschappen](#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door de RelationalSource.

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
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Toewijzing van het type voor Salesforce
| Salesforce-type | . Type op basis van NET |
| --- | --- |
| Automatische getal |Tekenreeks |
| Selectievakje |Boole-waarde |
| Valuta |Double |
| Date |Datum en tijd |
| Datum/tijd |Datum en tijd |
| E-mail |Tekenreeks |
| Id |Tekenreeks |
| Opzoekrelatie |Tekenreeks |
| Meervoudige selectie selectielijst |Tekenreeks |
| Aantal |Double |
| Procent |Double |
| Telefoon |Tekenreeks |
| Selectielijst |Tekenreeks |
| Tekst |Tekenreeks |
| Tekstgebied |Tekenreeks |
| Tekstgebied (lang) |Tekenreeks |
| Tekstgebied (uitgebreid) |Tekenreeks |
| Tekst (versleuteld) |Tekenreeks |
| URL |Tekenreeks |

> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
