---
title: Gegevens kopiëren naar of van Azure Data Lake Storage Gen2 met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens naar en van Azure Data Lake Storage Gen2 met Azure Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jingwang
ms.openlocfilehash: 355f61d6282c822e18cf4752044c1e1a5cbbc6a0
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560783"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Lake Storage Gen2 met Azure Data Factory

Azure Data Lake Storage Gen2 (ADLS Gen2) is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data, die is ingebouwd in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). Hiermee kunt u samenwerken met uw gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag.

In dit artikel bevat een overzicht over het kopiëren van gegevens naar en van Azure Data Lake Storage Gen2. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze connector Azure Data Lake Storage Gen2 wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Deze connector ondersteunt name:

- Kopiëren van gegevens met behulp van de sleutel, service-principal of beheerde identiteiten van Azure-resources-verificaties.
- Kopiëren van bestanden als- of parseren of genereren van bestanden met [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

>[!TIP]
>Als u de hiërarchische naamruimte inschakelt, bestaat er momenteel geen interoperabiliteit van bewerkingen tussen de Blob en ADLS Gen2 API's. Als u de fout van bereikt "ErrorCode FilesystemNotFound =" met het gedetailleerde bericht als "het opgegeven bestandssysteem bestaat niet.", dit wordt veroorzaakt door de opgegeven sink bestandssysteem is gemaakt via de Blob-API in plaats van ADLS Gen2 API elders. Los het probleem, Geef een nieuwe bestandssysteem met een naam die niet als de naam van een Blob-container bestaat en ADF maakt automatisch dit bestandssysteem tijdens het kopiëren van gegevens.

>[!NOTE]
>Als u Hiermee _'Vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan'_ optie voor Azure Storage firewall-instellingen, met behulp van Azure Integration Runtime verbinding maken met Data Lake Storage Gen2 mislukken met niet-toegestane fout, als ADF worden niet behandeld als vertrouwde Microsoft-service. Gebruik zelfgehoste Cloudintegratieruntime zoals verbinding maken via in plaats daarvan.

## <a name="get-started"></a>Aan de slag

>[!TIP]
>Zie voor een overzicht van het gebruik van Data Lake Storage Gen2 connector [gegevens laden in Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Data Lake Storage Gen2.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

Azure Data Lake Storage Gen2 connector ondersteunt de volgende verificatietypen, verwijzen naar de overeenkomstige sectie voor meer informatie:

- [Verificatie van account-sleutel](#account-key-authentication)
- [Verificatie van service-principal](#service-principal-authentication)
- [Beheerde identiteiten voor verificatie van de Azure-resources](#managed-identity)

### <a name="account-key-authentication"></a>Verificatie van account-sleutel

Voor het gebruik van storage-account sleutelverificatie, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor de Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| accountKey | Accountsleutel voor de Data Lake Storage Gen2-service. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

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

Volg deze stappen voor het gebruik van service-principal verificatie:

1. Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) door [uw toepassing registreren bij een Azure AD-tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

2. De service principal juiste machtiging verlenen.

    - **Als bron**, in Storage Explorer verlenen ten minste **lees- en uitvoeringsmachtigingen** toestemming om te lijst en kopieer de bestanden in mappen en submappen of verlenen **lezen** machtiging voor het kopiëren van een enkel bestand. U kunt ook in de Access control (IAM), verleent ten minste **gegevenslezer voor Opslagblob** rol.
    - **Als sink**, in Storage Explorer verlenen ten minste **schrijven + uitvoeren** machtiging voor het maken van onderliggende items in de map. U kunt ook in de Access control (IAM), verleent ten minste **Gegevensbijdrager voor Blob** rol.

>[!NOTE]
>Aan de lijst met mappen vanuit het accountniveau, of om verbinding te testen, moet u de machtiging van de service-principal wordt verleend aan instellen **storage-account met de machtiging 'Uitvoeren' in de IAM**. Dit geldt wanneer u de:
>- **Data-hulpprogramma voor kopiëren** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen. 
>Als u de bezorgdheid over het verlenen van toestemming op accountniveau hebt, kunt u testverbinding en invoerpad handmatig overslaan tijdens het ontwerpen. Kopieeractiviteit zullen nog steeds werken, zolang de service-principal is verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

Deze eigenschappen worden ondersteund in de gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor de Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

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

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md), die staat voor deze specifieke data factory. U kunt deze beheerde identiteit rechtstreeks gebruiken voor verificatie van ADLS Gen2 vergelijkbaar met het gebruik van uw eigen service-principal. Hiermee kunt deze aangewezen factory toegang en gegevens kopiëren van/naar uw ADLS-Gen2.

Voor het gebruik van beheerde identiteiten voor verificatie van de Azure-resources, de volgende stappen uit:

1. [Ophalen van gegevens van data factory beheerde identiteit](data-factory-service-identity.md#retrieve-managed-identity) door de waarde van "SERVICE-identiteit TOEPASSINGS-ID gegenereerd samen met uw factory kopiëren.

2. Verleen de juiste machtiging beheerde identiteit. 

    - **Als bron**, in Storage Explorer verlenen ten minste **lees- en uitvoeringsmachtigingen** toestemming om te lijst en kopieer de bestanden in mappen en submappen of verlenen **lezen** machtiging voor het kopiëren van een enkel bestand. U kunt ook in de Access control (IAM), verleent ten minste **gegevenslezer voor Opslagblob** rol.
    - **Als sink**, in Storage Explorer verlenen ten minste **schrijven + uitvoeren** machtiging voor het maken van onderliggende items in de map. U kunt ook in de Access control (IAM), verleent ten minste **Gegevensbijdrager voor Blob** rol.

>[!NOTE]
>Aan de lijst met mappen vanuit het accountniveau, of om verbinding te testen, moet u de machtiging van de beheerde identiteit wordt verleend aan instellen **storage-account met de machtiging 'Uitvoeren' in de IAM**. Dit geldt wanneer u de:
>- **Data-hulpprogramma voor kopiëren** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen. 
>Als u de bezorgdheid over het verlenen van toestemming op accountniveau hebt, kunt u testverbinding en invoerpad handmatig overslaan tijdens het ontwerpen. Kopieeractiviteit zullen nog steeds werken, zolang de beheerde identiteit wordt verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

Deze eigenschappen worden ondersteund in de gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| url | Eindpunt voor de Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net`. | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. 

- Voor **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Voor andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Het kopiëren van gegevens naar en van ADLS Gen2 in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor ADLS Gen2 onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **AzureBlobFSLocation**. | Ja      |
| fileSystem | De systeemnaam van ADLS Gen2-bestand.                              | Nee       |
| folderPath | Het pad naar map onder het opgegeven bestand-systeem. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De bestandsnaam van het onder de opgegeven bestandssysteem + folderPath. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
> **AzureBlobFSFile** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup/de GET metadata activity voor compatibiliteit met eerdere versies, maar werkt niet met gegevensstroom toewijzen. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen2 linked service name>",
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

Het kopiëren van gegevens naar en van ADLS Gen2 in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureBlobFSFile**. |Ja |
| folderPath | Pad naar de map in de Data Lake Storage Gen2. Indien niet opgegeven, wordt deze verwijst naar de hoofdmap. <br/><br/>Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: bestandssysteem/map/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Nee |
| fileName | **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ", bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'; Als u vanuit in tabelvorm bron met de tabelnaam van de in plaats van de query kopieert, het naampatroon is '*[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ", bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt kopiëren van bestanden als tussen winkels op basis van bestanden (binaire kopie), moet u de sectie indeling in de invoer en uitvoer gegevenssetdefinities overslaan.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling ](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimale** en **snelst**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken. 

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [kopiëren activiteitsconfiguraties](copy-activity-overview.md#configuration) en [pijplijnen en activiteiten](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Data Lake Storage Gen2 bron en sink.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake-opslag Gen2 als een brontype

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens van ADLS Gen2 in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor ADLS Gen2 onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens onder het opgegeven bestandssysteem dat is geconfigureerd in de gegevensset aan filter bronmappen. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder het opgegeven bestand system + folderPath/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in.  Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **AzureBlobFSSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

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

Het kopiëren van gegevens van ADLS Gen2 in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **AzureBlobFSSource**. |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink.<br/>Toegestane waarden zijn **waar** (standaard) en **false**. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

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

- Voor kopiëren naar **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Voor het kopiëren naar andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Gegevens kopiëren naar Gen2 in ADLS **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel voor kopiëren-indeling op basis van activiteit-sink en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor ADLS Gen2 onder `storeSettings` instellingen in op basis van opmaak kopiëren sink:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **AzureBlobFSSink** type activiteit-sink voor kopiëren die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

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

Gegevens kopiëren naar Gen2 in ADLS **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **AzureBlobFSSink**. |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

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
| `Folder*` | (leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Enkele voorbeelden van recursieve en copyBehavior

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursive | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + File5 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>ACL's van Data Lake Storage Gen1 behouden

>[!TIP]
>Voor gegevens uit Azure Data Lake Storage Gen1 in Gen2 in het algemeen Zie [gegevens kopiëren van Azure Data Lake Storage Gen1 naar Gen2 met Azure Data Factory](load-azure-data-lake-storage-gen2-from-gen1.md) met stapsgewijze instructies en best practices.

Bij het kopiëren van bestanden vanuit Azure Data Lake Storage (ADLS) Gen1 naar Gen2, kunt u de POSIX toegangsbeheerlijsten (ACL's) samen met gegevens behouden. Raadpleeg voor toegangsbeheer in details, [toegangsbeheer in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md) en [toegangsbeheer in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

De volgende typen ACL's kunnen worden bewaard met Azure Data Factory Copy-activiteit, kunt u een of meer typen selecteren:

- **ACL**: Kopieer en behouden **POSIX ACL's** voor bestanden en mappen. Het kopieert u de volledige bestaande ACL's van bron naar een sink. 
- **De eigenaar van**: Kopieer en behouden **de gebruiker die eigenaar is** van bestanden en mappen. Supergebruiker toegang tot ADLS Gen2 sink is vereist.
- **Groep**: Kopieer en behouden **de groep die eigenaar is** van bestanden en mappen. Supergebruiker toegang tot het sink-ADLS Gen2 of de gebruiker die eigenaar is (als de gebruiker die eigenaar ook lid is van de doelgroep is) is vereist.

Als u opgeeft als u wilt kopiëren uit een map, Data Factory de ACL's voor die opgegeven map, evenals de bestanden en mappen onder deze repliceert (als `recursive` is ingesteld op true). Als u opgeeft als u wilt kopiëren van een enkel bestand, de ACL's op wordt dat bestand gekopieerd.

>[!IMPORTANT]
>Als u behouden van ACL's wilt, zorg er dan voor dat u hiervoor toestemming hoog genoeg voor ADF werken op basis van uw sink Gen2 ADLS-account. Bijvoorbeeld, gebruik van verificatie van account-sleutel of de eigenaar van een opslag-Blob-gegevens-rol toewijzen aan de service-principal/beheerde identiteit.

Wanneer u als ADLS Gen1 met binaire kopie optie binaire indeling en sink als ADLS Gen2 met binaire kopie optie binaire indeling configureren, kunt u vinden **behouden** optie **pagina instellingen van het hulpprogramma Copy Data** of in **Copy Activity -> instellingen** tabblad voor het ontwerpen van de activiteit.

![ADLS Gen1 naar Gen2 ACL behouden](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

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

## <a name="mapping-data-flow-properties"></a>Eigenschappen van fouttoewijzing gegevensstroom

Informatie over de details van [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de gegevensstroom toewijzen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
