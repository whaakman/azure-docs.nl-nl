---
title: "Kopiëren van gegevens en naar Azure Table storage met behulp van de Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron winkels naar Azure Table storage, of van Table storage voor ondersteunde sink stores, met behulp van Data Factory."
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 41e2117e14f336d33f5d6f4e1f446e32a6886079
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Kopiëren van gegevens en naar Azure Table storage met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - algemeen beschikbaar](v1/data-factory-azure-table-connector.md)
> * [Versie 2 - Preview](connector-azure-table-storage.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren en naar Azure Table storage. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory, die in het algemeen beschikbaar is, Zie [Table storage-connector in versie 1](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit alle ondersteunde brongegevensarchief kopiëren naar Table storage. U kunt ook gegevens uit de opslag van de tabel kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze Azure-tabel connector ondersteunt het kopiëren van gegevens met behulp van sleutel-account en service gedeeld in het bijzonder access signature-verificaties.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Table storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

### <a name="use-an-account-key"></a>De sleutel van een account gebruiken

U kunt een gekoppelde Azure Storage-service maken met behulp van de accountsleutel. Het biedt de gegevensfactory globale toegang tot opslag. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureStorage**. |Ja |
| connectionString | Geef de benodigde informatie om verbinding maken met de opslag voor de eigenschap connectionString. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

### <a name="use-service-shared-access-signature-authentication"></a>Gebruik service shared access signature-verificatie

U kunt ook een gekoppelde Storage-service maken met behulp van een shared access signature. Het biedt de gegevensfactory beperkt/tijdsgebonden toegang tot alle/specifieke bronnen in de opslag.

Een shared access signature biedt gedelegeerde toegang tot bronnen in uw opslagaccount. U kunt deze gebruiken dat een client beperkte machtigingen voor objecten in uw storage-account gedurende een opgegeven periode en met een opgegeven set machtigingen verlenen. U hoeft niet te delen van de toegangssleutels van uw account. De shared access signature is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot resources met de shared access signature storage, moet de client alleen om door te geven in de handtekening voor gedeelde toegang tot de juiste constructor of methode. Zie voor meer informatie over handtekeningen voor gedeelde toegang [handtekeningen voor gedeelde toegang: inzicht in het shared access signature-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Data Factory ondersteunt nu alleen handtekeningen voor gedeelde service toegang maar geen account gedeelde handtekeningen voor toegang. Zie voor meer informatie over deze twee typen en het maken van deze [typen handtekeningen voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). De shared access signature-URL is gegenereerd op basis van de Azure portal of Azure Storage Explorer is een shared access signature van account, die wordt niet ondersteund.

> [!TIP]
> U kunt de volgende PowerShell-opdrachten voor het genereren van een service shared access signature voor uw opslagaccount kunt uitvoeren. Vervang de tijdelijke aanduidingen en de benodigde machtiging verlenen.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Voor het gebruik van shared access signature-verificatie van de service worden de volgende eigenschappen ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureStorage**. |Ja |
| sasUri | Geef de shared access signature URI de Storage-resources, zoals de blob-container of tabel. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt de Runtime van de integratie van Azure of de Self-hosted integratie-Runtime gebruiken (als uw gegevensarchief bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Wanneer u een shared access signature URI maakt, houd rekening met de volgende punten:

- Juiste lezen/schrijven-machtigingen voor objecten op basis van hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw data factory instellen.
- Stel **verlooptijdstip** op de juiste wijze. Zorg ervoor dat de toegang tot opslagobjecten niet binnen een actieve periode van de pijplijn verloopt.
- De URI moet worden gemaakt op het niveau van de rechtertabel op basis van de behoeften.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure Table-gegevensset.

Om gegevens te kopiëren naar en van Azure Table, stel de eigenschap type van de gegevensset **AzureTable**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureTable**. |Ja |
| tableName |De naam van de tabel in de tabel opslag database-instantie waarnaar de gekoppelde service verwijst. |Ja |

**Voorbeeld:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema door Data Factory

Voor gegevens zonder schema winkels zoals Azure Table afleidt Data Factory het schema in een van de volgende manieren:

* Als u de structuur van gegevens met behulp van opgeven de **structuur** eigenschap in de definitie van de gegevensset, Data Factory zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt in dit geval een null-waarde opgegeven voor deze.
* Als u de structuur van gegevens niet via opgeeft de **structuur** het schema-eigenschap in de definitie van de gegevensset, Data Factory afleidt met behulp van de eerste rij in de gegevens. Als de eerste rij niet het volledige schema bevat, worden sommige kolommen in dit geval wordt opgehaald in het resultaat van de kopieerbewerking.

Voor gegevensbronnen zonder schema, de beste manier is om op te geven van de structuur van gegevens met behulp van de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door de Azure Table-bron- en sink ondersteund.

### <a name="azure-table-as-a-source-type"></a>Azure-tabel als een type gegevensbron

Om gegevens te kopiëren van Azure Table, stelt u het brontype in de kopieerbewerking naar **AzureTableSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **AzureTableSource**. |Ja |
| azureTableSourceQuery |Gebruik de aangepaste Table storage-query om gegevens te lezen. Zie de voorbeelden in de volgende sectie. |Nee |
| azureTableSourceIgnoreTableNotFound |Geeft aan of de uitzondering van de tabel niet aanwezig is toegestaan.<br/>Toegestane waarden zijn **True** en **False** (standaard). |Nee |

### <a name="azuretablesourcequery-examples"></a>Voorbeelden van azureTableSourceQuery

Als de Azure Table-kolom van het type datum/tijd:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Als de Azure Table-kolom van het tekenreekstype:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

Als u de pijplijn-parameter gebruikt, geconverteerd naar de juiste indeling volgens de eerdere voorbeelden de datetime-waarde.

### <a name="azure-table-as-a-sink-type"></a>Azure-tabel als een sink-type

Om gegevens te kopiëren naar Azure Table, stelt u het sink-type in de kopieerbewerking naar **AzureTableSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op **AzureTableSink**. |Ja |
| azureTableDefaultPartitionKeyValue |De standaardwaarde voor de partitiesleutel die kan worden gebruikt door de sink. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet wordt opgegeven, wordt 'AzureTableDefaultPartitionKeyValue' gebruikt als de partitiesleutel. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet wordt opgegeven, gebruikt u een GUID voor elke rij. |Nee |
| azureTableInsertType |De modus voor gegevens in Azure-tabel invoegen. Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met partitie-als rijsleutels die overeenkomt met de waarden vervangen of samenvoegen van hebben. <br/><br/>Toegestane waarden zijn **samenvoegen** (standaard) en **vervangen**. <br/><br> Deze instelling is van toepassing op het rijniveau van de niet het tabelniveau van de. Geen van beide optie worden de rijen in de uitvoertabel die niet bestaan in de invoer verwijderd. Zie voor meer informatie over de werking van de instellingen voor samenvoegen en vervangen, [entiteit invoegen of merge](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Nee |
| writeBatchSize |Voegt de gegevens in Azure-tabel wanneer writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn geheel getal (aantal rijen). |Nee (de standaardwaarde is 10.000) |
| writeBatchTimeout |Voegt de gegevens in Azure-tabel wanneer writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn timespan. Een voorbeeld is ' 00: 20:00 ' (20 minuten). |Nee (de standaardwaarde is 90 seconden, de opslag van de client standaardtime-out) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Een bronkolom toewijzen aan een doelkolom met behulp van de **'vertaler'** eigenschap voordat u de doelkolom als azureTablePartitionKeyName kunt gebruiken.

In het volgende voorbeeld wordt de bronkolom DivisionID toegewezen aan de doelkolom DivisionID:

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

'DivisionID' is opgegeven als de partitiesleutel.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>De gegevenstypetoewijzing voor Azure Table

Wanneer u gegevens van en naar Azure Table kopiëren, worden de volgende toewijzingen van Azure Table-gegevenstypen gebruikt voor Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe het brontype schema en de gegevens in de kopieerbewerking wordt toegewezen aan de sink, [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

Wanneer u gegevens verplaatst naar en van Azure Table, de volgende [toewijzingen die zijn gedefinieerd door Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) worden gebruikt vanuit Azure tabel OData-typen aan .NET-type en vice versa.

| Azure Table-gegevenstype | Data Factory tussentijdse gegevenstype | Details |
|:--- |:--- |:--- |
| Edm.Binary |byte[] |Een matrix met bytes maximaal 64 KB. |
| Edm.Boolean |BOOL |Een Booleaanse waarde. |
| Edm.DateTime |Datum en tijd |Een 64-bits waarde wordt uitgedrukt als Coordinated Universal Time (UTC). Het ondersteunde bereik van de datum-/ begint middernacht, 1 januari 1601 A.D. (C.E.) UTC. Het bereik eindigt 31 December 9999. |
| Edm.Double |dubbele |Een 64-bits drijvende-kommawaarde. |
| Edm.Guid |GUID |Een globally unique identifier van 128-bits. |
| Edm.Int32 |Int32 |Een 32-bits geheel getal. |
| Edm.Int64 |Int64 |Een 64-bits geheel getal. |
| Edm.String |Tekenreeks |Een waarde UTF-16-codering. String-waarden mag maximaal 64 KB. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
