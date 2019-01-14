---
title: Gegevens kopiëren naar en van Azure Table storage met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van ondersteunde bron winkels Azure Table Storage, of van Table storage voor ondersteunde sink-stores, met behulp van Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: b1f4ad523f84616391d4121dbf7eaabb2dfde060
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018616"
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Gegevens kopiëren naar en van Azure Table storage met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-azure-table-connector.md)
> * [Huidige versie](connector-azure-table-storage.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Table storage. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de Kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een ondersteund brongegevensarchief kopiëren naar Table storage. U kunt ook gegevens uit Table storage kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze Azure Table-connector ondersteunt het kopiëren van gegevens met behulp van sleutel en de service gedeeld name access signature-verificatie.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Table storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

### <a name="use-an-account-key"></a>Een account-sleutel

U kunt een gekoppelde Azure Storage-service maken met behulp van de accountsleutel. Het biedt de data factory met wereldwijde toegang tot opslag. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureTableStorage**. |Ja |
| connectionString | Geef de informatie die nodig zijn voor het verbinding maken met opslag voor de connectionString-eigenschap. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Als u "AzureStorage" type gekoppelde service zijn gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u het gebruik van deze nieuwe 'AzureTableStorage"gekoppeld servicetype voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
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

### <a name="use-shared-access-signature-authentication"></a>Shared access signature-verificatie gebruiken

U kunt ook een gekoppelde Storage-service met behulp van een shared access signature maken. Het biedt de data factory met beperkte/tijdsgebonden toegang tot alle of naar een specifiek resources in de opslag.

Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. U kunt deze gebruiken op een client beperkte machtigingen voor objecten in uw storage-account gedurende een opgegeven periode en met een opgegeven set machtigingen verlenen. U hebt geen delen van de toegangssleutels van uw account. De shared access signature is een URI die in de queryparameters alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource omvat. Voor toegang tot de storage-resources met de shared access signature, moet de client alleen worden doorgegeven in de handtekening voor gedeelde toegang tot de juiste constructor of methode. Zie voor meer informatie over handtekeningen voor gedeelde toegang, [handtekeningen voor gedeelde toegang: Inzicht in het shared access signature-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Data Factory ondersteunt nu zowel **service gedeelde toegangshandtekeningen** en **account gedeelde toegangshandtekeningen**. Zie voor meer informatie over deze twee typen en hoe u een van deze [typen handtekeningen voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). 

> [!TIP]
> U kunt de volgende PowerShell-opdrachten uitvoeren voor het genereren van een gedeelde-toegangshandtekening van de service voor uw opslagaccount. Vervang de tijdelijke aanduidingen en de benodigde machtiging verlenen.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Voor het gebruik van shared access signature-verificatie, worden de volgende eigenschappen ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureTableStorage**. |Ja |
| sasUri | Geef de shared access signature URI voor de Storage-resources, zoals blob, container of tabel. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure Integration Runtime of de zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Als u "AzureStorage" type gekoppelde service zijn gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u het gebruik van deze nieuwe 'AzureTableStorage"gekoppeld servicetype voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureTableStorage",
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

Wanneer u een shared access signature URI maakt, moet u de volgende punten overwegen:

- Geschikt voor lezen/schrijven-machtigingen instellen voor objecten die zijn gebaseerd op hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw data factory.
- Stel **verlooptijd** op de juiste wijze. Zorg ervoor dat de toegang tot de Storage-objecten niet binnen een actieve periode van de pijplijn verloopt.
- De URI moet worden gemaakt op het niveau van de rechtertabel op basis van de behoeften.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure Table-gegevensset.

Om gegevens te kopiëren naar en van Azure Table, stel de eigenschap type van de gegevensset in **AzureTable**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureTable**. |Ja |
| tableName |De naam van de tabel in de tabel storage database-instantie waarnaar de gekoppelde service naar verwijst. |Ja |

**Voorbeeld:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Table storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema door Data Factory

Data Factory bepaalt voor winkels schemavrije gegevens, zoals Azure-tabel, het schema in een van de volgende manieren:

* Als u de structuur van gegevens met behulp van de **structuur** eigenschap in de definitie van de Data Factory zich houdt aan deze structuur als het schema. In dit geval, als een rij geen waarde voor een kolom bevat, wordt een null-waarde opgegeven voor deze.
* Als u de structuur van de gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de Data Factory bepaalt welk schema met behulp van de eerste rij in de gegevens. Als de eerste rij niet het volledige schema bevat, worden sommige kolommen in dit geval ontbreekt in het resultaat van de kopieerbewerking.

Voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van gegevens met behulp van de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Azure Table-bron en sink.

### <a name="azure-table-as-a-source-type"></a>Azure-tabel als een brontype

Voor het kopiëren van gegevens uit Azure-tabel, stelt u het brontype in de kopieeractiviteit naar **AzureTableSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **AzureTableSource**. |Ja |
| azureTableSourceQuery |Gebruik de aangepaste Table storage-query om gegevens te lezen. Zie de voorbeelden in de volgende sectie. |Nee |
| azureTableSourceIgnoreTableNotFound |Hiermee wordt aangegeven of de uitzondering van de tabel niet bestaat.<br/>Toegestane waarden zijn **waar** en **False** (standaard). |Nee |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery-voorbeelden

Als de Azure Table-kolom van het type datum/tijd:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Als de Azure Table-kolom van het tekenreekstype:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

Als u de pijplijnparameter gebruikt, cast-conversie de datum / tijdwaarde naar de juiste indeling op basis van de vorige voorbeelden.

### <a name="azure-table-as-a-sink-type"></a>Azure-tabel als een sink-type

Om gegevens te kopiëren naar Azure-tabel, stelt u het sink-type in de kopieeractiviteit naar **AzureTableSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **AzureTableSink**. |Ja |
| azureTableDefaultPartitionKeyValue |De standaard partitiesleutelwaarde die kan worden gebruikt door de sink. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet is opgegeven, wordt 'AzureTableDefaultPartitionKeyValue' gebruikt als de partitiesleutel. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de recordsleutel. Als niet is opgegeven, gebruikt u een GUID voor elke rij. |Nee |
| azureTableInsertType |De modus voor het invoegen van gegevens in Azure-tabel. Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met de bijbehorende partitie-en recordsleutels hebben hun waarden vervangen of samenvoegen. <br/><br/>Toegestane waarden zijn **samenvoegen** (standaard) en **vervangen**. <br/><br> Deze instelling is van toepassing op het rijniveau van de niet het tabelniveau van de. Geen van beide optie verwijdert rijen in de uitvoertabel die niet zijn opgenomen in de invoer. Zie voor meer informatie over de werking van de instellingen voor samenvoegen en vervangen, [entiteit invoegen of samenvoegen](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen van entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Nee |
| WriteBatchSize |Voegt de gegevens in Azure-tabel wanneer writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn gehele getallen (aantal rijen). |Nee (de standaardinstelling is 10.000) |
| writeBatchTimeout |Voegt de gegevens in Azure-tabel wanneer writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn timespan. Een voorbeeld hiervan is "00: 20:00" (20 minuten). |Nee (de standaardwaarde is 90 seconden, standaardtime-out van de opslagclient) |

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

Een bronkolom toewijzen aan een doelkolom met behulp van de **"translator"** eigenschap voordat u de doelkolom als azureTablePartitionKeyName kunt gebruiken.

In het volgende voorbeeld is de bronkolom DivisionID toegewezen aan de doelkolom DivisionID:

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

## <a name="data-type-mapping-for-azure-table"></a>De gegevenstypetoewijzing voor Azure-tabel

Wanneer u gegevens van en naar Azure-tabel kopiëren, worden de volgende toewijzingen van Azure Table-gegevenstypen gebruikt om Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe de kopieerbewerking het schema en de gegevens van een brontype aan de sink toegewezen, [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

Wanneer u gegevens wilt verplaatsen naar en van Azure Table, de volgende [toewijzingen die zijn gedefinieerd door Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) worden gebruikt vanaf Azure tabel OData typen .NET-type en vice versa.

| Azure Table-gegevenstype | Data Factory tussentijdse gegevenstype | Details |
|:--- |:--- |:--- |
| Edm.Binary |byte[] |Een matrix van bytes maximaal 64 KB. |
| Edm.Boolean |bool |Een Booleaanse waarde. |
| Edm.DateTime |DateTime |Een 64-bits waarde wordt uitgedrukt als Coordinated Universal Time (UTC). Het ondersteunde bereik voor de datum/tijd begint middernacht, 1 januari 1601 A.D. (C.E.), UTC. Het bereik eindigt en met 31 December 9999. |
| Edm.Double |double |Een 64-bits drijvende-kommawaarde. |
| Edm.Guid |GUID |Een globaal unieke id van 128-bits. |
| Edm.Int32 |Int32 |Een 32-bits geheel getal zijn. |
| Edm.Int64 |Int64 |Een 64-bits geheel getal zijn. |
| Edm.String |Reeks |Een waarde UTF-16-codering. Tekenreekswaarden mag maximaal 64 KB. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
