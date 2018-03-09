---
title: Aan de slag met Azure Table storage met behulp van Python | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/08/2018
ms.author: mimig
ms.openlocfilehash: 455479c9eb77093dd5611263fe5bdcf699b9d026
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="get-started-with-azure-table-storage-using-python"></a>Aan de slag met Azure Table storage met Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table Storage is een service die gestructureerde NoSQL-gegevens opslaat in de cloud en zo een sleutel/kenmerkarchiefontwerp zonder schema biedt. Omdat Table Storage schemaloos is, kunt u uw gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. Toegang tot Table Storage-gegevens is snel en kostenefficiënt voor veel soorten toepassingen en doorgaans goedkoper dan traditionele SQL voor vergelijkbare gegevensvolumes.

U kunt Table Storage gebruiken voor de opslag van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens of andere soorten metagegevens die uw service nodig heeft. In elke tabel kunt u een willekeurig aantal entiteiten opslaan. Een opslagaccount kan een onbeperkt aantal tabellen bevatten, tot de maximale capaciteit van het opslagaccount.

### <a name="about-this-tutorial"></a>Over deze zelfstudie
Deze zelfstudie leert u hoe u de [Azure DB tabel Cosmos-SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/) in algemene scenario's voor Azure Table-opslag. De naam van de SDK geeft is voor gebruik met Azure Cosmos DB, maar dit proces werkt met beide Cosmos Azure DB en opslag in Azure-tabellen, elke service heeft zojuist een unieke eindpunt. Deze scenario's worden verkend met behulp van Python voorbeelden van hoe:
* Maken en verwijderen van tabellen
* INSERT- en entiteiten
* Entiteiten wijzigen

Tijdens het werken met de scenario's in deze zelfstudie kunt u verwijzen naar de [Azure Cosmos DB SDK voor Python-API-referentiemateriaal](https://azure.github.io/azure-cosmosdb-python/).

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze zelfstudie te voltooien:

- [Python](https://www.python.org/downloads/) 2.7, 3.3, 3.4, 3.5 of 3.6
- [Azure Cosmos DB tabel SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/). Deze SDK maakt verbinding met Azure Table storage en de Azure-API voor tabel Cosmos DB.
- [Azure Storage-account](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account#create-a-storage-account) of [Azure DB die Cosmos-account](https://azure.microsoft.com/en-us/try/cosmosdb/)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Een Azure-service-account maken

U kunt werken met tabellen met Azure Table storage of Azure Cosmos DB. U kunt meer informatie over de verschillen tussen de services door te lezen [tabel de offerings](table-introduction.md#table-offerings). U moet maken van een account voor de service die u gaat gebruiken. 

### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken
De eenvoudigste manier om uw eerste Azure-opslagaccount te maken, is via [Azure Portal](https://portal.azure.com). Zie [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor meer informatie.

U kunt ook een Azure storage-account maken met behulp van [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) of [Azure CLI](../storage/common/storage-azure-cli.md).

Als u op dit moment liever nog geen opslagaccount maakt, kunt u ook de Azure-opslagemulator gebruiken om de code in een lokale omgeving uit te voeren en te testen. Zie [Use the Azure Storage Emulator for Development and Testing](../storage/common/storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie.

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Een tabel-API van Azure Cosmos DB-account maken

Zie voor instructies over het maken van een account voor Azure Cosmos DB tabel API [maken van een tabel-API-account](create-table-dotnet.md#create-a-database-account).

## <a name="install-the-azure-cosmos-db-table-sdk-for-python"></a>De tabel in Azure Cosmos DB SDK voor Python installeren

Nadat u een opslagaccount hebt gemaakt, wordt de volgende stap is het installeren van de [Microsoft Azure Cosmos DB tabel SDK voor Python](https://pypi.python.org/pypi/azure-cosmosdb-table/). Raadpleeg voor meer informatie over het installeren van de SDK de [README.rst](https://github.com/Azure/azure-cosmosdb-python/blob/master/azure-cosmosdb-table/README.rst) -bestand van de Cosmos DB tabel SDK voor Python-opslagplaats op GitHub.

## <a name="import-the-tableservice-and-entity-classes"></a>Importeer de klassen TableService en entiteit

Om te werken met entiteiten in de service Azure Table in Python, die u gebruikt de [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) en [entiteit] [ py_Entity] klassen. Voeg deze code boven uw Python-bestand voor het importeren van beide:

```python
from azure.cosmosdb.table.tableservice import TableService
from azure.cosmosdb.table.models import Entity
```

## <a name="connect-to-azure-table-service"></a>Verbinding maken met Azure Table-service

Voor verbinding met Azure Storage, Table-service, maakt u een [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) object en in de naam en sleutel van uw Opslagaccount. Vervang `myaccount` en `mykey` met uw accountnaam en de sleutel.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')
```

## <a name="connect-to-azure-cosmos-db"></a>Verbinding maken met Azure Cosmos DB

De primaire verbindingsreeks kopiëren vanuit de Azure-portal voor verbinding met Azure Cosmos DB, en maak een [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) object met de gekopieerde verbindingsreeks:

```python
table_service = TableService(connection_string='DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;TableEndpoint=myendpoint;)
```

## <a name="create-a-table"></a>Een tabel maken

Roep [create_table] [ py_create_table] om de tabel te maken.

```python
table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel

Als u wilt een entiteit toevoegen, maakt u eerst een object die uw entiteit vertegenwoordigt, en vervolgens geeft het object de [TableService.insert_entity methode][py_TableService]. Het entiteitsobject mag een woordenboek of een object van type [entiteit][py_Entity], en de namen en waarden van uw entiteit definieert. Elke entiteit moet bevatten de vereiste [PartitionKey en RowKey](#partitionkey-and-rowkey) eigenschappen, naast de andere eigenschappen die u voor de entiteit definieert.

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
from azure.cosmosdb.table.tablebatch import TableBatch
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

Een entiteit verwijderen door de **PartitionKey** en **RowKey** naar de [delete_entity] [ py_delete_entity] methode.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Een tabel verwijderen

Als u een tabel of een van de entiteiten in het niet meer nodig hebt, belt u het [delete_table] [ py_delete_table] methode voor de tabel permanent verwijderen uit Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over - ontwikkelen met de tabel-API](https://docs.microsoft.com/en-us/azure/cosmos-db/faq#develop-with-the-table-api)
* [Azure Cosmos-DB-SDK voor Python-API-referentiemateriaal](https://azure.github.io/azure-cosmosdb-python/)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): een gratis, platformoverschrijdende-toepassing voor visueel werken met Azure Storage-gegevens op Windows-, Mac OS- en Linux.
* [Werken met Python in Visual Studio (Windows)](https://docs.microsoft.com/en-us/visualstudio/python/overview-of-python-tools-for-visual-studio)

[py_commit_batch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_create_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_Entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.models.html
[py_get_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_or_replace_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableService]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableBatch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tablebatch.html
[py_merge_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_update_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
