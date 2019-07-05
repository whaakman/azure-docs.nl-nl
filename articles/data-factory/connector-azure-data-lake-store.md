---
title: Gegevens kopiëren naar of van Azure Data Lake Storage Gen1 met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron-gegevensopslag naar Azure Data Lake Store, of van Data Lake Store voor ondersteunde sink-stores, met behulp van Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: df88c3e2e07165182c917eaf30a5f37451fbd073
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509576"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Lake Storage Gen1 met Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-azure-datalake-connector.md)
> * [Huidige versie](connector-azure-data-lake-store.md)

In dit artikel bevat een overzicht over het kopiëren van gegevens naar en van Azure Data Lake Storage Gen1. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze connector Azure Data Lake Storage Gen1 wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [bron- of sinkservice matrix ondersteund](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Met deze connector kunt u met name:

- Bestanden kopiëren met behulp van een van de volgende methoden voor verificatie: service-principal of beheerde identiteiten voor Azure-resources.
- Kopiëren van bestanden of parseren of genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Als u gegevens kopiëren met behulp van de zelf-hostende integratieruntime, configureert u de firewall van het bedrijf om toe te staan van uitgaand verkeer naar `<ADLS account name>.azuredatalakestore.net` en `login.microsoftonline.com/<tenant>/oauth2/token` op poort 443. De laatste is de Azure Service voor beveiligingstokens die de integratieruntime communiceren moet met het toegangstoken ophalen.

## <a name="get-started"></a>Aan de slag

> [!TIP]
> Zie voor stapsgewijze instructies voor het gebruik van de Azure Data Lake Store-connector, [gegevens laden in Azure Data Lake Store](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Data Lake Store.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Azure Data Lake Store service gekoppelde:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De `type` eigenschap moet worden ingesteld op **AzureDataLakeStore**. | Ja |
| dataLakeStoreUri | Informatie over de Azure Data Lake Store-account. Deze informatie wordt een van de volgende indelingen: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| subscriptionId | De Azure-abonnement-ID die de Data Lake Store-account behoort. | Vereist voor sink |
| resourceGroupName | De Azure-resource-groepsnaam waartoe het Data Lake Store-account behoort. | Vereist voor sink |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure integratieruntime of een zelf-hostende integratieruntime gebruiken als uw gegevensarchief bevindt zich in een particulier netwerk. Als deze eigenschap niet is opgegeven, wordt de standaard Azure integratieruntime wordt gebruikt. |Nee |

### <a name="use-service-principal-authentication"></a>Service-principal verificatie gebruiken

De Toepassingsentiteit van een registreren in Azure Active Directory voor het gebruik van service-principal verificatie, en het toegang geven tot Data Lake Store. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Toepassingssleutel
- Tenant-id

>[!IMPORTANT]
> Verleen de service principal juiste machtiging in Data Lake Store:
>- **Als bron**: In **Data explorer** > **toegang**, ten minste verlenen **lees- en uitvoeringsmachtigingen** toestemming voor het weergeven en kopieer de bestanden in mappen en submappen. Of u kunt verlenen **lezen** machtiging voor het kopiëren van een enkel bestand. U kunt kiezen om toe te voegen aan **deze map en alle onderliggende** voor recursieve, en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Er is geen vereiste op accountniveau toegangsbeheer (IAM).
>- **Als sink**: In **Data explorer** > **toegang**, ten minste verlenen **schrijven + uitvoeren** machtiging voor het maken van onderliggende items in de map. U kunt kiezen om toe te voegen aan **deze map en alle onderliggende** voor recursieve, en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Als u een Azure integratieruntime om te kopiëren (bron en sink zijn in de cloud), verleen in IAM, ten minste de **lezer** rol om te kunnen detecteren van de regio voor Data Lake Store met Data Factory. Als u wilt om te voorkomen dat deze rol IAM expliciet [maken van een Azure integratieruntime](create-azure-integration-runtime.md#create-azure-ir) door de locatie van Data Lake Store. Bijvoorbeeld, als uw Data Lake Store in West-Europa is, een Azure integratieruntime maken met de locatie ingesteld op "West-Europa." Koppel deze in de Data Lake Store gekoppelde service, zoals wordt weergegeven in het volgende voorbeeld.

>[!NOTE]
>Aan de lijst met mappen begint vanuit de hoofdmap, moet u de machtiging van de service-principal wordt verleend aan instellen **op hoofdniveau met de machtiging 'Uitvoeren'** . Dit geldt wanneer u de:
>- **Het kopieerprogramma van gegevens** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen.
>Als u zich zorgen maakt over het verlenen van machtiging op hoofdniveau, tijdens het ontwerpen, gaat u verder testen verbinding en kies vervolgens een paraent pad met een machtiging verleend om te bladeren van invoer die opgegeven pad. Kopieer activiteit werkt, zolang de service-principal is verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een `SecureString` voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | Geef de tenantgegevens, zoals de domeinnaam van het of tenant-ID, die uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Gebruik van beheerde identiteiten voor verificatie van de Azure-resources

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md), die staat voor deze specifieke data factory. U kunt deze beheerde identiteit rechtstreeks gebruiken voor verificatie met Data Lake Store, vergelijkbaar met het gebruik van uw eigen service-principal. Kan deze aangewezen factory toegang en Kopieer gegevens naar of van Data Lake Store.

Beheerde om identiteiten te gebruiken voor verificatie van de Azure-resources:

1. [Ophalen van de identiteitsgegevens van de data factory beheerd](data-factory-service-identity.md#retrieve-managed-identity) door de waarde van de Service-identiteit toepassings-ID"gegenereerd samen met uw factory kopiëren.
2. De beheerde identiteit toegang verlenen tot Data Lake Store, de dezelfde manier als voor service-principal, deze opmerkingen te volgen.

>[!IMPORTANT]
> Zorg ervoor dat u de data factory beheerde identiteit juiste machtiging verlenen in Data Lake Store:
>- **Als bron**: In **Data explorer** > **toegang**, ten minste verlenen **lees- en uitvoeringsmachtigingen** toestemming voor het weergeven en kopieer de bestanden in mappen en submappen. Of u kunt verlenen **lezen** machtiging voor het kopiëren van een enkel bestand. U kunt kiezen om toe te voegen aan **deze map en alle onderliggende** voor recursieve, en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Er is geen vereiste op accountniveau toegangsbeheer (IAM).
>- **Als sink**: In **Data explorer** > **toegang**, ten minste verlenen **schrijven + uitvoeren** machtiging voor het maken van onderliggende items in de map. U kunt kiezen om toe te voegen aan **deze map en alle onderliggende** voor recursieve, en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Als u een Azure integratieruntime om te kopiëren (bron en sink zijn in de cloud), verleen in IAM, ten minste de **lezer** rol om te kunnen detecteren van de regio voor Data Lake Store met Data Factory. Als u wilt om te voorkomen dat deze rol IAM expliciet [maken van een Azure integratieruntime](create-azure-integration-runtime.md#create-azure-ir) door de locatie van Data Lake Store. Koppel deze in de Data Lake Store gekoppelde service, zoals wordt weergegeven in het volgende voorbeeld.

>[!NOTE]
>Aan de lijst met mappen begint vanuit de hoofdmap, moet u de machtiging van de beheerde identiteit wordt verleend aan instellen **op hoofdniveau met de machtiging 'Uitvoeren'** . Dit geldt wanneer u de:
>- **Het kopieerprogramma van gegevens** om de kopieerpijplijn auteur te.
>- **Data Factory-UI** voor het testen van verbinding en het navigeren door mappen tijdens het ontwerpen.
>Als u zich zorgen maakt over het verlenen van machtiging op hoofdniveau, tijdens het ontwerpen, gaat u verder testen verbinding en kies vervolgens een bovenliggend pad met een machtiging verleend om te bladeren van invoer die opgegeven pad. Kopieer activiteit werkt, zolang de service-principal is verleend met de juiste machtigingen aan de bestanden worden gekopieerd.

In Azure Data Factory moet u niet alle eigenschappen naast de algemene informatie van de Data Lake Store opgeven in de gekoppelde service.

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
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

- Zie voor de parquet en gescheiden tekstopmaak de [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Zie voor andere indelingen, zoals ORC, Avro, JSON of binaire indeling, de [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Als u wilt kopiëren van en naar Azure Data Lake Store Gen1 gegevens in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikelen op de gegevensset op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure Data Lake Store Gen1 onder `location` instellingen in de gegevensset op basis van een indeling:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **AzureDataLakeStoreLocation**. | Ja      |
| folderPath | Het pad naar een map. Als u wilt een jokerteken gebruiken om te filteren worden de mappen overslaan van deze instelling en deze opgeven in de instellingen voor de bron. | Nee       |
| fileName   | De naam van het bestand onder het opgegeven mappad. Als u een jokerteken gebruiken om te filteren van bestanden wilt, overslaan van deze instelling en deze opgeven in de instellingen voor de bron. | Nee       |

> [!NOTE]
>
> De **AzureDataLakeStoreFile** type gegevensset met de indeling van parquet of tekst vermeld in de volgende sectie wordt nog steeds ondersteund als voor het kopiëren, opzoeken en de activiteit GetMetadata voor achterwaartse compatibiliteit. Maar dit niet werkt met de functie voor toewijzing gegevensstroom. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen1 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureDataLakeStoreLocation",
                "folderPath": "root/folder/subfolder"
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

Om gegevens te kopiëren naar en van Azure Data Lake Store Gen1 in ORC, Avro, JSON of binaire indeling, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureDataLakeStoreFile**. |Ja |
| folderPath | Pad naar de map in Data Lake Store. Indien niet opgegeven, wordt deze verwijst naar de hoofdmap. <br/><br/>Filteren op jokerteken wordt ondersteund. Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken of deze escape-teken in. <br/><br/>Bijvoorbeeld: rootfolder/submap /. Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Nee |
| fileName | Naam of het jokerteken filter voor de bestanden in de opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter, de jokertekens toegestaan zijn `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke heeft een jokerteken of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ', bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'. Als u vanuit een bron in tabelvorm kopieert met behulp van een tabelnaam wordt opgegeven in plaats van een query, het naampatroon is ' *[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ', bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br/><br/> De algehele prestaties van de verplaatsing van gegevens wordt beïnvloed door deze instelling is ingeschakeld als u wilt filter het bestand met grote hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br/><br/> De algehele prestaties van de verplaatsing van gegevens wordt beïnvloed door deze instelling is ingeschakeld als u wilt filter het bestand met grote hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd.| Nee |
| format | Als u wilt kopiëren van bestanden als tussen winkels op basis van bestanden (binaire kopie), moet u de sectie indeling in beide definities van de gegevensset voor invoer en uitvoer overslaan.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling ](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimale** en **snelst**. |Nee |


>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Als u wilt één bestand met een bepaalde naam kopiëren, geef **folderPath** met een maponderdeel en **fileName** met een bestandsnaam op.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met een maponderdeel en **fileName** met een wildcard-filter. 

**Voorbeeld:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, [pijplijnen](concepts-pipelines-activities.md). Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake Store-bron- en sinkblobpaden.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store als bron

- Als u wilt kopiëren uit parquet of gescheiden tekstopmaak, Zie de [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Als u wilt kopiëren van andere indelingen, zoals ORC, Avro, JSON of binaire indeling, Zie de [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Om gegevens te kopiëren van Azure Data Lake Store Gen1 in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikelen op de bron van de activiteit kopiëren op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure Data Lake Store Gen1 onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureDataLakeStoreReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens voor het filteren van bronmappen. <br>Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder het opgegeven mappad/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` als escape voor als de naam van uw map zelf een jokerteken of deze escape-teken in. Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk Laatst gewijzigd. De bestanden zijn geselecteerd als hun laatst gewijzigd binnen het tijdsbereik tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de indeling van ' 2018-12-01T05:00:00Z '. <br> De eigenschappen kunnen niet null zijn, wat betekent dat er geen kenmerk bestandsfilter wordt toegepast op de gegevensset. Wanneer `modifiedDatetimeStart` een datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk zijn aan de datum / tijdwaarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` een datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het laatst gewijzigde kenmerk kleiner dan de datum / tijdwaarde is zijn geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor parquet of gescheiden tekstopmaak, de **AzureDataLakeStoreSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als voor achterwaartse compatibiliteit. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
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
                    "type": "AzureDataLakeStoreReadSetting",
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

Om gegevens te kopiëren van Azure Data Lake Store Gen1 in ORC, Avro, JSON of binaire indeling, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De `type` eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **AzureDataLakeStoreSource**. |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Wanneer `recursive` is ingesteld op true en de sink is een bestandsgebaseerde opslag, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen1 input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store als sink

- Als u wilt kopiëren naar parquet en gescheiden tekstopmaak, Zie de [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Als u wilt kopiëren naar andere indelingen, zoals ORC, Avro, JSON of binaire indeling, Zie de [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Om gegevens te kopiëren naar Azure Data Lake Store Gen1 in parquet of gescheiden tekstopmaak, Zie de [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikelen op kopiëren-indeling op basis van activiteit-sink- en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure Data Lake Store Gen1 onder `storeSettings` instellingen in de sink-indeling op basis van kopiëren:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureDataLakeStoreWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor de bronmap is identiek aan het relatieve pad van de doel-bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor parquet of gescheiden tekstopmaak, de **AzureDataLakeStoreSink** type activiteit-sink voor exemplaar vermeld in de volgende sectie wordt nog steeds ondersteund als voor achterwaartse compatibiliteit. U wordt aangeraden dat u dit nieuwe model gaan gebruiken. De Data Factory UI ontwerpen genereert deze nieuwe typen.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
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
                    "type": "AzureDataLakeStoreWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Andere indeling-sink

Om gegevens te kopiëren naar Azure Data Lake Store Gen1 in ORC, Avro, JSON of binaire indeling, de volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De `type` eigenschap van de kopie-activiteit-sink moet zijn ingesteld op **AzureDataLakeStoreSink**. |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor de bronmap is identiek aan het relatieve pad van de doel-bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van het automatisch gegenereerde. | Nee |
| maxConcurrentConnections | Het aantal verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen1 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
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

### <a name="examples-of-behavior-of-the-copy-operation"></a>Voorbeelden van het gedrag van de kopieerbewerking

In deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van `recursive` en `copyBehavior` waarden.

| recursive | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + File5 inhoud worden samengevoegd tot één bestand, met een automatisch gegenereerde naam. |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 niet worden opgehaald. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 niet worden opgehaald. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 niet worden opgehaald. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>ACL's voor Data Lake Storage Gen2 behouden

Als u wilt voor het repliceren van de toegangsbeheerlijsten (ACL's), samen met bestanden wanneer u een upgrade uitvoeren van Data Lake Storage Gen1 naar Data Lake Storage Gen2, Zie [behouden ACL's van Data Lake Storage Gen1](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1).

## <a name="mapping-data-flow-properties"></a>Eigenschappen van de gebruikersstroom toewijzing van gegevens

Meer informatie over [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de functie voor toewijzing gegevensstroom.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
