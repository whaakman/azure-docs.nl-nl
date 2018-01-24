---
title: "Gegevens kopiëren van Amazon Redshift met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over hoe u gegevens vanaf Amazon Redshift kopiëren naar gegevensarchieven ondersteunde sink met behulp van Azure Data Factory."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 1e1c8e03bbfc2a07f4d4faee4c3b171c44fa312d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-amazon-redshift-using-azure-data-factory"></a>Gegevens kopiëren van Amazon Redshift met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-amazon-redshift-connector.md)
> * [Versie 2 - Preview](connector-amazon-redshift.md)


In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Redshift Amazon. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Amazon Redshift connnector in V1](v1/data-factory-amazon-redshift-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens vanaf Amazon Redshift kopiëren naar een ondersteunde sink data store. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Amazon Redshift ophalen van gegevens uit Redshift met behulp van de query of ingebouwde Redshift UNLOAD-ondersteuning.

> [!TIP]
> Als u wilt de beste prestaties bereiken bij het kopiëren van grote hoeveelheden gegevens van Redshift, kunt u overwegen de ingebouwde Redshift UNLOAD via Amazon S3. Zie [UNLOAD gebruiken om gegevens te kopiëren van Amazon Redshift](#use-unload-to-copy-data-from-amazon-redshift) sectie voor meer informatie.

## <a name="prerequisites"></a>Vereisten

* Als u kopieert de gegevens naar een on-premises gegevens opslaan met behulp van [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md), integratie Runtime (Gebruik IP-adres van de machine) de toegang verlenen tot Amazon Redshift cluster. Zie [toegang verlenen aan het cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) voor instructies.
* Als u gegevens naar een Azure data store kopiëren wilt, raadpleegt u [Azure Data Center-IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653) voor de Compute-IP-adres en de SQL-adresbereiken die worden gebruikt door de Azure data centers.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Amazon Redshift-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Amazon Redshift gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AmazonRedshift** | Ja |
| server |IP-adres of de hostnaam de naam van de server Amazon Redshift. |Ja |
| poort |Het nummer van de TCP-poort die de Amazon Redshift-server gebruikt om te luisteren naar verbindingen van clients. |Nee, de standaard 5439 is |
| database |De naam van de Amazon Redshift-database. |Ja |
| gebruikersnaam |Naam van de gebruiker die toegang tot de database heeft. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Amazon Redshift dataset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van Amazon Redshift, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de Amazon-Redshift. | Nee (als 'query' in de activiteitbron is opgegeven) |

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Amazon Redshift bron.

### <a name="amazon-redshift-as-source"></a>Amazon Redshift als bron

Om gegevens te kopiëren van Amazon Redshift, stelt u het brontype in de kopieerbewerking naar **AmazonRedshiftSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AmazonRedshiftSource** | Ja |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * from MijnTabel. |Nee (als 'tableName' in de gegevensset is opgegeven) |
| redshiftUnloadSettings | Eigenschappengroep bij gebruik van Amazon Redshift verwijderen. | Nee |
| s3LinkedServiceName | Verwijst naar een Amazon S3 naar-worden gebruikt als een tussentijdse store door te geven van een gekoppelde service-naam van het type 'AmazonS3'. | Ja als u laden ongedaan maken |
| bucketName | S3-bucket voor het opslaan van de tijdelijke gegevens aangeven. Als u niet is opgegeven, Data Factory-service wordt automatisch gegenereerd.  | Ja als u laden ongedaan maken |

**Voorbeeld: Redshift Amazon-bron in de kopieeractiviteit middels de verwijderen**

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

Meer informatie over het gebruik van UNLOAD gegevens gekopieerd van Amazon Redshift efficiënt uit de volgende sectie.

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>UNLOAD gebruiken om gegevens van Amazon Redshift kopiëren

[LADEN](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) is een mechanisme dat wordt geleverd door Amazon Redshift die u kunt de resultaten van een query naar een of meer bestanden op de Amazon eenvoudige Storage-Service (Amazon S3) niet laden. Dit is de aanbevolen door Amazon voor het kopiëren van grote gegevensset uit Redshift manier.

**Voorbeeld: gegevens kopiëren van Amazon Redshift naar Azure SQL Data Warehouse laden ongedaan maken, met voorbereide copy en PolyBase**

Voor dit voorbeeld gebruiken geval, activiteit verwijdering gegevens kopiëren van Amazon Redshift naar Amazon S3 zoals geconfigureerd in 'redshiftUnloadSettings' en gegevens kopiëren van Amazon S3 naar Azure Blob zoals opgegeven in 'stagingSettings' ten slotte gebruiken PolyBase gegevens laadt in SQL-gegevens Het magazijn. De tijdelijke indeling wordt verwerkt door de kopieeractiviteit goed.

![Redshift naar SQL DW kopie werkstroom](media\copy-data-from-amazon-redshift\redshift-to-sql-dw-copy-workflow.png)

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
            "cloudDataMovementUnits": 32
        }
    }
]
```

## <a name="data-type-mapping-for-amazon-redshift"></a>De gegevenstypetoewijzing voor Amazon Redshift

Bij het kopiëren van gegevens vanaf Amazon Redshift, worden de volgende toewijzingen van Amazon Redshift gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Amazon Redshift gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BIGINT |Int64 |
| BOOLEAANSE WAARDE |Tekenreeks |
| CHAR |Tekenreeks |
| DATE |Datum en tijd |
| DECIMAL |Decimale |
| DUBBELE PRECISIE |Double |
| GEHEEL GETAL |Int32 |
| ECHTE |Single |
| SMALLINT |Int16 |
| TEXT |Tekenreeks |
| TIJDSTEMPEL |Datum en tijd |
| VARCHAR |Tekenreeks |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
