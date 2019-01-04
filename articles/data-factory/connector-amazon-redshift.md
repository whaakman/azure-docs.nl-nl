---
title: Gegevens kopiëren van Amazon Redshift met Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u gegevens kopiëren van Amazon Redshift naar ondersteunde sink-gegevensopslag met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 7c790d03143eece9b0c827a033bdd46bfd1a8f45
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024362"
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Gegevens kopiëren van Amazon Redshift met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-amazon-redshift-connector.md)
> * [Huidige versie](connector-amazon-redshift.md)


In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een Amazon Redshift. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van Amazon Redshift naar elke ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze Amazon Redshift-connector ondersteunt met name bij het ophalen van gegevens van Redshift met behulp van de query of ingebouwde ondersteuning voor het verwijderen van Redshift.

> [!TIP]
> Voor het bereiken van de beste prestaties bij het kopiëren van grote hoeveelheden gegevens van Redshift, kunt u overwegen de ingebouwde verwijderen van Redshift via Amazon S3. Zie [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](#use-unload-to-copy-data-from-amazon-redshift) sectie voor meer informatie.

## <a name="prerequisites"></a>Vereisten

* Als u kopieert de gegevens naar een on-premises gegevens opslaan met behulp van [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md), geef Integration Runtime (Gebruik IP-adres van de machine) de toegang tot Amazon Redshift-cluster. Zie [verleent toegang tot het cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) voor instructies.
* Als u gegevens naar een Azure data store kopieert, raadpleegt u [IP-bereiken van Azure Data Center](https://www.microsoft.com/download/details.aspx?id=41653) voor de Compute-IP-adres en de SQL-bereiken die worden gebruikt door de Azure data centers.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Amazon Redshift-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Amazon Redshift gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AmazonRedshift** | Ja |
| server |IP-adres of de hostnaam de naam van de Amazon Redshift-server. |Ja |
| poort |Het nummer van de TCP-poort die gebruikmaakt van de Amazon Redshift-server om te luisteren naar clientverbindingen. |Nee, de standaardwaarde is 5439 |
| database |De naam van de Amazon Redshift-database. |Ja |
| gebruikersnaam |De naam van de gebruiker die toegang tot de database heeft. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "<server name>",
            "database": "<database name>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Amazon Redshift-gegevensset.

Om gegevens te kopiëren van Amazon Redshift, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de Amazon Redshift. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "AmazonRedshiftDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Amazon Redshift linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Amazon Redshift-bron.

### <a name="amazon-redshift-as-source"></a>Amazon Redshift als bron

Om gegevens te kopiëren van Amazon Redshift, stelt u het brontype in de kopieeractiviteit naar **AmazonRedshiftSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AmazonRedshiftSource** | Ja |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Nee (als de 'tableName' in de gegevensset is opgegeven) |
| redshiftUnloadSettings | Eigenschappengroep bij het verwijderen van Amazon Redshift gebruik. | Nee |
| s3LinkedServiceName | Verwijst naar een Amazon S3 naar-worden gebruikt als een tijdelijke opslag door de naam van een gekoppelde service van het type 'Amazon S3' op te geven. | Ja, als uit het geheugen laden |
| bucketName | Geven de S3-bucket voor het opslaan van de tussentijdse gegevens. Als niet is opgegeven, Data Factory-service wordt automatisch gegenereerd.  | Ja, als uit het geheugen laden |

**Voorbeeld: Amazon Redshift-bron in de kopieeractiviteit middels de laden ongedaan maken**

```json
"source": {
    "type": "AmazonRedshiftSource",
    "query": "<SQL query>",
    "redshiftUnloadSettings": {
        "s3LinkedServiceName": {
            "referenceName": "<Amazon S3 linked service>",
            "type": "LinkedServiceReference"
        },
        "bucketName": "bucketForUnload"
    }
}
```

Meer informatie over het gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift efficiënt in de volgende sectie.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Laden ongedaan maken om gegevens te kopiëren van Amazon Redshift gebruiken

[GEHEUGEN](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) is een mechanisme dat wordt geleverd door Amazon Redshift, die de resultaten van een query op een of meer bestanden op de Amazon Simple Storage-Service (Amazon S3) kunt verwijderen. Dit is de aanbevolen door Amazon voor het kopiëren van grote gegevensset van Redshift manier.

**Voorbeeld: gegevens kopiëren van Amazon Redshift naar Azure SQL Data Warehouse laden ongedaan maken, met behulp van gefaseerd kopiëren en PolyBase**

Use-case voor dit voorbeeld, activiteit sessies gegevens kopiëren van Amazon Redshift naar Amazon S3 zoals geconfigureerd in 'redshiftUnloadSettings' en gegevens kopiëren van Amazon S3 naar Azure-Blob die zijn opgegeven in "stagingSettings", ten slotte gebruiken PolyBase om gegevens te laden in SQL-gegevens Het magazijn. De tijdelijke indeling wordt verwerkt door de kopieeractiviteit goed.

![Redshift naar SQL DW-exemplaar-werkstroom](media/copy-data-from-amazon-redshift/redshift-to-sql-dw-copy-workflow.png)

```json
"activities":[
    {
        "name": "CopyFromAmazonRedshiftToSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "AmazonRedshiftDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AmazonRedshiftSource",
                "query": "select * from MyTable",
                "redshiftUnloadSettings": {
                    "s3LinkedServiceName": {
                        "referenceName": "AmazonS3LinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "bucketName": "bucketForUnload"
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "AzureStorageLinkedService",
                "path": "adfstagingcopydata"
            },
            "dataIntegrationUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>De gegevenstypetoewijzing voor Amazon Redshift

Het kopiëren van gegevens van Amazon Redshift, worden de volgende toewijzingen van Amazon Redshift-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| Amazon Redshift-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BIGINT |Int64 |
| BOOLEAANSE WAARDE |Reeks |
| CHAR |Reeks |
| DATE |DateTime |
| DECIMAAL |Decimaal |
| DUBBELE PRECISIE |Double-waarde |
| GEHEEL GETAL ZIJN |Int32 |
| ECHTE |Enkelvoudig |
| SMALLINT |Int16 |
| TEKST |Reeks |
| TIJDSTEMPEL |DateTime |
| VARCHAR |Reeks |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
