---
title: "Gegevens kopiëren van/naar Salesforce met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Salesforce tot gegevensarchieven ondersteunde sink (of) van de ondersteunde bron gegevensarchieven bij Salesforce met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: 7cd86922b0445fc81766ca54080e2fd3e64a6c61
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-fromto-salesforce-using-azure-data-factory"></a>Gegevens kopiëren van/naar Salesforce met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-salesforce-connector.md)
> * [Versie 2 - Preview](connector-salesforce.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Salesforce. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Salesforce-connector in V1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Salesforce kopiëren naar een ondersteunde sink-gegevensarchief of of gegevens kopiëren van een ondersteunde brongegevensarchief naar Salesforce. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Salesforce-connector:

- De volgende versies van Salesforce: **ontwikkelaarsversie, Professional Edition, Enterprise Edition of onbeperkte editie**.
- Kopiëren van gegevens van/naar Salesforce **productie, sandbox en aangepast domein**.

## <a name="prerequisites"></a>Vereisten

* API-machtiging moet zijn ingeschakeld in Salesforce. Zie [hoe inschakelen API-toegang in Salesforce door machtigingenset?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>SalesForce aanvraaglimieten

SalesForce heeft limieten voor totaal aantal API-aanvragen en de gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen de limiet overschrijdt, beperking optreedt en er willekeurige fouten.
- Als het totale aantal aanvragen de limiet overschrijdt, wordt het Salesforce-account van 24 uur geblokkeerd.

De "REQUEST_LIMIT_EXCEEDED" fout kan ook worden weergegeven in beide scenario's. Zie de sectie 'API aanvraaglimieten' in de [Salesforce Developer limieten](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Salesforce-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Salesforce gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **Salesforce**. |Ja |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br> -Standaard is `"https://login.salesforce.com"`. <br> -Geef om gegevens te kopiëren vanuit sandbox, `"https://test.salesforce.com"`. <br> -Om gegevens te kopiëren van een aangepast domein, opgeven, bijvoorbeeld `"https://[domain].my.salesforce.com"`. |Nee |
| gebruikersnaam |Geef een naam op voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount.<br/><br/>U kunt kiezen voor dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord worden opgeslagen in Azure Sleutelkluis en de kopie vizier pull daar bij het uitvoeren van de gegevens opnieuw te kopiëren: meer informatie kunt [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). |Ja |
| securityToken |Geef een beveiligingstoken voor de gebruikersaccount. Zie [security token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen of ophalen van een beveiligingstoken. Zie voor informatie over het algemeen over beveiligingstokens, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>U kunt kiezen voor dit veld markeren als een SecureString veilig opslaan in ADF of beveiligingstoken worden opgeslagen in Azure Sleutelkluis en de kopie vizier pull daar bij het uitvoeren van de gegevens opnieuw te kopiëren: meer informatie kunt [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Niet voor de gegevensbron, Ja voor sink als bron gekoppeld-service heeft geen IR |

>[!IMPORTANT]
>Als u gegevens **in** Salesforce, standaard Azure integratie Runtime kan niet worden gebruikt om uit te voeren kopiëren. In een ander woord als de bron gekoppeld service beschikt niet over een opgegeven IR expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) gekoppelde service als het volgende voorbeeld met een locatie in de buurt van uw Salesforce en koppelen in het Salesforce.

**Voorbeeld: opslaan van referenties in ADF**

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

**Voorbeeld: opslaan van referenties in Azure Sleutelkluis**

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Salesforce-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van/naar Salesforce, **SalesforceObject**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SalesforceObject**  | Ja |
| objectApiName | De objectnaam Salesforce gegevens ophalen van. | Er is geen voor bron Ja voor sink |

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

![Naam van de Data Factory - verbinding Salesforce - API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

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
>Voor de vorige compatibiliy bij het kopiëren van gegevens van Salesforce wordt met behulp van de vorige 'RelationalTable' type gegevensset blijven werken, terwijl u overschakelen naar het nieuwe 'SalesforceObject'-type worden voorgesteld.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in Salesforce. | Nee (als 'query' in de activiteitbron is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Salesforce-bron- en sink.

### <a name="salesforce-as-source"></a>SalesForce als bron

Om gegevens te kopiëren van Salesforce, stelt u het brontype in de kopieerbewerking naar **SalesforceSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SalesforceSource** | Ja |
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
>Voor de vorige compatibiliy bij het kopiëren van gegevens van Salesforce wordt met behulp van de vorige bron van het type kopiëren 'RelationalSource' blijven werken, terwijl u overschakelen naar het nieuwe 'SalesforceSource'-type worden voorgesteld.

### <a name="salesforce-as-sink"></a>SalesForce als sink

Om gegevens te kopiëren naar Salesforce, stelt u het sink-type in de kopieerbewerking naar **SalesforceSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **SalesforceSink** | Ja |
| WriteBehavior | Het gedrag schrijven voor de bewerking.<br/>Toegestane waarden zijn: **invoegen**, en **Upsert**. | Nee (de standaardwaarde is invoegen) |
| externalIdFieldName | De naam van het externe ID-veld voor upsert bewerking. Het opgegeven veld moet worden gedefinieerd als 'Externe Id Field' in het Salesforce-object en kan het geen NULL-waarden in de bijbehorende invoergegevens hebben. | Ja voor "Upsert" |
| writeBatchSize | Het aantal rijen van de gegevens die naar Salesforce zijn geschreven in elke batch. | Nee (de standaardwaarde is 5000) |
| ignoreNullValues | Hiermee wordt aangegeven of voor het negeren van null-waarden van invoer gegevens tijdens de schrijfbewerking.<br/>Toegestane waarden zijn: **true**, en **false**.<br>- **de waarde True**: laat de gegevens in de doel-object niet worden gewijzigd bij het uitvoeren van bewerking upsert/bijwerken en voeg standaardwaarde gedefinieerd bij het uitvoeren van insert-bewerking.<br/>- **ONWAAR**: de gegevens in het doelobject op NULL bijwerken bij het uitvoeren van bewerking upsert/bijwerken en voeg NULL-waarde bij het uitvoeren van insert-bewerking. | Nee (de standaardwaarde is ONWAAR) |

### <a name="example-salesforce-sink-in-copy-activity"></a>Voorbeeld: Salesforce sink in de kopieerbewerking

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

### <a name="retrieving-data-from-salesforce-report"></a>Ophalen van gegevens uit het Salesforce-rapport

U kunt gegevens ophalen uit het Salesforce-rapporten door te geven van de query als `{call "<report name>"}`. Voorbeeld: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Bij het ophalen van records verwijderd uit Salesforce Prullenbak

Om te vragen de voorlopig verwijderde records uit Salesforce-Prullenbak, kunt u **' IsDeleted = 1 "** in uw query. Bijvoorbeeld:

* Opgeven om te vragen alleen de verwijderde records, ' Selecteer * uit MyTable__c **waar IsDeleted = 1**'
* Opgeven om te vragen alle records met inbegrip van de bestaande en de verwijderde, ' Selecteer * uit MyTable__c **waar IsDeleted = 0 of IsDeleted = 1**'

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Bij het ophalen van gegevens met behulp van waar component voor datum/tijd-kolom

Geef bij de SOQL of SQL-query, let u op het verschil van datum/tijd-indeling. Bijvoorbeeld:

* **Voorbeeld van SOQL**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL-voorbeeld**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Toewijzing voor de Salesforce-gegevenstype

Bij het kopiëren van gegevens van Salesforce, worden de volgende toewijzingen van Salesforce gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| SalesForce-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Automatische getal |Tekenreeks |
| Selectievakje |Boole-waarde |
| Valuta |dubbele |
| Date |Datum en tijd |
| Datum/tijd |Datum en tijd |
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
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).