---
title: Azure Table Storage en Azure Cosmos DB tabel API gebruiken met Ruby | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 02/27/2018
ms.author: mimig
ms.openlocfilehash: 104d793826116462f71e4889386906256b2df8f8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-ruby"></a>Azure Table Storage en Azure Cosmos DB tabel API gebruiken met Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de Azure Table-service en de API van Azure Cosmos DB tabel uitvoeren. De voorbeelden zijn geschreven in Ruby en gebruik de [Azure tabel Opslagclientbibliotheek voor Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). De scenario's worden behandeld: **maken en verwijderen van een tabel en het invoegen en het opvragen van entiteiten in een tabel**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="add-access-to-storage-or-azure-cosmos-db"></a>Toegang toevoegen aan Storage of Azure Cosmos-DB
U moet voor het gebruik van Azure Storage of Azure Cosmos DB, downloaden en gebruiken van het Ruby Azure-pakket bevat een set met gemak bibliotheken die met de REST van de tabel-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Type **gem installeren azure-opslag-tabel** in het opdrachtvenster voor het installeren van de gem en afhankelijkheden.

### <a name="import-the-package"></a>Het pakket importeren
Uw favoriete teksteditor gebruiken, Voeg het volgende toe aan het begin van de Ruby bestand waarop u wilt opslag gebruiken:

```ruby
require "azure/storage/table"
```

## <a name="add-an-azure-storage-connection"></a>Een Azure Storage-verbinding toevoegen
De Azure Storage-module leest de omgevingsvariabelen **AZURE_STORAGE_ACCOUNT** en **AZURE_STORAGE_ACCESS_KEY** voor de benodigde informatie om te verbinden met uw Azure Storage-account. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens voordat u **Azure::Storage::Table::TableService** met de volgende code:

```ruby
Azure.config.storage_account_name = "<your Azure Storage account>"
Azure.config.storage_access_key = "<your Azure Storage access key>"
```

Als u deze waarden van een klassiek of Resource Manager-opslagaccount in de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het opslagaccount dat u wilt gebruiken.
3. Klik op de blade instellingen aan de rechterkant **toegangstoetsen**.
4. In de blade van de toegang tot sleutels die wordt weergegeven, ziet u de toegangssleutel 1 en toegangssleutel 2. U kunt een van beide gebruiken.
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren.

## <a name="add-an-azure-cosmos-db-connection"></a>Een Azure Cosmos DB-verbinding toevoegen
De primaire verbindingsreeks kopiëren vanuit de Azure-portal voor verbinding met Azure Cosmos DB, en maak een **Client** object met de gekopieerde verbindingsreeks. U kunt doorgeven de **Client** object bij het maken van een **TableService** object:

```ruby
common_client = Azure::Storage::Common::Client.create(storage_account_name:'myaccount', storage_access_key:'mykey', storage_table_host:'mycosmosdb_endpoint')
table_client = Azure::Storage::Table::TableService.new(client: common_client)
```

## <a name="create-a-table"></a>Een tabel maken
De **Azure::Storage::Table::TableService** object kunt u samenwerken met tabellen en entiteiten. Een tabel te maken, gebruikt u de **create_table()** methode. Het volgende voorbeeld maakt een tabel of de fout wordt afgedrukt indien deze aanwezig is.

```ruby
azure_table_service = Azure::Storage::Table::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen, moet u eerst een hash-object dat de entiteitseigenschappen van uw definieert maken. Houd er rekening mee dat voor elke entiteit moet u een **PartitionKey** en **RowKey**. Dit zijn de unieke id's van de entiteiten en waarden die veel sneller dan de andere eigenschappen kunnen worden opgevraagd. Azure Storage maakt gebruik van **PartitionKey** automatisch over veel opslagknooppunten verdelen van de tabel entiteiten. Entiteiten met dezelfde **PartitionKey** op hetzelfde knooppunt worden opgeslagen. De **RowKey** is de unieke ID van de entiteit in de partitie hoort bij.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Bijwerken van een entiteit
Er zijn meerdere methoden beschikbaar voor het bijwerken van een bestaande entiteit:

* **update_entity():** bijwerken van een bestaande entiteit door deze te vervangen.
* **merge_entity():** Updates van een bestaande entiteit door nieuwe eigenschapswaarden in de bestaande entiteit samen te voegen.
* **insert_or_merge_entity():** Updates van een bestaande entiteit door deze te vervangen. Als er is geen entiteit bestaat, wordt er een nieuwe ingevoegd:
* **insert_or_replace_entity():** Updates van een bestaande entiteit door nieuwe eigenschapswaarden in de bestaande entiteit samen te voegen. Als er is geen entiteit bestaat, wordt er een nieuwe ingevoegd.

Het volgende voorbeeld toont het bijwerken van een entiteit met **update_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

Met **update_entity()** en **merge_entity()**, als de entiteit die u wilt bijwerken, mislukt de update-bewerking niet bestaat. Dus als u opslaan van een entiteit wilt ongeacht of deze al bestaat, moet u in plaats daarvan gebruiken **insert_or_replace_entity()** of **insert_or_merge_entity()**.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten
Soms is het zinvol verzenden meerdere bewerkingen samen in een batch om ervoor te zorgen atomic verwerking door de server. Om te realiseren dat, maakt u eerst een **Batch** object en gebruik vervolgens de **execute_batch()** methode op **TableService**. Het volgende voorbeeld ziet twee entiteiten met RowKey 2 en 3 in een batch te verzenden. U ziet dat deze functie alleen voor entiteiten met de dezelfde PartitionKey werkt.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Query voor een entiteit
Om te vragen een entiteit in een tabel, gebruikt u de **get_entity()** methode door de naam van de tabel **PartitionKey** en **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Query uitvoeren op een verzameling entiteiten
Om te vragen een set van entiteiten in een tabel, een query-hash-object maken en gebruiken de **query_entities()** methode. Het volgende voorbeeld toont ophalen van de entiteiten met dezelfde **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Als de resultatenset te groot voor één query op om te retourneren is, wordt een vervolgtoken geretourneerd dat u gebruiken kunt voor het ophalen van de volgende pagina's.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen
Een query naar een tabel ophalen slechts enkele eigenschappen van een entiteit. Deze techniek, 'projectie genoemd,' verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. Gebruik de component select en geeft de namen van de eigenschappen die u wilt worden overgebracht naar de client.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U een entiteit verwijderen met de **delete_entity()** methode. Naam van de tabel die de entiteit, de PartitionKey en de RowKey van de entiteit bevat die worden doorgegeven.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Als u wilt verwijderen van een tabel, gebruikt u de **delete_table()** methode aan en geeft u op de naam van de tabel die u wilt verwijderen.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Volgende stappen

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Ruby-ontwikkelaars](https://azure.microsoft.com/develop/ruby/)
* [Microsoft Azure tabel Opslagclientbibliotheek voor Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table) 

