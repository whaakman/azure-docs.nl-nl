---
title: Importeren van gegevens voor gebruik met de Azure-API voor tabel Cosmos DB | Microsoft Docs
description: Meer informatie over hoe de gegevens voor gebruik met de Azure-API voor tabel Cosmos-database importeren.
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: b60743e2-0227-43ab-965a-0ae3ebacd917
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: mimig
ms.openlocfilehash: 1c53be736ad65a53767626033be27f0891de06ba
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="import-data-for-use-with-the-azure-cosmos-db-table-api"></a>Gegevens importeren voor gebruik met de Azure-API voor tabel Cosmos DB

Deze zelfstudie vindt u instructies voor het importeren van gegevens voor gebruik met de Azure DB die Cosmos [tabel API](table-introduction.md). Als u gegevens die zijn opgeslagen in Azure Table storage hebt, kunt u het hulpprogramma voor migratie van gegevens of AzCopy om uw gegevens te importeren. Als u gegevens die zijn opgeslagen in een Azure-API voor het tabel van de Cosmos-database (preview)-account hebt, moet u het hulpprogramma voor migratie van gegevens om uw gegevens te migreren. Nadat uw gegevens worden geïmporteerd, kunt u zult kunnen profiteren van de premium-mogelijkheden biedt voor Azure Cosmos DB, zoals klare globale distributie, speciale doorvoer, één cijfer milliseconde latenties op het 99th percentiel, hoge beschikbaarheid, gegarandeerd en automatische secundaire indexeren.

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Het importeren van gegevens met het hulpprogramma voor migratie van gegevens
> * Gegevens importeren met AzCopy
> * Migreren van de tabel-API (preview) naar tabel-API 

## <a name="data-migration-tool"></a>Hulpprogramma voor migratie van gegevens

Het opdrachtregelprogramma van Azure Cosmos DB Data Migration (dt.exe) kan worden gebruikt om uw bestaande Azure Table storage-gegevens importeren in een tabel API GA-account of migreren van gegevens uit een tabel-API (preview)-account in een tabel API GA-account. Andere bronnen worden momenteel niet ondersteund. De gebruikersinterface op basis van de gegevensmigratie hulpprogramma (dtui.exe) wordt momenteel niet ondersteund voor tabel-API-accounts. 

Voor een migratie van gegevens in een tabel, moet u de volgende taken uitvoeren:

1. Download het hulpprogramma voor migratie van [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool).
2. Voer `dt.exe` met behulp van de opdrachtregelargumenten voor uw scenario.

DT.exe heeft een opdracht in de volgende indeling:

    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 

Opties voor de opdracht zijn:

    /ErrorLog: Optional. Name of the CSV file to redirect data transfer failures
    /OverwriteErrorLog: Optional. Overwrite error log file
    /ProgressUpdateInterval: Optional, default is 00:00:01. Time interval to refresh on-screen data transfer progress
    /ErrorDetails: Optional, default is None. Specifies that detailed error information should be displayed for the following errors: None, Critical, All

### <a name="command-line-source-settings"></a>Instellingen voor de opdrachtregel

Gebruik de volgende opties bij het definiëren van Azure Table Storage of tabel API preview als de bron van de migratie.

    /s:AzureTable: Reads data from Azure Table storage
    /s.ConnectionString: Connection string for the table endpoint. This can be retrieved from the Azure portal
    /s.LocationMode: Optional, default is PrimaryOnly. Specifies which location mode to use when connecting to Azure Table storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
    /s.Table: Name of the Azure Table
    /s.InternalFields: Set to All for table migration as RowKey and PartitionKey are required for import.
    /s.Filter: Optional. Filter string to apply
    /s.Projection: Optional. List of columns to select

Als u wilt de bron-verbindingsreeks ophalen bij het importeren uit Azure Table storage, de Azure portal openen en klik op **opslagaccounts** > **Account**  >   **Toegangssleutels**, en vervolgens met de knop kopiëren kopieert de **verbindingsreeks**.

![Schermopname van HBase-opties voor gegevensbron](./media/table-import/storage-table-access-key.png)

Als u wilt de bron-verbindingsreeks ophalen bij het importeren van een Azure-API voor het tabel van de Cosmos-database (preview)-account, opent u de Azure portal op **Azure Cosmos DB** > **Account**  >  **Verbindingsreeks** en gebruik de knop kopiëren om te kopiëren de **verbindingsreeks**.

![Schermopname van HBase-opties voor gegevensbron](./media/table-import/cosmos-connection-string.png)

