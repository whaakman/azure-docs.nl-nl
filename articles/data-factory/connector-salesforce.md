---
title: Gegevens kopiëren van en naar Salesforce met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van Salesforce op ondersteunde sink gegevensarchieven of van ondersteunde bron gegevensarchieven bij Salesforce met behulp van een kopieeractiviteit in een data factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: jingwang
ms.openlocfilehash: f4de97ef2df5351ac7e8574717ee1439b54a90e8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Salesforce met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1: algemeen verkrijgbaar](v1/data-factory-salesforce-connector.md)
> * [Versie 2 - Preview](connector-salesforce.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Salesforce. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory, die in het algemeen beschikbaar is, Zie [Salesforce-connector in versie 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Salesforce in alle ondersteunde sink-gegevensarchief. U kunt ook gegevens uit alle ondersteunde brongegevensarchief kopiëren naar Salesforce. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Salesforce-connector:

- SalesForce Developer, Professional, Enterprise of onbeperkt-edities.
- Kopiëren van gegevens van en naar de productie, sandbox en aangepast domein Salesforce.

## <a name="prerequisites"></a>Vereisten

API-machtiging moet zijn ingeschakeld in Salesforce. Zie voor meer informatie [inschakelen API-toegang in Salesforce door machtigingen zijn ingesteld](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>SalesForce aanvraaglimieten

SalesForce heeft limieten voor totaal aantal API-aanvragen en de gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen de limiet overschrijdt, beperking optreedt en er willekeurige fouten.
- Als het totale aantal aanvragen de limiet overschrijdt, wordt het Salesforce-account van 24 uur geblokkeerd.

U kunt ook het foutbericht 'REQUEST_LIMIT_EXCEEDED' in beide scenario's. Zie voor meer informatie de sectie 'Aanvraaglimieten API' in [Salesforce developer limieten](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met de Salesforce-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Salesforce gekoppeld.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op **Salesforce**. |Ja |
| environmentUrl | Geef de URL van het exemplaar van Salesforce. <br> -Standaard is `"https://login.salesforce.com"`. <br> -Geef om gegevens te kopiëren vanuit sandbox, `"https://test.salesforce.com"`. <br> -Om gegevens te kopiëren van een aangepast domein, opgeven, bijvoorbeeld `"https://[domain].my.salesforce.com"`. |Nee |
| gebruikersnaam |Geef een naam op voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount.<br/><br/>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| securityToken |Geef een beveiligingstoken voor de gebruikersaccount. Zie voor instructies over het opnieuw instellen en ophalen van een beveiligingstoken [ophalen van een beveiligingstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Zie voor informatie over het algemeen over beveiligingstokens, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Niet voor de gegevensbron, Ja voor sink als de bron gekoppeld service beschikt niet over integratie runtime |

>[!IMPORTANT]
>Wanneer u gegevens in Salesforce kopieert, kan niet de standaard Azure integratie Runtime worden gebruikt voor het kopiëren uitvoeren. Met andere woorden, als de bron gekoppeld service beschikt niet over een runtime opgegeven integratie expliciet [maken van een Azure-integratie Runtime](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Salesforce-exemplaar. Koppel de gekoppelde Salesforce service zoals in het volgende voorbeeld.

**Voorbeeld: Referenties opgeslagen in Data Factory**

```json
{
    "name": "SalesforceLinkedService",
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
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: Referenties opgeslagen in de Sleutelkluis**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Salesforce-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van en naar Salesforce, **SalesforceObject**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SalesforceObject**.  | Ja |
| objectApiName | De objectnaam Salesforce gegevens ophalen van. | Er is geen voor bron Ja voor sink |

> [!IMPORTANT]
> Het gedeelte '__c' van **API-naam** nodig voor elk object dat aangepast is.

![Data Factory Salesforce verbinding API-naam](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Voorbeeld:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Voor achterwaartse compatibiliteit: wanneer u gegevens van Salesforce, kopiëren als u de vorige 'RelationalTable' type gegevensset gebruikt, blijft deze werken terwijl u een suggestie ziet overschakelen naar het nieuwe 'SalesforceObject'-type.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **RelationalTable**. | Ja |
| tableName | De naam van de tabel in Salesforce. | Nee (als 'query' in de activiteitbron is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Salesforce-bron- en sink.

### <a name="salesforce-as-a-source-type"></a>SalesForce als een type gegevensbron

Om gegevens te kopiëren van Salesforce, stelt u het brontype in de kopieerbewerking naar **SalesforceSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **SalesforceSource**. | Ja |
| query |Gebruik de aangepaste query om gegevens te lezen. U kunt een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Een voorbeeld is `select * from MyTable__c`. | Nee (als 'tableName' in de gegevensset is opgegeven) |
| readBehavior | Geeft aan of query uitvoeren op de bestaande records, of alle records, inclusief de verwijderde query. Als niet wordt opgegeven, is het standaardgedrag voor het eerste. <br>Toegestane waarden: **query** (standaard), **queryAll**.  | Nee |

> [!IMPORTANT]
> Het gedeelte '__c' van **API-naam** nodig voor elk object dat aangepast is.

![Data Factory Salesforce verbinding lijst API-naam](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Voor achterwaartse compatibiliteit: wanneer u gegevens van Salesforce, kopiëren als u het vorige exemplaar van de 'RelationalSource'-type, de bron blijven werken terwijl u een suggestie ziet overschakelen naar het nieuwe 'SalesforceSource'-type.

### <a name="salesforce-as-a-sink-type"></a>SalesForce als sink-type

Om gegevens te kopiëren naar Salesforce, stelt u het sink-type in de kopieerbewerking naar **SalesforceSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op **SalesforceSink**. | Ja |
| WriteBehavior | Het gedrag schrijven voor de bewerking.<br/>Toegestane waarden zijn **invoegen** en **Upsert**. | Nee (de standaardwaarde is invoegen) |
| externalIdFieldName | De naam van het externe ID-veld voor de bewerking upsert. Het opgegeven veld moet worden gedefinieerd als 'Externe Id Field' in het Salesforce-object. Er kan geen NULL-waarden in de bijbehorende invoergegevens. | Ja voor "Upsert" |
| writeBatchSize | Het aantal rijen van de gegevens die naar Salesforce zijn geschreven in elke batch. | Nee (de standaardwaarde is 5.000) |
| ignoreNullValues | Hiermee wordt aangegeven of NULL-waarden van invoergegevens tijdens een schrijfactie genegeerd.<br/>Toegestane waarden zijn **true** en **false**.<br>- **De waarde True**: laat de gegevens in het doelobject ongewijzigd wanneer u een upsert of update-bewerking uitvoeren. Plaats een gedefinieerde standaardwaarde als u een insert-bewerking doet.<br/>- **ONWAAR**: de gegevens in het doelobject op NULL bijwerken wanneer u een upsert of update-bewerking uitvoeren. Plaats een NULL-waarde als u een insert-bewerking doet. | Nee (de standaardwaarde is ONWAAR) |

**Voorbeeld: Salesforce sink in een kopieeractiviteit**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Tips voor query

### <a name="retrieve-data-from-a-salesforce-report"></a>Gegevens ophalen uit een Salesforce-rapport

U kunt gegevens ophalen uit het Salesforce-rapporten door te geven van een query als `{call "<report name>"}`. Een voorbeeld is `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Verwijderde records uit de Salesforce-Prullenbak ophalen

Om te vragen de voorlopig verwijderde records uit de Salesforce-Prullenbak, kunt u **' IsDeleted = 1 "** in uw query. Bijvoorbeeld:

* Opgeven om te vragen alleen de verwijderde records, ' Selecteer * uit MyTable__c **waar IsDeleted = 1**. "
* Opgeven om te vragen van alle records, met inbegrip van de bestaande en de verwijderde, ' Selecteer * uit MyTable__c **waar IsDeleted = 0 of IsDeleted = 1**. "

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Gegevens ophalen met behulp van een where component voor de datum/tijd-kolom

Wanneer u de SOQL of SQL-query opgeeft, moet u aandacht schenken aan het verschil van datum/tijd-indeling. Bijvoorbeeld:

* **Voorbeeld van SOQL**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL-voorbeeld**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

## <a name="data-type-mapping-for-salesforce"></a>Toewijzing voor de Salesforce-gegevenstype

Als u gegevens van Salesforce kopieert, worden de volgende toewijzingen van Salesforce-gegevenstypen gebruikt voor Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe het brontype schema en de gegevens in de kopieerbewerking wordt toegewezen aan de sink, [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| SalesForce-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| Automatische getal |Tekenreeks |
| Selectievakje |Boole-waarde |
| Valuta |Double |
| Date |DateTime |
| Datum/tijd |DateTime |
| Email |Tekenreeks |
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

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).