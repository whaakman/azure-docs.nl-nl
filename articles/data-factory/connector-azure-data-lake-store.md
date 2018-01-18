---
title: "Gegevens kopiëren naar/van Azure Data Lake Store met behulp van de Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron gegevensarchieven met Azure Data Lake Store (of) van Data Lake Store voor ondersteunde sink stores gebruik Data Factory."
services: data-factory
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: c388fe0cfe85ec2bf2b752f74d39eb2ebe38ceb1
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="copy-data-to-or-from-azure-data-lake-store-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Lake Store met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-datalake-connector.md)
> * [Versie 2 - Preview](connector-azure-data-lake-store.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Data Lake Store. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure Data Lake Store-connector in V1](v1/data-factory-azure-datalake-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure Data Lake Store of gegevens kopiëren van Azure Data Lake Store met alle ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Azure Data Lake Store-connector:

- Het kopiëren van bestanden met behulp van **service-principal** of **beheerde service-identiteit (MSI)** verificatie.
- Kopiëren van bestanden als-is, of parsering/genereren van bestanden met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Data lake Store.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Data Lake Store service gekoppelde:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureDataLakeStore**. | Ja |
| dataLakeStoreUri | Informatie over het Azure Data Lake Store-account. Deze informatie heeft een van de volgende indelingen: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| tenant | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja |
| subscriptionId | Azure-abonnement-ID waartoe het Data Lake Store-account behoort. | Vereist voor sink |
| resourceGroupName | Naam Azure resourcegroep waartoe het Data Lake Store-account behoort. | Vereist voor sink |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

Raadpleeg de volgende secties over meer eigenschappen en voorbeelden van JSON voor andere verificatietypen respectievelijk:

- [Met behulp van verificatie van de service-principal](#using-service-principal-authentication)
- [Met behulp van verificatie van de beheerde service-identiteit](#using-managed-service-identity-authentication)

### <a name="using-service-principal-authentication"></a>Met behulp van verificatie van de service-principal

Als u verificatie van de service-principal, registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) en het toegang geven tot Data Lake Store. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Sleutel van toepassing
- Tenant-id

>[!IMPORTANT]
> Zorg ervoor dat u service principal juiste toestemming geven in Azure Data Lake Store:
>- **Als bron**, ten minste verlenen **lezen + Execute** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van één bestand. Er is geen vereiste voor account toegangsniveau control (IAM).
>- **Als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR kopiëren (bron- en sink zijn in de cloud), om te kunnen laten Data Factory Data Lake Store regio detecteren, verlenen ten minste **lezer** rol in account toegangsbeheer (IAM). Als u wilt voorkomen van deze rol IAM expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) gekoppelde service als het volgende voorbeeld met de locatie van uw Data Lake Store en koppelen in de Data Lake Store:

De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| servicePrincipalId | Geef de toepassing client-ID. | Ja |
| servicePrincipalKey | De sleutel van de toepassing opgeven. Dit veld markeren als een SecureString. | Ja |

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

### <a name="using-managed-service-identity-authentication"></a>Met behulp van verificatie van de beheerde service-identiteit

Een gegevensfactory kan worden gekoppeld aan een [beheerde service-identiteit](data-factory-service-identity.md), die staat voor deze specifieke gegevensfactory. U kunt deze service-identiteit rechtstreeks gebruiken voor verificatie van Data Lake Store vergelijkbaar met het gebruik van uw eigen service-principal. Hierdoor kan de aangewezen fabriek voor toegang en gegevens kopiëren van/naar uw Data Lake Store.

Beheerde service identiteitsverificatie (MSI) gebruiken:

1. [Ophalen van de data factory-service-identiteit](data-factory-service-identity.md#retrieve-service-identity) door de waarde van 'SERVICE identiteit TOEPASSINGS-ID gegenereerd samen met uw gegevensfactory.
2. De service-identiteit toegang verlenen aan Data Lake Store dezelfde manier als u zou voor service-principal doen. Zie voor gedetailleerde stappen [authentication Service-naar-service - toewijzen de Azure AD-toepassing met het Azure Data Lake Store-accountbestand of map](../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md#step-3-assign-the-azure-ad-application-to-the-azure-data-lake-store-account-file-or-folder).

>[!IMPORTANT]
> Zorg ervoor dat u data factory-service de identiteit juiste toestemming geven in Azure Data Lake Store:
>- **Als bron**, ten minste verlenen **lezen + Execute** data access-machtiging voor het weergeven en kopieer de inhoud van een map of **lezen** machtiging voor het kopiëren van één bestand. Er is geen vereiste voor account toegangsniveau control (IAM).
>- **Als sink**, ten minste verlenen **schrijven + uitvoeren** data access-machtiging voor het maken van onderliggende items in de map. En als u Azure IR kopiëren (bron- en sink zijn in de cloud), om te kunnen laten Data Factory Data Lake Store regio detecteren, verlenen ten minste **lezer** rol in account toegangsbeheer (IAM). Als u wilt voorkomen van deze rol IAM expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) gekoppelde service als het volgende voorbeeld met de locatie van uw Data Lake Store en koppelen in de Data Lake Store:

In Azure Data Factory hoeft u niet naast de algemene gegevens voor de Data Lake Store-eigenschappen opgeven in de gekoppelde service.

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
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

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake Store-gegevensset.

Om gegevens te kopiëren naar/van Azure Data Lake Store, stel de eigenschap type van de gegevensset **AzureDataLakeStoreFile**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureDataLakeStoreFile** |Ja |
| folderPath | Pad naar de container en map in de opslag van bestanden. Voorbeeld: rootfolder/submap / |Ja |
| fileName | Geef de naam van het bestand in de **folderPath** als u wilt kopiëren naar/van een specifiek bestand. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map.<br/><br/>Wanneer de bestandsnaam niet wordt opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in activiteit sink kopieeractiviteit genereert automatisch de bestandsnaam van het met de volgende indeling: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. Bijvoorbeeld: `Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz`. |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

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
            "fileName": "myfile.csv.gz",
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

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake-bron- en sink.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store als bron

Om gegevens te kopiëren van Azure Data Lake Store, stelt u het brontype in de kopieerbewerking naar **AzureDataLakeStoreSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureDataLakeStoreSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. Opmerking Wanneer recursieve is ingesteld op true en sink is bestandsgebaseerde opslag, lege map/subbewerkingen-folder niet worden gekopieerd/gemaakt op de sink.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS input dataset name>",
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

Om gegevens te kopiëren naar Azure Data Lake Store, kunt u het sink-type instellen in de kopieeractiviteit om te **AzureDataLakeStoreSink**. De volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **AzureDataLakeStoreSink** |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron-bestanden van het gegevensarchief op basis van bestanden.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><b>-MergeFiles</b>: alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand/de opgegeven Blob is opgegeven, zijn de samengevoegde bestandsnaam de opgegeven naam; anders zou worden automatisch gegenereerde naam. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS output dataset name>",
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

### <a name="recursive-and-copybehavior-examples"></a>Voorbeelden van recursieve en copyBehavior

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor verschillende combinaties van recursieve en copyBehavior waarden.

| Recursieve | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| waar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| waar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 bestand2 + bestand3 + File4 + bestand 5 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| onwaar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
