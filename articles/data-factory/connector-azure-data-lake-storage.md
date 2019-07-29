---
title: Gegevens kopiëren naar of van Azure Data Lake Storage Gen2 met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar en van Azure Data Lake Storage Gen2 met behulp van Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 9f60c6258da77c0aaa99d16e178f4b3531ce90d9
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509250"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Lake Storage Gen2 met Azure Data Factory

Azure Data Lake Storage Gen2 is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data die is ingebouwd in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). U kunt deze gebruiken om met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag.

In dit artikel bevat een overzicht over het kopiëren van gegevens naar en van Azure Data Lake Storage Gen2. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze connector Azure Data Lake Storage Gen2 wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [bron- of sinkservice matrix ondersteund](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Met deze connector kunt u met name:

- Gegevens kopiëren met behulp van de sleutel, service-principal of beheerde identiteiten van Azure-resources-verificaties.
- Kopiëren van bestanden of parseren of bestanden met genereren [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

>[!TIP]
>Als u de hiërarchische naamruimte inschakelt, bestaat er momenteel geen interoperabiliteit van bewerkingen tussen de Blob en Data Lake Storage Gen2 API's. Als u de fout ' ErrorCode FilesystemNotFound = "van het bericht 'het opgegeven bestandssysteem does not exist', dit wordt veroorzaakt door het opgegeven sink-bestandssysteem dat is gemaakt via de Blob-API in plaats van de API van Data Lake Storage Gen2 elders. Los het probleem, geeft u een nieuwe bestandssysteem met een naam die als de naam van een Blob-container bestaat niet. Data Factory maakt die bestandssysteem vervolgens automatisch tijdens het kopiëren van gegevens.

>[!NOTE]
>Als u inschakelt de **vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan** optie in de firewallinstellingen van Azure Storage, Azure integratieruntime niet verbinden met Data Lake Storage Gen2 en ziet u een niet-toegestane fout. Het foutbericht wordt weergegeven, omdat de Data Factory wordt niet beschouwd als een vertrouwde Microsoft-service. Gebruik de zelf-hostende integratieruntime om in plaats daarvan.

## <a name="get-started"></a>Aan de slag

>[!TIP]
>Zie voor een overzicht van het gebruik van de connector Data Lake Storage Gen2 [gegevens laden in Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Data Lake Storage Gen2.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De Azure Data Lake Storage Gen2-connector ondersteunt de volgende verificatietypen. Zie de betreffende secties voor meer informatie:

- [Verificatie van account-sleutel](#account-key-authentication)
- [Verificatie van service-principal](#service-principal-authentication)
- [Beheerde identiteiten voor verificatie van de Azure-resources](#managed-identity)

>[!NOTE]
>Bij het gebruik van PolyBase om gegevens te laden in SQL Data Warehouse, als uw Data Lake Storage Gen2-bron is geconfigureerd met het eindpunt voor Virtueelnetwerk, moet u verificatie van de beheerde identiteit zoals vereist door PolyBase gebruiken. Zie de [verificatie van de beheerde identiteit](#managed-identity) sectie met meer configuratievereisten.

### <a name="account-key-authentication"></a>Verificatie van account-sleutel

Voor het gebruik van storage-account sleutelverificatie, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| accountKey | Accountsleutel voor het Data Lake Storage Gen2. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure integratieruntime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief zich in een particulier netwerk. Als deze eigenschap niet is opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Volg deze stappen voor het gebruik van service-principal verificatie.

1. Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) met de volgende stappen in [uw toepassing registreren bij een Azure AD-tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

2. De service principal juiste machtiging verlenen. Meer informatie over de werking van machtiging in Data Lake Storage Gen2 van [toegangsbeheerlijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)

    - **Als bron**: In Storage Explorer verlenen ten minste **Execute** machtiging starten vanaf de bron-bestandssysteem, samen met **lezen** machtiging voor de bestanden te kopiëren. U kunt ook in Access control (IAM), verleent u ten minste de **gegevenslezer voor Opslagblob** rol.
    - **Als sink**: In Storage Explorer verlenen ten minste **Execute** machtiging vanaf het sink-bestandssysteem, samen met **schrijven** machtiging voor de sink-map. U kunt ook in Access control (IAM), verleent u ten minste de **Gegevensbijdrager voor Blob** rol.

>[!NOTE]
>Aan de lijst met mappen vanuit het accountniveau, of om verbinding te testen, moet u de machtiging van de service-principal wordt verleend aan instellen **storage-account met de machtiging 'Gegevenslezer voor Opslagblob' in IAM**. Dit geldt wanneer u de:
>- **Het kopieerprogramma van gegevens** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen. 
>Als u zich zorgen maakt over het verlenen van machtiging op het accountniveau tijdens het ontwerpen, gaat u verder testen verbinding en kies vervolgens een bovenliggend pad met een machtiging verleend om te bladeren van invoer die opgegeven pad. Kopieer activiteit werkt, zolang de service-principal is verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

Deze eigenschappen worden ondersteund voor de gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een `SecureString` Bewaar deze zorgvuldig in Data Factory. U kunt [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure integratieruntime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief zich in een particulier netwerk. Indien niet opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Beheerde identiteiten voor verificatie van de Azure-resources

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md), die staat voor deze specifieke data factory. U kunt deze beheerde identiteit rechtstreeks gebruiken voor verificatie met Data Lake Storage Gen2, vergelijkbaar met het gebruik van uw eigen service-principal. Kan deze aangewezen factory toegang en Kopieer gegevens naar of van uw Data Lake Storage Gen2.

Volg deze stappen voor het gebruik van beheerde identiteiten voor verificatie van de Azure-resource.

1. [Ophalen van de identiteitsgegevens van de Data Factory beheerd](data-factory-service-identity.md#retrieve-managed-identity) door te kopiëren van de waarde van de **identiteitstoepassings-ID-service** gegenereerd samen met uw gegevensfactory.

2. Verleen de juiste machtiging beheerde identiteit. Meer informatie over de werking van machtiging in Data Lake Storage Gen2 van [toegangsbeheerlijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

    - **Als bron**: In Storage Explorer verlenen ten minste **Execute** machtiging starten vanaf de bron-bestandssysteem, samen met **lezen** machtiging voor de bestanden te kopiëren. U kunt ook in Access control (IAM), verleent u ten minste de **gegevenslezer voor Opslagblob** rol.
    - **Als sink**: In Storage Explorer verlenen ten minste **Execute** machtiging vanaf het sink-bestandssysteem, samen met **schrijven** machtiging voor de sink-map. U kunt ook in Access control (IAM), verleent u ten minste de **Gegevensbijdrager voor Blob** rol.

>[!NOTE]
>Aan de lijst met mappen vanuit het accountniveau, of om verbinding te testen, moet u de machtiging van de beheerde identiteit wordt verleend aan instellen **storage-account met de machtiging 'Gegevenslezer voor Opslagblob' in IAM**. Dit geldt wanneer u de:
>- **Het kopieerprogramma van gegevens** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen. 
>Als u zich zorgen maakt over het verlenen van machtiging op het accountniveau tijdens het ontwerpen, gaat u verder testen verbinding en kies vervolgens een bovenliggend pad met een machtiging verleend om te bladeren van invoer die opgegeven pad. Kopieer activiteit werkt, zolang de service-principal is verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

>[!IMPORTANT]
>Als u PolyBase om gegevens te laden uit Data Lake Storage Gen2 in SQL Data Warehouse bij het gebruik van verificatie van de beheerde identiteit voor Data Lake Storage Gen2 gebruikt, zorg ervoor dat u ook stappen 1 en 2 in [deze richtlijnen](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) op 1) uw SQL registreren Database-server met Azure Active Directory (Azure AD) en 2) toewijzen de Gegevensbijdrager voor Blob-rol aan uw SQL-Database-server. de rest worden verwerkt door Data Factory. Als uw Data Lake Storage Gen2 is geconfigureerd met een Azure Virtual Network-eindpunt, voor het gebruik van PolyBase om gegevens te laden, moet u verificatie van de beheerde identiteit zoals vereist door PolyBase.

Deze eigenschappen worden ondersteund voor de gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure integratieruntime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief zich in een particulier netwerk. Indien niet opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, [gegevenssets](concepts-datasets-linked-services.md).

- Zie voor de parquet en gescheiden tekstopmaak de [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Zie voor andere indelingen, zoals ORC, Avro, JSON of binaire indeling, de [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Als u wilt kopiëren van en naar Data Lake Storage Gen2 gegevens in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikelen op de gegevensset op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `location` instellingen in de gegevensset op basis van een indeling:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **AzureBlobFSLocation**. | Ja      |
| fileSystem | De naam van Data Lake Storage Gen2 bestand system.                              | Nee       |
| folderPath | Het pad naar een map onder het opgegeven bestand-systeem. Als u wilt een jokerteken gebruiken om te filteren worden de mappen overslaan van deze instelling en deze opgeven in de instellingen voor de bron. | Nee       |
| fileName   | De bestandsnaam van het onder de opgegeven bestandssysteem + folderPath. Als u een jokerteken gebruiken om te filteren van bestanden wilt, overslaan van deze instelling en deze opgeven in de instellingen voor de bron. | Nee       |

> [!NOTE]
> De **AzureBlobFSFile** type gegevensset met de indeling van parquet of tekst vermeld in de volgende sectie wordt nog steeds ondersteund als kopiëren, lookup of de activiteit GetMetadata voor achterwaartse compatibiliteit. Maar dit niet werkt met de functie voor toewijzing gegevensstroom. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Andere indeling-gegevensset

Om gegevens te kopiëren naar en van Data Lake Storage Gen2 in ORC, Avro, JSON of binaire indeling, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureBlobFSFile**. |Ja |
| folderPath | Pad naar de map in Data Lake Storage Gen2. Indien niet opgegeven, wordt deze verwijst naar de hoofdmap. <br/><br/>Filteren op jokerteken wordt ondersteund. Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken is of deze escape-teken in. <br/><br/>Voorbeelden: bestandssysteem/map /. Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Nee |
| fileName | Naam of het jokerteken filter voor de bestanden in de opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter, de jokertekens toegestaan zijn `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke een jokerteken heeft of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ', bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'. Als u van een tabellair bron in plaats van een query met behulp van een tabelnaam wordt opgegeven kopieert, wordt het naampatroon is ' *[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ', bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br/><br/> De algehele prestaties van de verplaatsing van gegevens wordt beïnvloed door deze instelling is ingeschakeld als u wilt filter het bestand met grote hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br/><br/> De algehele prestaties van de verplaatsing van gegevens wordt beïnvloed door deze instelling is ingeschakeld als u wilt filter het bestand met grote hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd.| Nee |
| format | Als u wilt kopiëren van bestanden als tussen winkels op basis van bestanden (binaire kopie), moet u de sectie indeling in de invoer en uitvoer gegevenssetdefinities overslaan.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [ORC-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling ](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimale** en **snelst**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Als u wilt kopiëren van één bestand met een specifieke naam, geef **folderPath** met een maponderdeel en **fileName** met een bestandsnaam op.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met een maponderdeel en **fileName** met een wildcard-filter. 

**Voorbeeld:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, [kopiëren activiteitsconfiguraties](copy-activity-overview.md#configuration) en [pijplijnen en activiteiten](concepts-pipelines-activities.md). Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Data Lake Storage Gen2 bron en sink.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 als een brontype

- Als u wilt kopiëren uit de parquet of gescheiden tekstopmaak, Zie de [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Als u wilt kopiëren van andere indelingen, zoals ORC, Avro, JSON of binaire indeling, Zie de [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Als u wilt kopiëren van gegevens van Data Lake Storage Gen2 in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens onder het opgegeven bestandssysteem dat is geconfigureerd in de gegevensset aan filter bronmappen. <br>Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder het opgegeven bestand system + folderPath/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken of deze escape-teken in. Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het laatste kenmerk gewijzigd is groter dan of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor de parquet of gescheiden tekstopmaak, de **AzureBlobFSSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als voor achterwaartse compatibiliteit. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobFSReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>De bron van andere indeling

Als u wilt kopiëren van gegevens van Data Lake Storage Gen2 in ORC, Avro, JSON of binaire indeling, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **AzureBlobFSSource**. |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink.<br/>Toegestane waarden zijn **waar** (standaard) en **false**. | Nee |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 als een sink-type

- Als u wilt kopiëren naar de parquet of gescheiden tekstopmaak, Zie de [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Als u wilt kopiëren naar andere indelingen, zoals ORC, Avro, JSON of binaire indeling, Zie de [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Om gegevens te kopiëren naar Data Lake Storage Gen2 in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikelen op kopiëren-indeling op basis van activiteit-sink- en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `storeSettings` instellingen in op basis van opmaak kopiëren sink:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor de bronmap is identiek aan het relatieve pad van de doel-bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor de parquet of gescheiden tekstopmaak, de **AzureBlobFSSink** type activiteit-sink voor exemplaar vermeld in de volgende sectie wordt nog steeds ondersteund als voor achterwaartse compatibiliteit. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobFSWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Andere indeling-sink

Om gegevens te kopiëren naar Data Lake Storage Gen2 in ORC, Avro, JSON of binaire indeling, de volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **AzureBlobFSSink**. |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor de bronmap is identiek aan het relatieve pad van de doel-bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName | recursive | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (Leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Enkele voorbeelden van recursieve en copyBehavior

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursive | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + File5 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>ACL's van Data Lake Storage Gen1 behouden

>[!TIP]
>Om gegevens te kopiëren van Azure Data Lake Storage Gen1 in Gen2 in het algemeen, Zie [gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2 met Azure Data Factory](load-azure-data-lake-storage-gen2-from-gen1.md) voor een overzicht en best practices.

Wanneer u bestanden van Azure Data Lake Storage Gen1 naar Gen2 kopiëren, kunt u kiezen of u de POSIX toegangsbeheerlijsten (ACL's) samen met gegevens behouden. Zie voor meer informatie over toegangsbeheer [toegangsbeheer in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md) en [toegangsbeheer in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

De volgende typen ACL's kunnen worden bewaard met behulp van de kopieeractiviteit in Azure Data Factory. U kunt een of meer typen selecteren:

- **ACL**: Kopieer en POSIX ACL's voor bestanden en mappen behouden. De volledige bestaande ACL's worden gekopieerd van bron naar een sink. 
- **De eigenaar van**: Kopieer en behouden van de gebruiker die eigenaar is van bestanden en mappen. Supergebruiker toegang tot Data Lake Storage Gen2 sink is vereist.
- **Groep**: Kopieer en behouden van de groep die eigenaar is van bestanden en mappen. Supergebruiker toegang tot het sink-Data Lake Storage Gen2 of de gebruiker die eigenaar is (als de gebruiker die eigenaar ook lid is van de doelgroep is) is vereist.

Als u opgeeft als u wilt kopiëren uit een map, Data Factory de ACL's voor die opgegeven map en de bestanden en mappen, repliceert als `recursive` is ingesteld op true. Als u opgeeft als u wilt kopiëren van een enkel bestand, wordt de ACL's op dat bestand worden gekopieerd.

>[!IMPORTANT]
>Als u behouden van ACL's wilt, zorg er dan voor dat u hiervoor toestemming hoog genoeg voor Data Factory om te werken op basis van uw sink Gen2 van Data Lake Storage-account. Bijvoorbeeld, gebruik van verificatie van account-sleutel of de eigenaar van gegevens van Storage Blob-rol toewijzen aan de service-principal of beheerde identiteit.

Wanneer u als Data Lake Storage Gen1 met de optie binaire kopiëren of de binaire indeling en de sink als Data Lake Storage Gen2 met de optie binaire kopiëren of de binaire indeling configureren, vindt u de **behouden** kiezen op de **kopiëren Instellingen van het hulpprogramma Data** pagina of op de **Kopieeractiviteit** > **instellingen** tabblad voor het ontwerpen van de activiteit.

![Data Lake Storage Gen1 naar Gen2 behouden ACL](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

Hier volgt een voorbeeld van JSON-configuratie (Zie `preserve`): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            },
            "preserve": [
                "ACL",
                "Owner",
                "Group"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen1 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="mapping-data-flow-properties"></a>Eigenschappen van de gebruikersstroom toewijzing van gegevens

Meer informatie over [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de functie voor toewijzing gegevensstroom.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
