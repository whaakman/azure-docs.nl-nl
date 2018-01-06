---
title: "Gegevens kopiëren naar/van Azure Blob Storage met behulp van de Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron gegevensarchieven naar Azure Blob-opslag (of) van Blob Storage tot gegevensarchieven ondersteunde sink gebruik Data Factory."
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: 4b138c0a759c490f30aaa3be543e01aa356fb8ac
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Kopiëren van gegevens of naar Azure Blob Storage met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-blob-connector.md)
> * [Versie 2 - Preview](connector-azure-blob-storage.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Blob Storage. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure Blog van Storage connnector in V1](v1/data-factory-azure-blob-connector.md).


## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure Blob-opslag of gegevens uit Azure Blob Storage kopiëren naar een ondersteunde sink-gegevensarchief. Voor een lijst met opgeslagen gegevens die worden ondersteund als bronnen / of PUT door de kopieeractiviteit, Zie de [ondersteunde gegevensarchieven](copy-activity-overview.md) tabel.

In het bijzonder ondersteunt deze Azure Blob-connector:

- Bezig met het kopiëren van de blobs naar/van zowel algemeen Azure Storage-accounts als Hot/Cool Blob-opslag. 
- Kopiëren van BLOB's met zowel **accountsleutel** en **Service-SAS** authenticaties (Shared Access Signature).
- Kopiëren van BLOB's **vanuit blok, toevoegen of pagina-blobs**, gegevens en kopiëren **naar alleen blok-blobs**. Azure Premium-opslag wordt niet ondersteund als een sink omdat het wordt ondersteund door de pagina-blobs.
- Kopiëren van BLOB's als-is of bij het parseren van/genereren van BLOB's met de [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met de kopieeractiviteit middels de .NET SDK, Python SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [kopie activiteit zelfstudie](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Blob Storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

### <a name="using-account-key"></a>Met behulp van sleutel-account

U kunt een gekoppelde Azure Storage-service maken met behulp van de accountsleutel, die de gegevensfactory met globale toegang tot Azure Storage biedt. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met Azure-opslag voor de eigenschap connectionString. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-service-sas-authentication"></a>Service-SAS-verificatie

U kunt ook een gekoppelde Azure Storage-service maken met behulp van een Shared Access Signature (SAS), waardoor de gegevensfactory met beperkte/tijdsgebonden toegang tot alle/specifieke bronnen (blobcontainer) / in de opslag.

Een Shared Access Signature (SAS) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. Met SAS, verleent u dat een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode, zonder dat voor het delen van de toegangssleutels van uw account. De SAS is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot de storage-resources met de SA's, moet de client alleen worden doorgegeven in de SA's aan de juiste constructor of methode. Zie voor gedetailleerde informatie over SAS [Shared Access Signatures: inzicht in het SAS-Model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Azure Data Factory nu ondersteunt alleen het **Service-SAS** maar geen Account-SAS. Zie [typen van handtekeningen voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen en bouwen. De SAS-URL die is gegenereerd op basis van de Azure portal of Opslagverkenner is een SAS-Account, wat niet wordt ondersteund.

> [!TIP]
> U kunt de onderstaande PowerShell-opdrachten voor het genereren van een SAS-Service voor uw opslagaccount (vervangen de houders van de locatie en de benodigde machtiging grant) uitvoeren:`$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Service-SAS om verificatie te gebruiken, zijn de volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| sasUri | Shared Access Signature URI voor de Azure Storage-resources zoals blob-container of tabel opgeven. Dit veld markeren als een SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Bij het maken van een **SAS-URI**, met inbegrip van de volgende punten:

- Stel de juiste lezen/schrijven **machtigingen** op objecten op basis van hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw gegevensfactory.
- Stel **verlooptijdstip** op de juiste wijze. Zorg ervoor dat de toegang tot Azure Storage-objecten niet binnen een actieve periode van de pijplijn verloopt.
- URI moet worden gemaakt op de juiste container/blob of tabelniveau op basis van de behoeften. Een SAS-Uri met een Azure-blob kan de Data Factory-service voor toegang tot bepaalde blob. Een SAS-Uri naar een Azure blob-container kan de Data Factory-service om te doorlopen blobs in de container. Als u wilt toegang later meer/minder objecten bieden of bijwerken van de SAS-URI, moet u bij het bijwerken van de gekoppelde service met de nieuwe URI.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Blob-gegevensset.

Om gegevens te kopiëren naar/van Azure Blob, stel de eigenschap type van de gegevensset **AzureBlob**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureBlob** |Ja |
| folderPath | Pad naar de container en map in blob storage. Voorbeeld: myblobcontainer/myblobfolder / |Ja |
| fileName | Geef de naam van de blob in de **folderPath** als u wilt kopiëren naar/van een specifieke blob. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle blobs in de map.<br/><br/>Wanneer de bestandsnaam niet wordt opgegeven voor een uitvoergegevensset en **preserveHierarchy** niet is opgegeven in activiteit sink kopieeractiviteit genereert automatisch de blobnaam van de met de volgende indeling: `Data.[activity run id GUID].[GUID if FlattenHierarchy].[format if configured].[compression if configured]`. Bijvoorbeeld: `Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz`. |Nee |
| Indeling | Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [parketvloeren indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor scenario binaire kopiëren) |
| Compressie | Geef het type en de compressie van de gegevens. Zie voor meer informatie [ondersteunde bestandsindelingen en compressiecodecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus: **optimale** en **snelst**. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Blob-bron- en sink.

### <a name="azure-blob-as-source"></a>Azure Blob als bron

Om gegevens te kopiëren van Azure-Blob, stelt u het brontype in de kopieerbewerking naar **BlobSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **BlobSource** |Ja |
| Recursieve | Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map.<br/>Toegestane waarden zijn: **true** (standaard), **false** | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
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
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-blob-as-sink"></a>Azure Blob als sink

Om gegevens te kopiëren naar Azure Blob, stelt u het sink-type in de kopieerbewerking naar **BlobSink**. De volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **BlobSink** |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron-bestanden van het gegevensarchief op basis van bestanden.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><b>-MergeFiles</b>: alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand/de opgegeven Blob is opgegeven, zijn de samengevoegde bestandsnaam de opgegeven naam; anders zou worden automatisch gegenereerde naam. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
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
| waar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| waar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | het doel Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 bestand2 + bestand3 + File4 + bestand 5 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| onwaar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |
| onwaar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 zijn niet opgenomen. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).