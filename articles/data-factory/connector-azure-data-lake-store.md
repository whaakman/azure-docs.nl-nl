---
title: Kopiëren van gegevens naar/van Azure Data Lake Storage Gen1 met Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron-gegevensopslag naar Azure Data Lake Store (of) van Data Lake Store voor ondersteunde sink stores met behulp van Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: jingwang
ms.openlocfilehash: d500bc9c910858341d7fdacb4d85bffc8be215e1
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338759"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Data Lake Storage Gen1 met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-datalake-connector.md)
> * [Huidige versie](connector-azure-data-lake-store.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Data Lake Storage Gen1 (voorheen bekend als Azure Data Lake Store). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van een ondersteund brongegevensarchief naar Azure Data Lake Store, of gegevens van Azure Data Lake Store kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Specifiek, ondersteunt deze Azure Data Lake Store-connector:

- Het kopiëren van bestanden met behulp van **service-principal** of **managed service identity (MSI)** verificatie.
- Kopiëren van bestanden als-is, of parsering/genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Als u gegevens met behulp van zelfgehoste Cloudintegratieruntime kopieert, configureert u de firewall van het bedrijf om toe te staan van uitgaand verkeer naar `<ADLS account name>.azuredatalakestore.net` en `login.microsoftonline.com/<tenant>/oauth2/token` op poort 443. De laatste is Azure Security Token Service (STS) die IR met communiceren moet met het toegangstoken ophalen.

## <a name="get-started"></a>Aan de slag

> [!TIP]
> Zie voor een overzicht van het gebruik van Azure Data Lake Store-connector, [gegevens laden in Azure Data Lake Store](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

>[!NOTE]
>Wanneer u hulpprogramma voor kopiëren-gegevens gebruiken om te schrijven kopieerpijplijn of ADF UI gebruiken voor het uitvoeren van tests verbinding/navigeren door mappen tijdens het ontwerpen, moeten de machtiging van de service-principal of MSI wordt verleend op hoofdniveau. While, kunt uitvoering van activiteit kopiëren werken, zolang de machtiging te tot de gegevens krijgen moeten worden gekopieerd. Als u moet de machtiging beperken, kunt u de authoring bewerkingen overslaan.

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Data lake Store.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Data Lake Store service gekoppelde:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureDataLakeStore**. | Ja |
| dataLakeStoreUri | Informatie over de Azure Data Lake Store-account. Deze informatie wordt een van de volgende indelingen: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| subscriptionId | Azure-abonnement-ID die de Data Lake Store-account behoort. | Vereist voor sink |
| resourceGroupName | Naam van Azure-resource die de Data Lake Store-account behoort. | Vereist voor sink |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

Raadpleeg de volgende secties over meer eigenschappen en voorbeelden van de JSON voor andere verificatietypen respectievelijk:

- [Met behulp van service-principal verificatie](#using-service-principal-authentication)
- [Met behulp van verificatie van de beheerde service-identiteit](#using-managed-service-identity-authentication)

### <a name="using-service-principal-authentication"></a>Met behulp van service-principal verificatie

De Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) voor het gebruik van service-principal verificatie, en het toegang geven tot Data Lake Store. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Toepassingssleutel
- Tenant-id

>[!IMPORTANT]
> Zorg ervoor dat u verleent de service principal juiste machtiging in Azure Data Lake Store:
>- **Als bron**, in Data explorer -> toegang, ten minste verlenen **lees- en uitvoeringsmachtigingen** toestemming voor het weergeven en kopieer de bestanden in de map/submappen, of **lezen** toestemming voor het kopiëren van een enkel bestand; en wilt toevoegen aan **deze map en alle onderliggende** voor recursieve en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Er is geen vereiste op account niveau toegangsbeheer (IAM).
>- **Als sink**, in Data explorer -> toegang, ten minste verlenen **schrijven + uitvoeren** toestemming voor het maken van de onderliggende items in de map en toevoegen aan **deze map en alle onderliggende** voor recursieve en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Als u Azure IR gebruiken om te kopiëren (bron en sink zijn in de cloud), in de Access control (IAM), verleent ten minste **lezer** rol om te kunnen detecteren van Data Lake Store-regio met Data Factory. Als u wilt om te voorkomen dat deze rol IAM expliciet [maken van een Azure IR](create-azure-integration-runtime.md#create-azure-ir) door de locatie van uw Data Lake Store en koppelen in de Data Lake Store gekoppelde service als in het volgende voorbeeld.

De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja |

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

Een data factory kan worden gekoppeld aan een [beheerde service-identiteit](data-factory-service-identity.md), die staat voor deze specifieke data factory. U kunt deze service-identiteit rechtstreeks gebruiken voor de Data Lake Store-verificatie is vergelijkbaar met het gebruik van uw eigen service-principal. Kan deze aangewezen factory om gegevens te toegang en kopiëren van/naar uw Data Lake Store.

Beheerde service identity (MSI)-verificatie gebruiken:

1. [Ophalen van data factory-service-identiteit](data-factory-service-identity.md#retrieve-service-identity) door de waarde van "SERVICE-identiteit TOEPASSINGS-ID gegenereerd samen met uw factory kopiëren.
2. De service-identiteit toegang verlenen tot Data Lake Store de dezelfde manier als voor de service principal volgende hieronder opmerkingen bij de.

>[!IMPORTANT]
> Zorg ervoor dat u de data factory-service-identiteit juiste machtigingen in Azure Data Lake Store verlenen:
>- **Als bron**, in Data explorer -> toegang, ten minste verlenen **lees- en uitvoeringsmachtigingen** toestemming voor het weergeven en kopieer de bestanden in de map/submappen, of **lezen** toestemming voor het kopiëren van een enkel bestand; en wilt toevoegen aan **deze map en alle onderliggende** voor recursieve en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Er is geen vereiste op account niveau toegangsbeheer (IAM).
>- **Als sink**, in Data explorer -> toegang, ten minste verlenen **schrijven + uitvoeren** toestemming voor het maken van de onderliggende items in de map en toevoegen aan **deze map en alle onderliggende** voor recursieve en toevoegen als **een toegangsmachtiging en een standaardmachtiging**. Als u Azure IR gebruiken om te kopiëren (bron en sink zijn in de cloud), in de Access control (IAM), verleent ten minste **lezer** rol om te kunnen detecteren van Data Lake Store-regio met Data Factory. Als u wilt om te voorkomen dat deze rol IAM expliciet [maken van een Azure IR](create-azure-integration-runtime.md#create-azure-ir) door de locatie van uw Data Lake Store en koppelen in de Data Lake Store gekoppelde service als in het volgende voorbeeld.

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake Store-gegevensset.

Als u wilt kopiëren van gegevens naar/van Azure Data Lake Store, stel de eigenschap type van de gegevensset in **AzureDataLakeStoreFile**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureDataLakeStoreFile** |Ja |
| folderPath | Pad naar de map in de Data Lake Store. Filteren op jokerteken wordt niet ondersteund. Voorbeeld: rootfolder/submap / |Ja |
| fileName | **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met de volgende indeling: '*gegevens. [ uitvoering van activiteit-id GUID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ". Een voorbeeld is 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'. |Nee |
| Indeling | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken. 

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

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Data Lake-bron- en sinkblobpaden.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store als bron

Om gegevens te kopiëren van Azure Data Lake Store, stelt u het brontype in de kopieeractiviteit naar **AzureDataLakeStoreSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureDataLakeStoreSource** |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |

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

Om gegevens te kopiëren naar Azure Data Lake Store, stelt u het sink-type in de kopieeractiviteit naar **AzureDataLakeStoreSink**. De volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **AzureDataLakeStoreSink** |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron-bestanden van gegevensopslag op basis van bestanden.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden uit de bronmap van het zich in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><b>-MergeFiles</b>: alle bestanden uit de bronmap naar één bestand worden samengevoegd. Als de naam van de bestands-/ Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. anders zou worden automatisch gegenereerde naam. | Nee |

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

### <a name="recursive-and-copybehavior-examples"></a>recursieve en copyBehavior voorbeelden

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursieve | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + 5-bestand inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
