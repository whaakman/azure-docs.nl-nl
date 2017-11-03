---
title: "Gegevens kopiëren naar/van Azure Table Storage met behulp van de Data Factory | Microsoft Docs"
description: "Ontdek hoe gegevens van ondersteunde bron winkels naar Azure Table Storage (of) van Table Storage kopiëren voor ondersteunde sink stores met behulp van de Data Factory."
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
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: 2fa03b82750585454430da0c29392db57b20d3c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-to-or-from-azure-table-using-azure-data-factory"></a>Gegevens kopiëren naar of van de Azure-tabel met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-azure-table-connector.md)
> * [Versie 2 - Preview](connector-azure-table-storage.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren naar en van Azure Table. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Azure Table Storage-connector in V1](v1/data-factory-azure-table-connector.md).

## <a name="supported-scenarios"></a>Ondersteunde scenario's

U kunt gegevens van alle ondersteunde brongegevensarchief kopiëren naar Azure Table of gegevens kopiëren van Azure Table naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder deze Azure Table-connector ondersteunt de kopiëren van gegevens met zowel **accountsleutel** en **Service-SAS** authenticaties (Shared Access Signature).

## <a name="get-started"></a>Aan de slag
U kunt een pijplijn maken met de kopieeractiviteit middels de .NET SDK, Python SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [kopie activiteit zelfstudie](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Table Storage.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

### <a name="using-account-key"></a>Met behulp van sleutel-account

U kunt een gekoppelde Azure Storage-service maken met behulp van de accountsleutel, die de gegevensfactory met globale toegang tot Azure Storage biedt. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met Azure-opslag voor de eigenschap connectionString. Dit veld markeren als SecureString. |Ja |
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
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-service-sas-authentication"></a>Service-SAS-verificatie

U kunt ook een gekoppelde Azure Storage-service maken met behulp van een Shared Access Signature (SAS), waardoor de gegevensfactory met beperkte/tijdsgebonden toegang tot alle/specifieke bronnen in de opslag.

Een Shared Access Signature (SAS) biedt gedelegeerde toegang tot bronnen in uw opslagaccount. Hiermee kunt u dat een client beperkte machtigingen voor objecten in uw storage-account gedurende een bepaalde tijd en met een opgegeven set machtigingen, zonder dat voor het delen van de toegangssleutels van uw account verlenen. De SAS is een URI die de gegevens die zijn vereist voor geverifieerde toegang tot een opslagresource, in de queryparameters omvat. Voor toegang tot de storage-resources met de SA's, moet de client alleen worden doorgegeven in de SA's aan de juiste constructor of methode. Zie voor gedetailleerde informatie over SAS [Shared Access Signatures: inzicht in het SAS-Model](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure Data Factory nu ondersteunt alleen het **Service-SAS** maar geen Account-SAS. Zie [typen van handtekeningen voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen en bouwen. De SAS-URL generable vanuit Azure-portal of Opslagverkenner is een SAS-Account, wat niet wordt ondersteund.
>

Service-SAS om verificatie te gebruiken, zijn de volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| sasUri | Shared Access Signature URI voor de Azure Storage-resources zoals blob-container of tabel opgeven. Dit veld markeren als SecureString. |Ja |
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
- URI moet worden gemaakt op het niveau van de rechtertabel op basis van de behoeften.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Table-gegevensset.

Om gegevens te kopiëren naar/van Azure Table, stel de eigenschap type van de gegevensset **AzureTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **AzureTable** |Ja |
| tableName |De naam van de tabel in de Azure-tabel Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

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

Voor gegevens zonder schema winkels zoals Azure Table afleidt de Data Factory-service het schema in een van de volgende manieren:

1. Als u de structuur van gegevens met behulp van opgeven de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt in dit geval een null-waarde opgegeven voor deze.
2. Als u de structuur van gegevens niet via opgeeft de **structuur** het schema-eigenschap in de definitie van de gegevensset, Data Factory afleidt met behulp van de eerste rij in de gegevens. Als de eerste rij niet het volledige schema bevat, worden sommige kolommen in dit geval wordt opgehaald in het resultaat van de kopieerbewerking.

Daarom voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van gegevens met de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Azure Table-bron- en sink.

### <a name="azure-table-as-source"></a>Azure-tabel als bron

Om gegevens te kopiëren van Azure Table, stelt u het brontype in de kopieerbewerking naar **AzureTableSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureTableSource** |Ja |
| azureTableSourceQuery |Gebruik de aangepaste Azure table-query om gegevens te lezen. Zie de voorbeelden in de volgende sectie. |Nee |
| azureTableSourceIgnoreTableNotFound |Geef aan of de uitzondering van de tabel slikken bestaat niet.<br/>Toegestane waarden zijn: **True**, en **False** (standaard). |Nee |

### <a name="azuretablesourcequery-examples"></a>Voorbeelden van azureTableSourceQuery

Als Azure Table-kolom van het tekenreekstype:

```json
"azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', <datetime parameter>)"
```

Als Azure Table-kolom van het type datetime:

```json
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', <datetime parameter>, <datetime parameter>)"
```

### <a name="azure-table-as-sink"></a>Azure-tabel als sink

Om gegevens te kopiëren van Azure Table, stelt u het brontype in de kopieerbewerking naar **AzureTableSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzureTableSink** |Ja |
| azureTableDefaultPartitionKeyValue |Standaardwaarde voor de partitiesleutel die kan worden gebruikt door de sink. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet wordt opgegeven, wordt 'AzureTableDefaultPartitionKeyValue' gebruikt als de partitiesleutel. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet wordt opgegeven, gebruikt u een GUID voor elke rij. |Nee |
| azureTableInsertType |De modus voor gegevens in Azure-tabel invoegen. Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met partitie-als rijsleutels die overeenkomt met de waarden vervangen of samenvoegen van hebben. <br/><br/>Toegestane waarden zijn: **samenvoegen** (standaard), en **vervangen**. <br/><br> Deze instelling is van toepassing op het rijniveau van niet tabelniveau, en geen van beide optie worden de rijen in de uitvoertabel die niet bestaan in de invoer verwijderd. Zie voor meer informatie over de werking van deze instellingen (merge en vervangen), [invoegen of Merge entiteit](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. |Nee |
| writeBatchSize |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen) |Nee (de standaardwaarde is 10000) |
| writeBatchTimeout |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 20:00 ' (20 minuten) |Nee (de standaardwaarde is 90 sec - opslag-client standaardtime-out) |

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

Een bronkolom worden toegewezen aan een doelkolom met behulp van de eigenschap 'vertaler' voordat u de doelkolom als de azureTablePartitionKeyName gebruiken kunt.

In het volgende voorbeeld wordt de bronkolom DivisionID toegewezen aan de doelkolom DivisionID.

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

De 'DivisionID' is opgegeven als de partitiesleutel.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>De gegevenstypetoewijzing voor Azure Table

Bij het kopiëren van gegevens van/naar Azure Table worden de volgende toewijzingen van Azure Table-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

Wanneer u gegevens verplaatst naar & van Azure-tabel, de volgende [toewijzingen die zijn gedefinieerd door de service Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) worden gebruikt vanuit Azure tabel OData-typen aan .NET-type en vice versa.

| Azure Table-gegevenstype | Data factory tussentijdse gegevenstype | Details |
|:--- |:--- |:--- |
| Edm.Binary |Byte] |Een matrix met bytes maximaal 64 KB. |
| Edm.Boolean |BOOL |Een Booleaanse waarde. |
| Edm.DateTime |Datum/tijd |Een 64-bits waarde wordt uitgedrukt als Coordinated Universal Time (UTC). Het ondersteunde bereik van de datum-/ begint vanaf 12:00 middernacht, 1 januari 1601 A.D. (C.E.) UTC. Het bereik eindigt op 31 December 9999. |
| Edm.Double |dubbele |Een 64-bits drijvende-kommawaarde. |
| Edm.Guid |GUID |Een globally unique identifier van 128-bits. |
| Edm.Int32 |Int32 |Een 32-bits geheel getal. |
| Edm.Int64 |Int64 |Een 64-bits geheel getal. |
| Edm.String |Tekenreeks |Een waarde UTF-16-codering. Tekenreekswaarden mogelijk maximaal 64 KB. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).