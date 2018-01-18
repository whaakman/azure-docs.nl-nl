---
title: Azure Table Storage gebruiken met Ruby | Microsoft Docs
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
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: decc6ffb38a4358d3593642f9cedb59d08f6bfef
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-with-ruby"></a>Azure Table storage gebruiken met Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met de service Azure Table uitvoeren. De voorbeelden zijn geschreven met behulp van de Ruby-API. De scenario's worden behandeld: **maken en verwijderen van een tabel, invoegen en het uitvoeren van query's entiteiten in een tabel**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken
Voor instructies hoe een Ruby-toepassing maken, Zie [Ruby op Rails webtoepassing op een virtuele machine van Azure](../virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag
Voor het gebruik van Azure Storage, die u wilt downloaden en gebruiken van het Ruby azure-pakket met een set van gemak bibliotheken die met de REST van de Storage-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Type **gem installeren azure** in het opdrachtvenster voor het installeren van de gem en afhankelijkheden.

### <a name="import-the-package"></a>Het pakket importeren
Uw favoriete teksteditor gebruiken, Voeg het volgende toe aan het begin van de Ruby bestand waarop u wilt opslag gebruiken:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen
De azure-module leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_toegang\_sleutel** voor de benodigde informatie om te verbinden met uw Azure Storage-account. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens voordat u **Azure::TableService** met de volgende code:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

Als u deze waarden van een klassiek of Resource Manager-opslagaccount in de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het opslagaccount dat u wilt gebruiken.
3. Klik op de blade instellingen aan de rechterkant **toegangstoetsen**.
4. In de blade van de toegang tot sleutels die wordt weergegeven, ziet u de toegangssleutel 1 en toegangssleutel 2. U kunt een van beide gebruiken.
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren.

## <a name="create-a-table"></a>Een tabel maken
De **Azure::TableService** object kunt u samenwerken met tabellen en entiteiten. Een tabel te maken, gebruikt u de **maken\_table()** methode. Het volgende voorbeeld maakt een tabel of de fout wordt afgedrukt indien deze aanwezig is.

```ruby
azure_table_service = Azure::TableService.new
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

* **bijwerken\_entity():** bijwerken van een bestaande entiteit door deze te vervangen.
* **samenvoegen\_entity():** Updates van een bestaande entiteit door nieuwe eigenschapswaarden in de bestaande entiteit samen te voegen.
* **invoegen\_of\_samenvoegen\_entity():** Updates van een bestaande entiteit door deze te vervangen. Als er is geen entiteit bestaat, wordt er een nieuwe ingevoegd:
* **invoegen\_of\_vervangen\_entity():** Updates van een bestaande entiteit door nieuwe eigenschapswaarden in de bestaande entiteit samen te voegen. Als er is geen entiteit bestaat, wordt er een nieuwe ingevoegd.

Het volgende voorbeeld toont het bijwerken van een entiteit met **bijwerken\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

Met **bijwerken\_entity()** en **samenvoegen\_entity()**, als de entiteit die u wilt bijwerken, mislukt de update-bewerking niet bestaat. Dus als u opslaan van een entiteit wilt ongeacht of deze al bestaat, moet u in plaats daarvan gebruiken **invoegen\_of\_vervangen\_entity()** of **invoegen\_of\_samenvoegen\_entity()**.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten
Soms is het zinvol verzenden meerdere bewerkingen samen in een batch om ervoor te zorgen atomic verwerking door de server. Om te realiseren dat, maakt u eerst een **Batch** object en gebruik vervolgens de **uitvoeren\_batch()** methode op **TableService**. Het volgende voorbeeld ziet twee entiteiten met RowKey 2 en 3 in een batch te verzenden. U ziet dat deze functie alleen voor entiteiten met de dezelfde PartitionKey werkt.

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
Om te vragen een entiteit in een tabel, gebruikt u de **ophalen\_entity()** methode door de naam van de tabel **PartitionKey** en **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Query uitvoeren op een verzameling entiteiten
Om te vragen een set van entiteiten in een tabel, een query-hash-object maken en gebruiken de **query\_entities()** methode. Het volgende voorbeeld toont ophalen van de entiteiten met dezelfde **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Als het resultaat is te groot voor één query op om te retourneren, een vervolgtoken wordt geretourneerd dat u kunt gebruiken voor het ophalen van de volgende pagina's.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen
Een query naar een tabel ophalen slechts enkele eigenschappen van een entiteit. Deze techniek, 'projectie' genoemd, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. Gebruik de component select en geeft de namen van de eigenschappen die u wilt worden overgebracht naar de client.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U een entiteit verwijderen met de **verwijderen\_entity()** methode. U moet doorgeven in de naam van de tabel die de entiteit, de PartitionKey en RowKey van de entiteit bevat.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Als u wilt verwijderen van een tabel, gebruikt u de **verwijderen\_table()** methode aan en geeft u op de naam van de tabel die u wilt verwijderen.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Volgende stappen

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Azure SDK voor Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) opslagplaats op GitHub

