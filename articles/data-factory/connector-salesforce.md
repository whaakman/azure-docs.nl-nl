---
title: "Gegevens kopiëren van Salesforce met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van Salesforce naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: jingwang
ms.openlocfilehash: 7978e955bf5516a853443555ab10a69dcf22d63f
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-salesforce-using-azure-data-factory"></a>Gegevens kopiëren van Salesforce met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-salesforce-connector.md)
> * [Versie 2 - Preview](connector-salesforce.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een database Salesforce. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Salesforce-connector in V1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de Salesforce-database kopiëren naar een ondersteunde sink data store. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder deze Salesforce-connector ondersteunt de volgende versies van Salesforce: **ontwikkelaarsversie, Professional Edition, Enterprise Edition of onbeperkte editie**. Kopiëren van gegevens van Salesforce ondersteunt **productie, sandbox en aangepast domein**.

## <a name="prerequisites"></a>Vereisten

* API-machtiging moet zijn ingeschakeld in Salesforce. Zie [hoe inschakelen API-toegang in Salesforce door machtigingenset?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>SalesForce aanvraaglimieten

SalesForce heeft limieten voor totaal aantal API-aanvragen en de gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen de limiet overschrijdt, beperking optreedt en er willekeurige fouten.
- Als het totale aantal aanvragen de limiet overschrijdt, wordt het Salesforce-account van 24 uur geblokkeerd.

De "REQUEST_LIMIT_EXCEEDED" fout kan ook worden weergegeven in beide scenario's. Zie de sectie 'API aanvraaglimieten' in de [Salesforce Developer limieten](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met de kopieeractiviteit middels de .NET SDK, Python SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [kopie activiteit zelfstudie](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Salesforce-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Salesforce gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **Salesforce**. |Ja |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br><br> -Standaard is `"https://login.salesforce.com"`. <br> -Geef om gegevens te kopiëren vanuit sandbox, `"https://test.salesforce.com"`. <br> -Om gegevens te kopiëren van een aangepast domein, opgeven, bijvoorbeeld `"https://[domain].my.salesforce.com"`. |Nee |
| gebruikersnaam |Geef een naam op voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount. |Ja |
| securityToken |Geef een beveiligingstoken voor de gebruikersaccount. Zie [security token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen of ophalen van een beveiligingstoken. Zie voor informatie over het algemeen over beveiligingstokens, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

**Voorbeeld:**

```json
{
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        }
    },
    "name": "SalesforceLinkedService"
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Salesforce-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van Salesforce, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | Naam van de tabel in het Salesforce-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Voorbeeld:**

```json
{
    "name": "SalesforceDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable__c"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Salesforce-bron.

### <a name="salesforce-as-source"></a>SalesForce als bron

Om gegevens te kopiëren van Salesforce, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query |Gebruik de aangepaste query om gegevens te lezen. U kunt een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Bijvoorbeeld: `select * from MyTable__c`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="query-tips"></a>Tips voor query

### <a name="retrieving-data-from-salesforce-report"></a>Ophalen van gegevens uit het Salesforce-rapport

U kunt gegevens ophalen uit het Salesforce-rapporten door te geven van de query als `{call "<report name>"}. Example: `'query': ' {aanroepen \"TestReport\"} ' is.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Bij het ophalen van records verwijderd uit Salesforce Prullenbak

Om te vragen de voorlopig verwijderde records uit Salesforce-Prullenbak, kunt u **' IsDeleted = 1 "** in uw query. Bijvoorbeeld:

* Opgeven om te vragen alleen de verwijderde records, ' Selecteer * uit MyTable__c **waar IsDeleted = 1**'
* Opgeven om te vragen alle records met inbegrip van de bestaande en de verwijderde, ' Selecteer * uit MyTable__c **waar IsDeleted = 0 of IsDeleted = 1**'

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Bij het ophalen van gegevens met behulp van waar component voor datum/tijd-kolom

Geef bij de SOQL of SQL-query, let u op het verschil van datum/tijd-indeling. Bijvoorbeeld:

* **Voorbeeld van SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', <datetime parameter>, <datetime parameter>)`
* **SQL-voorbeeld**:`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', <datetime parameter>, <datetime parameter>)`

## <a name="data-type-mapping-for-salesforce"></a>Toewijzing voor de Salesforce-gegevenstype

Bij het kopiëren van gegevens van Salesforce, worden de volgende toewijzingen van Salesforce gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| SalesForce-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Automatische getal |Tekenreeks |
| Selectievakje |Booleaanse waarde |
| Valuta |dubbele |
| Date |Datum/tijd |
| Datum/tijd |Datum/tijd |
| E-mail |Tekenreeks |
| Id |Tekenreeks |
| Opzoekrelatie |Tekenreeks |
| Meervoudige selectie selectielijst |Tekenreeks |
| Aantal |dubbele |
| Procent |dubbele |
| Telefoon |Tekenreeks |
| Selectielijst |Tekenreeks |
| Tekst |Tekenreeks |
| Tekstgebied |Tekenreeks |
| Tekstgebied (lang) |Tekenreeks |
| Tekstgebied (uitgebreid) |Tekenreeks |
| Tekst (versleuteld) |Tekenreeks |
| URL |Tekenreeks |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).