[Azure Table Storage voorbeeldopdracht](#azure-table-storage)

[Voorbeeldopdracht Azure Cosmos DB tabel-API (preview)](#table-api-preview)

### <a name="command-line-target-settings"></a>Opdrachtregelprogramma doelinstellingen

Gebruik de volgende doel opties bij het definiëren van de API van Azure Cosmos DB tabel als het doel van de migratie.

    /t:TableAPIBulk: Uploads data into Azure CosmosDB Table in batches
    /t.ConnectionString: Connection string for the table endpoint
    /t.TableName: Specifies the name of the table to write to
    /t.Overwrite: Optional, default is false. Specifies if existing values should be overwritten
    /t.MaxInputBufferSize: Optional, default is 1GB. Approximate estimate of input bytes to buffer before flushing data to sink
    /t.Throughput: Optional, service defaults if not specified. Specifies throughput to configure for table
    /t.MaxBatchSize: Optional, default is 2MB. Specify the batch size in bytes

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Voorbeeld van de opdracht: bron is Azure Table storage

Hier volgt een voorbeeld van een opdrachtregel importeren uit Azure Table storage API van de tabel wordt weergegeven:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```
<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Voorbeeld van de opdracht: bron is Azure Cosmos DB tabel-API (preview)

Hier volgt een voorbeeld van het opdrachtregelprogramma voor het importeren van tabel API preview naar tabel-API Ondersteunt:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="azcopy-command"></a>AzCopy-opdracht

Met het AzCopy-opdrachtregelprogramma is de andere optie voor het migreren van gegevens van Azure Table storage naar de Azure-API voor tabel Cosmos DB. Voor het gebruik van AzCopy moet u eerst uw gegevens exporteren zoals beschreven in [gegevens exporteren uit de tabelopslag](../storage/common/storage-use-azcopy.md#export-data-from-table-storage), de gegevens vervolgens importeren in Azure Cosmos DB zoals beschreven in [Azure Cosmos DB tabel API](../storage/common/storage-use-azcopy.md#import-data-into-table-storage).

Bij het uitvoeren van de invoer in Azure Cosmos DB, raadpleegt u het volgende voorbeeld. Houd er rekening mee dat de waarde/dest cosmosdb, geen core gebruikt.

Opdracht importeren:

```
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Migreren van de tabel-API (preview) naar tabel-API

> [!WARNING]
> Als u wilt onmiddellijk profiteren van de algemeen beschikbaar tabellen en Migreer uw bestaande preview tabellen zoals opgegeven in deze sectie, anders we gaan uitvoeren automatisch migraties voor bestaande klanten preview in de komende weken, houd er rekening mee maar die automatisch gemigreerde hebben preview tabellen bepaalde beperkingen toe die nieuw tabellen wordt niet gemaakt.
> 

De tabel-API is nu algemeen beschikbaar (GA). Er zijn verschillen tussen de preview en GA versies van tabellen in de code die wordt uitgevoerd in de cloud, evenals in code die wordt uitgevoerd op de client. Daarom niet aanbevolen om te proberen voor een preview-SDK-client te combineren met een account GA tabel API en vice versa. Tabel API preview-klanten wilt blijven gebruiken hun bestaande tabellen, maar moet migreren van de evaluatieversie naar de GA-omgeving of wacht voor automatische migratie in een productieomgeving. Als u automatische migratie wachten, wordt u wordt gewaarschuwd van de beperkingen van de gemigreerde tabellen. Na de migratie, kunt u zich kunt maken van nieuwe tabellen op uw bestaande account zonder beperkingen (alleen gemigreerde tabellen beperkingen moeten).

Migreren van tabel-API (preview) voor de algemeen beschikbaar tabel-API:

1. Een nieuw Azure DB die Cosmos-account maken en instellen van het type API op Azure Table, zoals beschreven in [een databaseaccount maken](create-table-dotnet.md#create-a-database-account).

2. Wijzigen van clients voor gebruik van een GA-release van de [tabel API SDK's](table-sdk-dotnet.md).

3. De clientgegevens migreren vanaf preview tabellen aan GA tabellen met behulp van het hulpprogramma voor migratie van gegevens. Instructies voor het gebruik van het hulpprogramma voor migratie van gegevens voor dit doel worden beschreven in [hulpprogramma voor gegevensmigratie](#data-migration-tool). 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Gegevens importeren met het hulpprogramma voor migratie van gegevens
> * Gegevens importeren met AzCopy
> * Migreren van de tabel-API (preview) naar tabel-API

U kunt nu doorgaan met de volgende zelfstudie en informatie over het opvragen van gegevens met de Azure-API voor tabel Cosmos DB. 

> [!div class="nextstepaction"]
>[Hoe kan ik een query over gegevens?](../cosmos-db/tutorial-query-table.md)
