---
title: Azure Table storage gebruiken met behulp van Python | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.openlocfilehash: 0c46f04786ba4b62bd7ca22c5e25643123e6e136
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-in-python"></a>Table storage gebruiken in Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's voor Azure Table-opslag om in te voeren met behulp van Python de [Microsoft Azure-opslag-SDK voor Python](https://github.com/Azure/azure-storage-python). De scenario's worden behandeld bevatten maken en verwijderen van een tabel en het invoegen en het opvragen van entiteiten.

Tijdens het werken met de scenario's in deze zelfstudie kunt u desgewenst om te verwijzen naar de [Azure-opslag-SDK voor Python-API-referentiemateriaal](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a>De Azure Storage-SDK voor Python installeren

Als u een opslagaccount hebt gemaakt, wordt de volgende stap is het installeren van de [Microsoft Azure-opslag-SDK voor Python](https://github.com/Azure/azure-storage-python). Raadpleeg voor meer informatie over het installeren van de SDK de [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) bestand in de opslag-SDK voor Python-opslagplaats op GitHub.

## <a name="create-a-table"></a>Een tabel maken

Met de service Azure Table in Python wilt werken, moet u importeren de [TableService] [ py_TableService] module. Omdat u met tabelentiteiten werken gaat, moet u ook de [entiteit] [ py_Entity] klasse. Voeg deze code boven uw Python-bestand voor het importeren van beide:

```python
from azure.storage.table import TableService, Entity
```

Maak een [TableService] [ py_TableService] -object doorgeven in een naam en sleutel van uw opslagaccount. Vervang `myaccount` en `mykey` met uw accountnaam en -sleutel en aanroep [create_table] [ py_create_table] naar de tabel maken in Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen, maakt u eerst een object die uw entiteit vertegenwoordigt, en vervolgens geeft het object de [TableService][py_TableService].[ insert_entity] [ py_insert_entity] methode. Het entiteitsobject mag een woordenboek of een object van type [entiteit][py_Entity], en de namen en waarden van uw entiteit definieert. Elke entiteit moet bevatten de vereiste [PartitionKey en RowKey](#partitionkey-and-rowkey) eigenschappen, naast de andere eigenschappen die u voor de entiteit definieert.

In dit voorbeeld maakt u een dictionary-object dat een entiteit vertegenwoordigt vervolgens doorgegeven aan de [insert_entity] [ py_insert_entity] methode toe te voegen aan de tabel:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

In dit voorbeeld wordt een [entiteit] [ py_Entity] object en vervolgens doorgegeven aan de [insert_entity] [ py_insert_entity] methode toe te voegen aan de tabel:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey en RowKey

U moet opgeven dat zowel een **PartitionKey** en een **RowKey** eigenschap voor elke entiteit. Dit als samen de unieke id's van uw entiteiten, zijn ze de primaire sleutel van een entiteit vormen. U kunt een query met deze waarden veel sneller dan u kunt een andere entiteitseigenschappen query omdat alleen deze eigenschappen zijn geïndexeerd.

De tabel-service wordt gebruikt **PartitionKey** voor de distributie op intelligente wijze tabelentiteiten over opslagknooppunten. Entiteiten met dezelfde **PartitionKey** op hetzelfde knooppunt worden opgeslagen. **RowKey** is de unieke ID van de entiteit in de partitie hoort bij.

## <a name="update-an-entity"></a>Bijwerken van een entiteit

Aanroepen voor het bijwerken van alle waarden van de eigenschap van een entiteit, de [update_entity] [ py_update_entity] methode. In dit voorbeeld ziet u hoe u een bestaande entiteit vervangen door een bijgewerkte versie:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Als de entiteit die wordt bijgewerkt, niet al bestaat, wordt de update-bewerking mislukt. Als u wilt opslaan van een entiteit of deze of niet bestaat, gebruiken [insert_or_replace_entity][py_insert_or_replace_entity]. In het volgende voorbeeld wordt de eerste aanroep van de bestaande entiteit vervangen. De tweede aanroep wordt een nieuwe entiteit worden ingevoegd omdat er geen entiteit met de opgegeven PartitionKey en RowKey bestaat in de tabel.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> De [update_entity] [ py_update_entity] methode vervangt alle eigenschappen en waarden van een bestaande entiteit, u ook gebruiken kunt om eigenschappen te verwijderen uit een bestaande entiteit. U kunt de [merge_entity] [ py_merge_entity] methode voor het bijwerken van een bestaande entiteit met nieuwe of gewijzigde eigenschapswaarden zonder de entiteit volledig te vervangen.

## <a name="modify-multiple-entities"></a>Meerdere entiteiten wijzigen

U kunt meerdere bewerkingen samen in een batch te verzenden zodat de atomic verwerking van een aanvraag door de tabel-service. Gebruik eerst de [TableBatch] [ py_TableBatch] klasse meerdere bewerkingen toevoegen aan één batch. Daarna roept [TableService][py_TableService].[ commit_batch] [ py_commit_batch] verzenden van de bewerkingen in een atomic-bewerking. Alle entiteiten in batch worden gewijzigd, moeten zich in dezelfde partitie.

In dit voorbeeld voegt twee entiteiten samen in een batch:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Batches kunnen ook worden gebruikt met de syntaxis van de manager context:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Query voor een entiteit

Om te vragen voor een entiteit in een tabel, de PartitionKey en RowKey naar doorgeven de [TableService][py_TableService].[ get_entity] [ py_get_entity] methode.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Query uitvoeren op een verzameling entiteiten

U kunt een query voor een aantal entiteiten door het opgeven van een filtertekenreeks in met de **filter** parameter. In dit voorbeeld worden alle taken in Haarlem gevonden door een filter toepassen op PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen

U kunt ook beperken welke eigenschappen voor elke entiteit in een query zijn geretourneerd. Deze methode, aangeroepen *projectie*, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral voor grote entiteiten of resultatensets. Gebruik de **Selecteer** parameter en de namen van de eigenschappen die u wilt dat wordt geretourneerd naar de client op te geven.

De query in de volgende code retourneert alleen de beschrijvingen van entiteiten in de tabel.

> [!NOTE]
> Het volgende fragment werkt alleen op basis van de Azure-opslag. Dit wordt niet ondersteund door de opslagemulator.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen

Een entiteit verwijderen door de PartitionKey en RowKey naar de [delete_entity] [ py_delete_entity] methode.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u een tabel of een van de entiteiten in het niet meer nodig hebt, belt u het [delete_table] [ py_delete_table] methode voor de tabel permanent verwijderen uit Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Volgende stappen

* [Azure-opslag-SDK voor Python-API-referentiemateriaal](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Azure-opslag-SDK voor Python](https://github.com/Azure/azure-storage-python)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): een gratis, platformoverschrijdende-toepassing voor visueel werken met Azure Storage-gegevens op Windows-, Mac OS- en Linux.

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch
