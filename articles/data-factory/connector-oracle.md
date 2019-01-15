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
ms.date: 11/21/2018
ms.author: jingwang
ms.openlocfilehash: 35c0d9190a11ad76ef44b43ef5160d2b39bee1fc
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016904"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Oracle met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-onprem-oracle-connector.md)
> * [Huidige versie](connector-oracle.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een Oracle-database. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een Oracle-database kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren naar een Oracle-database. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Oracle-connector de volgende versies van een Oracle-database. Het biedt ook ondersteuning voor Basic- of OID verificaties:

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

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
| connectionString | Hiermee geeft u de informatie die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md).<br><br>**Verbindingstype ondersteund**: U kunt **Oracle-SID** of **Oracle-servicenaam** voor het identificeren van uw database:<br>-Als u beveiligings-id: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Als u de naam van de Service: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!TIP]
>Als u fout uitspraak bereikt "ORA 01025: UPI parameter buiten het bereik' en de Oracle-is van het versie-8i, toevoegen `WireProtocolMode=1` met de verbindingstekenreeks en probeer het opnieuw.

**Versleuteling op Oracle-verbinding inschakelen**, hebt u twee opties:

1.  Gebruik **versleuteling Triple-DES (3DES) en Advanced Encryption Standard (AES)**, Oracle-serverzijde, gaat u naar Oracle Advanced Security (OAS) en de versleutelingsinstellingen configureren, Raadpleeg de details [hier](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759). ADF Oracle-connector wordt automatisch onderhandelt over de versleutelingsmethode voor het gebruik van de versie die u in OAS configureert bij het maken van verbinding met Oracle.

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

        **Voorbeeld:** maakt een PKCS12 trustsotre-bestand met de naam MyTrustStoreFile met een wachtwoord

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

Om gegevens te kopiëren van Oracle, stelt u het brontype in de kopieeractiviteit naar **OracleSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **OracleSource**. | Ja |
| oracleReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`. | Nee |

Als u geen 'oracleReaderQuery' opgeeft, de kolommen die zijn gedefinieerd in de sectie "structuur" van de gegevensset worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) op de Oracle-database uit te voeren. Als de definitie van de gegevensset geen "structuur", worden alle kolommen uit de tabel geselecteerd.

**Voorbeeld:**

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

### <a name="oracle-as-a-sink-type"></a>Oracle als een sink-type

Om gegevens te kopiëren naar Oracle, stelt u het sink-type in de kopieeractiviteit naar **OracleSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **OracleSink**. | Ja |
| WriteBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn gehele getallen (aantal rijen). |Nee (de standaardinstelling is 10.000) |
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

## <a name="data-type-mapping-for-oracle"></a>Toewijzing voor Oracle-gegevenstype

Wanneer u gegevens van en naar Oracle kopiëren, worden de volgende toewijzingen van Oracle-gegevenstypen gebruikt om Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe de kopieerbewerking het schema en de gegevens van een brontype aan de sink toegewezen, [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| Oracle-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| BBESTAND |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund op Oracle 10g en hoger) |
| CHAR |Reeks |
| CLOB |Reeks |
| DATE |DateTime |
| DRIJVENDE KOMMA |Decimaal getal, tekenreeks (als precisie > 28) |
| GEHEEL GETAL ZIJN |Decimaal getal, tekenreeks (als precisie > 28) |
| LANG |Reeks |
| LANGE ONBEWERKTE |Byte[] |
| NCHAR |Reeks |
| NCLOB |Reeks |
| AANTAL |Decimaal getal, tekenreeks (als precisie > 28) |
| NVARCHAR2 |Reeks |
| ONBEWERKTE |Byte[] |
| ROWID |Reeks |
| TIJDSTEMPEL |DateTime |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |Reeks |
| TIJDSTEMPEL MET TIJDZONE |Reeks |
| GEHEEL GETAL ZONDER TEKEN |Aantal |
| VARCHAR2 |Reeks |
| XML |Reeks |

> [!NOTE]
> De gegevenstypen INTERVAL jaar aan maand en het INTERVAL dag aan worden niet ten tweede ondersteund.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
