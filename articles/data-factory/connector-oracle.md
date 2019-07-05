---
title: Gegevens kopiëren naar en van Oracle met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens van ondersteunde bron-archieven met een Oracle-database of van Oracle naar ondersteunde sink-archieven met behulp van Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: jingwang
ms.openlocfilehash: 04f623a889a87c325b1f53e3b39656ca4b703961
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509229"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Oracle met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-onprem-oracle-connector.md)
> * [Huidige versie](connector-oracle.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een Oracle-database. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een Oracle-database kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren naar een Oracle-database. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Oracle-connector:

- De volgende versies van een Oracle-database:
  - Oracle 12c R1 (12.1)
  - Oracle 11g R1, R2 (11.1, 11.2)
  - Oracle 10g R1, R2 (10.1, 10.2)
  - Oracle 9i R1, R2 (9.0.1, 9.2)
  - Oracle 8i R3 (8.1.7)
- Kopiëren van gegevens met **Basic** of **OID** verificaties.
- Parallelle kopie van Oracle-bron. Zie [parallelle kopie van Oracle](#parallel-copy-from-oracle) gedeelte met details.

> [!Note]
> Proxyserver voor Oracle wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van en naar een Oracle-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie voor meer informatie over integratieruntime [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md). De integratieruntime biedt een ingebouwd Oracle-stuurprogramma. Daarom moet u niet handmatig een stuurprogramma hebt geïnstalleerd wanneer u gegevens van en naar Oracle kopiëren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op de Oracle-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Oracle gekoppelde service.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Oracle**. | Ja |
| connectionString | Hiermee geeft u de informatie die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar. <br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook wachtwoord plaatsen in Azure Key Vault en pull de `password` configuratie buiten de verbindingsreeks. Raadpleeg de volgende voorbeelden en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. <br><br>**Verbindingstype ondersteund**: U kunt **Oracle-SID** of **Oracle-servicenaam** voor het identificeren van uw database:<br>-Als u beveiligings-id: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Als u de naam van de Service: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!TIP]
>Als u fout uitspraak bereikt "ORA 01025: UPI parameter buiten het bereik' en de Oracle-is van het versie-8i, toevoegen `WireProtocolMode=1` met de verbindingstekenreeks en probeer het opnieuw.

**Versleuteling op Oracle-verbinding inschakelen**, hebt u twee opties:

1.  Gebruik **versleuteling Triple-DES (3DES) en Advanced Encryption Standard (AES)** , Oracle-serverzijde, gaat u naar Oracle Advanced Security (OAS) en de versleutelingsinstellingen configureren, Raadpleeg de details [hier](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). ADF Oracle-connector wordt automatisch onderhandelt over de versleutelingsmethode voor het gebruik van de versie die u in OAS configureert bij het maken van verbinding met Oracle.

2.  Gebruik **SSL**, volg de onderstaande stappen:

    1.  Informatie over SSL-certificaat ophalen. Lees de informatie voor DER-gecodeerd certificaat van uw SSL-certificaat en sla de uitvoer (---Begin Certificate... Certificate---einde) als een tekstbestand.

        ```
        openssl x509 -inform DER -in [Full Path to the DER Certificate including the name of the DER Certificate] -text
        ```

        **Voorbeeld:** cert gegevens ophalen uit DERcert.cer; Sla de uitvoer op CERT

        ```
        openssl x509 -inform DER -in DERcert.cer -text
        Output:
        -----BEGIN CERTIFICATE-----
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        XXXXXXXXX
        -----END CERTIFICATE-----
        ```
    
    2.  De keystore of truststore maken. De volgende opdracht maakt u het bestand truststore met of zonder een wachtwoord in PKCS-12-indeling.

        ```
        openssl pkcs12 -in [Path to the file created in the previous step] -out [Path and name of TrustStore] -passout pass:[Keystore PWD] -nokeys -export
        ```

        **Voorbeeld:** maakt een PKCS12 truststore-bestand met de naam MyTrustStoreFile met een wachtwoord

        ```
        openssl pkcs12 -in cert.txt -out MyTrustStoreFile -passout pass:ThePWD -nokeys -export  
        ```

    3.  Plaats het bestand truststore op de machine zelfgehoste IR, bijvoorbeeld op C:\MyTrustStoreFile.
    4.  In ADF, configureert u de Oracle-verbindingsreeks met `EncryptionMethod=1` en de bijbehorende `TrustStore` / `TrustStorePassword`waarde, bijvoorbeeld `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;EncryptionMethod=1;TrustStore=C:\\MyTrustStoreFile;TrustStorePassword=<trust_store_password>`.

**Voorbeeld:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: wachtwoord opslaan in Azure Key Vault**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor de Oracle-gegevensset.

Als u wilt kopiëren van gegevens van en tot Oracle, stel de eigenschap type van de gegevensset in **OracleTable**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **OracleTable**. | Ja |
| tableName |De naam van de tabel in de Oracle-database waarnaar de gekoppelde service verwijst. | Ja |

**Voorbeeld:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Oracle-bron en sink.

### <a name="oracle-as-a-source-type"></a>Oracle als een brontype

> [!TIP]
>
> Meer informatie uit [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie over het laden van gegevens uit Oracle efficiënt gebruik maakt van partitioneren van gegevens.

Om gegevens te kopiëren van Oracle, stelt u het brontype in de kopieeractiviteit naar **OracleSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **OracleSource**. | Ja |
| oracleReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`.<br>Wanneer u gepartitioneerde laden inschakelen, moet u de bijbehorende ingebouwde partitie parameter (s) van een toepassing aansluiten in uw query. Zie de voorbeelden in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie. | Nee |
| partitionOptions | Hiermee geeft u de gegevens partitioneren van de opties die worden gebruikt om gegevens te laden van Oracle. <br>Toestaan dat waarden zijn: **Geen** (standaard), **PhysicalPartitionsOfTable** en **DynamicRange**.<br>Wanneer de partitieoptie is ingeschakeld (niet ' None'), ook Configureer **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** instellen op kopieeractiviteit bijvoorbeeld als 4, dat bepaalt de mate van parallelle gelijktijdig om gegevens te laden van Oracle de database. | Nee |
| partitionSettings | Geef de groep van de instellingen voor het partitioneren van gegevens. <br>Wanneer de partitieoptie is niet van toepassing `None`. | Nee |
| partitionNames | De lijst met fysieke partities die moeten worden gekopieerd. <br>Van toepassing wanneer de partitieoptie is `PhysicalPartitionsOfTable`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfTabularPartitionName` WHERE-component. Zie het voorbeeld in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie. | Nee |
| partitionColumnName | Geef de naam van de bronkolom **in het type geheel getal** dat wordt gebruikt door het bereik voor parallelle kopie partitioneren. Als niet is opgegeven, worden de primaire sleutel van de tabel automatisch gedetecteerd en gebruikt als partitiekolom. <br>Van toepassing wanneer de partitieoptie is `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfRangePartitionColumnName` WHERE-component. Zie het voorbeeld in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie. | Nee |
| partitionUpperBound | Maximale waarde van de partitiekolom kopiëren van gegevens. <br>Van toepassing wanneer de partitieoptie is `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfRangePartitionUpbound` WHERE-component. Zie het voorbeeld in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie. | Nee |
| PartitionLowerBound | De minimumwaarde van de partitiekolom om gegevens te kopiëren uit. <br>Van toepassing wanneer de partitieoptie is `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfRangePartitionLowbound` WHERE-component. Zie het voorbeeld in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie. | Nee |

**Voorbeeld: gegevens met behulp van eenvoudige query uitvoert zonder partitie kopiëren**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Meer voorbeelden in [parallelle kopie van Oracle](#parallel-copy-from-oracle) sectie.

### <a name="oracle-as-a-sink-type"></a>Oracle als een sink-type

Om gegevens te kopiëren naar Oracle, stelt u het sink-type in de kopieeractiviteit naar **OracleSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **OracleSink**. | Ja |
| writeBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn gehele getallen (aantal rijen). |Nee (de standaardinstelling is 10.000) |
| writeBatchTimeout | Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn Timespan. Een voorbeeld is 00:30:00 uur (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in Oracle in elke uitvoering. U kunt deze eigenschap gebruiken voor het opschonen van de vooraf geladen gegevens. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="parallel-copy-from-oracle"></a>Parallelle kopie van Oracle

Data factory Oracle-connector biedt ingebouwde gegevens partitioneren om gegevens te kopiëren van Oracle in combinatie met de uitstekende prestaties. Gegevens partitioneren op kopieeractiviteit -> Oracle-bron, kunt u vinden:

![Opties voor partitioneren](./media/connector-oracle/connector-oracle-partition-options.png)

Wanneer u gepartitioneerde kopiëren inschakelt, wordt data factory parallelle query's uitgevoerd op de Oracle-gegevensbron om gegevens te laden door partities. De mate van parallelle is geconfigureerd en beheerd **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** instellen voor de kopieeractiviteit. Als u bijvoorbeeld `parallelCopies` als vier, data factory gelijktijdig genereert en vier query's uitgevoerd op basis van de opgegeven partitieoptie en instellingen voor elk gedeelte bij het ophalen van gegevens uit uw Oracle-database.

U worden voorgesteld om in te schakelen parallelle kopie met gegevens partitioneren met name wanneer u grote hoeveelheden gegevens uit Oracle-database laden. Hier volgen de aanbevolen configuraties voor verschillende scenario's:

| Scenario                                                     | Aanbevolen instellingen                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Volledig laden van grote tabel met fysieke partities          | **Partitie-optie**: Fysieke partities van de tabel. <br><br/>Tijdens de uitvoering, data Factory automatisch detecteren van de fysieke partities en gegevens kopiëren met partities. |
| Volledig laden van grote tabel zonder fysieke partities terwijl met een kolom met gehele getallen voor het partitioneren van gegevens | **Opties voor partitioneren**: Dynamisch Bereikpartitie.<br>**Partitiekolom**: Geef op de kolom die wordt gebruikt om gegevens te partitioneren. Als dit niet opgeeft, primaire-sleutelkolom wordt gebruikt. |
| Laden van grote hoeveelheden gegevens met behulp van aangepaste query onder met fysieke partities | **Partitie-optie**: Fysieke partities van de tabel.<br>**Query**: `SELECT * FROM <TABLENAME> PARTITION("?AdfTabularPartitionName") WHERE <your_additional_where_clause>`.<br>**Naam van de partitie**: Geef de namen van de partitie te kopiëren van gegevens uit. Indien niet opgegeven, worden de fysieke partities op de tabel die u hebt opgegeven in de Oracle-gegevensset automatisch gedetecteerd door ADF.<br><br>Bij het uitvoeren van data factory vervangen `?AdfTabularPartitionName` met de naam van de werkelijke partitie en verzenden naar Oracle. |
| Laden van grote hoeveelheden gegevens met behulp van aangepaste query, onder zonder fysieke partities terwijl met een kolom met gehele getallen voor het partitioneren van gegevens | **Opties voor partitioneren**: Dynamisch Bereikpartitie.<br>**Query**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Partitiekolom**: Geef op de kolom die wordt gebruikt om gegevens te partitioneren. U kunt partitioneren op basis van de kolom met gegevenstype integer.<br>**Partitie bovengrens** en **partitie ondergrens**: Als u wilt filteren op basis van de partitiekolom alleen ophalen van gegevens tussen de boven- en ondergrenzen bereik opgeven<br><br>Bij het uitvoeren van data factory vervangen `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound`, en `?AdfRangePartitionLowbound` met de kolomnaam van de werkelijke en waarde bereiken voor elke partitie en verzenden naar Oracle. <br>Bijvoorbeeld, als uw partitie kolom-ID ingesteld met ondergrens als 1 en bovengrens als 80, met parallelle kopie instellen als 4, ADF ophalen van gegevens door 4 partities met de ID tussen [1,20], [21, 40], [41, 60] en [61, 80]. |

**Voorbeeld: query's uitvoeren met fysieke partitie**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> PARTITION(\"?AdfTabularPartitionName\") WHERE <your_additional_where_clause>",
    "partitionOption": "PhysicalPartitionsOfTable",
    "partitionSettings": {
        "partitionNames": [
            "<partitionA_name>",
            "<partitionB_name>"
        ]
    }
}
```

**Voorbeeld: query's uitvoeren met dynamisch Bereikpartitie**

```json
"source": {
    "type": "OracleSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-oracle"></a>Toewijzing voor Oracle-gegevenstype

Wanneer u gegevens van en naar Oracle kopiëren, worden de volgende toewijzingen van Oracle-gegevenstypen gebruikt om Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe de kopieerbewerking het schema en de gegevens van een brontype aan de sink toegewezen, [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| Oracle-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund op Oracle 10g en hoger) |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimal, String (als precisie > 28) |
| INTEGER |Decimal, String (als precisie > 28) |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Decimal, String (als precisie > 28) |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |String |
| TIMESTAMP WITH TIME ZONE |String |
| UNSIGNED INTEGER |Number |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> De gegevenstypen INTERVAL jaar aan maand en het INTERVAL dag aan worden niet ten tweede ondersteund.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
