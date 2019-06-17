---
title: Gekoppelde services in Azure Data Factory | Microsoft Docs
description: Meer informatie over gekoppelde services in Data Factory. Gekoppelde services berekenen/gegevensarchieven aan data factory koppelen.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: shlo
ms.openlocfilehash: ba2041495e1e3c63ee322a0b748753ad6cb68914
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870130"
---
# <a name="linked-services-in-azure-data-factory"></a>Gekoppelde services in Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-create-datasets.md)
> * [Huidige versie](concepts-datasets-linked-services.md)

Dit artikel wordt beschreven welke gekoppelde services bent, hoe ze worden gedefinieerd in JSON-indeling, en hoe ze worden gebruikt Azure Data Factory-pijplijnen.

Als u niet bekend bent met Data Factory, raadpleegt u [Inleiding tot Azure Data Factory](introduction.md) voor een overzicht.

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een **pijplijn** is een logische groepering van **activiteiten** die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren uit een on-premises SQL Server naar Azure Blob storage. Vervolgens kunt u een Hive-activiteit die een Hive-script uitvoert op een Azure HDInsight-cluster om gegevens te verwerken van Blob-opslag om uitvoergegevens te produceren. U kunt tot slot een tweede kopieeractiviteit gebruiken om het kopiëren van de uitvoergegevens naar Azure SQL Data Warehouse, boven op welke business intelligence (BI) reporting oplossingen zijn gemaakt. Zie voor meer informatie over pijplijnen en activiteiten [pijplijnen en activiteiten](concepts-pipelines-activities.md) in Azure Data Factory.

Nu een **gegevensset** is een benoemde weergave van gegevens die gewoon verwijst of verwijst naar de gegevens die u gebruiken wilt uw **activiteiten** als invoer en uitvoer.

Voordat u een gegevensset maakt, moet u een **gekoppelde service** uw gegevensopslag aan de data factory koppelen. Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Beschouw dit niet mogelijk. de structuur van de gegevens in de gekoppelde gegevensarchieven, vertegenwoordigt de gegevensset en de gekoppelde service definieert de verbinding met de gegevensbron. Bijvoorbeeld, gekoppelde een Azure Storage-service wordt een storage-account aan de data factory. Een Azure Blob-gegevensset vertegenwoordigt de blob-container en de map in het Azure storage-account met de blobs voor invoer om te worden verwerkt.

Hier volgt een voorbeeldscenario. Om gegevens te kopiëren van Blob-opslag met een SQL-database, moet u twee gekoppelde services maken: Azure Storage en Azure SQL Database. Vervolgens maakt u twee gegevenssets: Azure Blob-gegevensset (die verwijst naar de gekoppelde Azure Storage-service) en Azure SQL Table-gegevensset (die verwijst naar de gekoppelde Azure SQL Database-service). De Azure Storage en de gekoppelde Azure SQL Database-services bevatten verbindingsreeksen die Data Factory tijdens runtime gebruikt verbinding maken met uw Azure Storage en Azure SQL Database, respectievelijk. De Azure Blob-gegevensset specificeert de blob-container en de blob-map met de blobs voor invoer in uw Blob storage. De Azure SQL Table-gegevensset bevat de SQL-tabel in uw SQL-database waarnaar de gegevens zijn om te worden gekopieerd.

Het volgende diagram toont de relaties tussen de pijplijn, activiteit, gegevensset en gekoppelde service in Data Factory:

![Relatie tussen pijplijn, activiteit, gegevensset, gekoppelde services](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>Gekoppelde service JSON
Een gekoppelde service in Data Factory wordt in JSON-indeling als volgt gedefinieerd:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

De volgende tabel beschrijft de eigenschappen in de bovenstaande JSON:

Eigenschap | Description | Vereist |
-------- | ----------- | -------- |
name | De naam van de gekoppelde service. Zie [Azure Data Factory - naamgevingsregels](naming-rules.md). |  Ja |
type | Het type van de gekoppelde service. Bijvoorbeeld: AzureStorage (gegevensopslag) of AzureBatch (rekenen). Zie de beschrijving voor typeProperties. | Ja |
typeProperties | De type-eigenschappen zijn verschillend voor elk gegevensarchief of compute. <br/><br/> Voor de ondersteunde gegevens opslaat typen en hun type-eigenschappen, Zie de [gegevenssettype](concepts-datasets-linked-services.md#dataset-type) tabel in dit artikel. Ga naar het data store connector artikel voor meer informatie over de type-eigenschappen die specifiek zijn voor een gegevensarchief. <br/><br/> Zie voor de typen ondersteunde compute en hun eigenschappen [gekoppelde services berekenen](compute-linked-services.md). | Ja |
connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee

## <a name="linked-service-example"></a>Voorbeeld van de gekoppelde service
De volgende gekoppelde service is een gekoppelde Azure Storage-service. U ziet dat het type is ingesteld voor opslag van Azureverzonden. De eigenschappen van het type voor de gekoppelde Azure Storage-service zijn een verbindingsreeks. De Data Factory-service gebruikt deze verbindingsreeks verbinding maken met het gegevensarchief tijdens runtime.

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

## <a name="create-linked-services"></a>Gekoppelde services maken
U maakt gekoppelde services met behulp van een van deze hulpprogramma's of de SDK's: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST-API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager-sjabloon en Azure-portal

## <a name="data-store-linked-services"></a>Gekoppelde services voor gegevensopslag
Verbinding maken met gegevensarchieven kunt u vinden in onze [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats). Verwijzen naar de lijst voor specifieke verbinding-eigenschappen die nodig zijn voor verschillende winkels.

## <a name="compute-linked-services"></a>Gekoppelde services berekenen
Naslaginformatie over [omgevingen ondersteund compute](compute-linked-services.md) voor meer informatie over verschillende omgevingen u verbinding kunt maken vanuit uw data factory, evenals de verschillende configuraties.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van pijplijnen en gegevenssets met behulp van een van deze hulpprogramma's of de SDK's.

- [Snelstartgids: een gegevensfactory maken met .NET](quickstart-create-data-factory-dot-net.md)
- [Snelstartgids: een data factory maken met PowerShell](quickstart-create-data-factory-powershell.md)
- [Snelstartgids: een data factory maken met REST-API](quickstart-create-data-factory-rest-api.md)
- [Snelstartgids: een data factory maken met Azure portal](quickstart-create-data-factory-portal.md)
