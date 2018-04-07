---
title: Het gebruik van Azure Storage, Table-service of Azure DB tabel Cosmos-API van PHP | Microsoft Docs
description: Informatie over het gebruik van de service-API van de tabel met PHP maken en verwijderen van een tabel, invoegen, verwijderen en opvragen van de tabel.
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 02/22/2018
ms.author: mimig
ms.openlocfilehash: 69fa1f8f9717d1ea4ca5081a45317cff8d752809
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-use-azure-storage-table-service-or-cosmos-db-table-api-from-php"></a>Het gebruik van Azure Storage, Table-service of Cosmos DB tabel API met PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de Azure Storage, Table-service en de API van Azure Cosmos DB tabel uitvoeren. De voorbeelden zijn geschreven in PHP en gebruik de [clientbibliotheek van Azure Storage tabel PHP][download]. De scenario's worden behandeld: **maken en het verwijderen van een tabel**, en **invoegen, verwijderen en het uitvoeren van query's entiteiten in een tabel**. Zie voor meer informatie over de Azure Table-service de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Een Azure-service-account maken

U kunt werken met tabellen met Azure Table storage of Azure Cosmos DB tabel-API. U kunt meer informatie over de verschillen tussen de services door te lezen [tabel de offerings](table-introduction.md#table-offerings). U moet maken van een account voor de service die u gaat gebruiken. 

### <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

De eenvoudigste manier om uw eerste opslagaccount te maken is met behulp van de [Azure-portal](https://portal.azure.com). Zie [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor meer informatie.

U kunt ook een opslagaccount maken met behulp van [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) of [Azure CLI](../storage/common/storage-azure-cli.md).

Als u liever niet te maken van een opslagaccount op dit moment, kunt u ook de Azure-opslagemulator gebruiken voor het uitvoeren en testen van uw code in een lokale omgeving. Zie [De Azure-opslagemulator gebruiken voor ontwikkeling en testen](../storage/common/storage-use-emulator.md) voor meer informatie.

### <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Zie voor instructies over het maken van een account voor Azure Cosmos DB [maken van een tabel-API-account](create-table-dotnet.md#create-a-database-account).

## <a name="create-a-php-application"></a>Een PHP-toepassing maken

De enige vereiste voor het maken van een PHP-toepassing voor toegang tot de tabelservice Storage of Azure Cosmos DB tabel API is verwijzen naar klassen in de SDK van azure-opslag-tabel voor PHP vanuit uw code. Alle ontwikkelingsprogramma's kunt u uw toepassing, met inbegrip van Kladblok maken.

In deze handleiding gebruikt u tabelservice Storage of Azure DB die Cosmos-functies die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een Azure-Webrol, werkrol of website.

## <a name="get-the-client-library"></a>Ophalen van de clientbibliotheek

1. Maak een bestand met de naam composer.json in de hoofdmap van uw project en voeg de volgende code toe:
```json
{
  "require": {
    "microsoft/azure-storage-table": "*"
  }
}
```
2. Download [composer.phar](http://getcomposer.org/composer.phar) in de hoofdmap. 
3. Open een opdrachtprompt en voer de volgende opdracht in de hoofdmap van uw project:
```
php composer.phar install
```
U kunt ook gaat u naar de [clientbibliotheek van Azure Storage tabel PHP](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table) op GitHub voor het klonen van de broncode.


## <a name="add-required-references"></a>Vereiste verwijzingen toevoegen
Voor het gebruik van de tabelservice Storage of Azure Cosmos DB API's, moet u het volgende doen:

* Verwijst naar de autoloader-bestand met de [require_once] [ require_once] -instructie en
* Verwijst naar alle klassen die u gebruikt.

Het volgende voorbeeld laat zien hoe de autoloader-bestand en de verwijzing naar de **TableRestProxy** klasse.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Table\TableRestProxy;
```

In de onderstaande voorbeelden de `require_once` instructie wordt altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="add-a-storage-table-service-connection"></a>De verbinding met een Opslagaccount toevoegen
U moet een geldige verbindingsreeks hebben om te instantiëren van een client Table Storage-service. De indeling voor de verbindingsreeks voor opslag tabel-service is:

```php
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"
```

## <a name="add-an-azure-cosmos-db-connection"></a>Een Azure Cosmos DB-verbinding toevoegen
U moet een geldige verbindingsreeks hebben voor het concretiseren van een Azure Cosmos DB Table-client. De indeling voor de Azure DB die Cosmos-verbindingsreeks is:

```php
$connectionString = "DefaultEndpointsProtocol=[https];AccountName=[myaccount];AccountKey=[myaccountkey];TableEndpoint=[https://myendpoint/]";
```

## <a name="add-a-storage-emulator-connection"></a>Een verbinding van de emulator opslag toevoegen
Toegang tot de emulator opslag:

```php
UseDevelopmentStorage = true
```

Voor het maken van een client van de service Azure Table of Azure DB die Cosmos-client, die u wilt gebruiken, de **TableRestProxy** klasse. U kunt:

* de verbindingsreeks rechtstreeks aan deze doorgeven of
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
  * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen.
  * U kunt nieuwe bronnen toevoegen door het uitbreiden van de `ConnectionStringSource` klasse.

Voor de voorbeelden die hier wordt beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;

$tableClient = TableRestProxy::createTableService($connectionString);
```

## <a name="create-a-table"></a>Een tabel maken
Een **TableRestProxy** object kunt u bij het maken van een tabel met de **createTable** methode. Bij het maken van een tabel, kunt u de time-out voor de tabel-service instellen. (Zie voor meer informatie over de time-out voor de tabel-service, [time-outs instellen voor tabel servicebewerkingen][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create Table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Create table.
    $tableClient->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
}
```

Zie voor meer informatie over de beperkingen op tabelnamen [inzicht in de tabel Service Data Model][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen aan een tabel, maakt u een nieuwe **entiteit** object en doorgegeven aan **TableRestProxy -> insertEntity**. Houd er rekening mee dat wanneer u een entiteit maakt, moet u een `PartitionKey` en `RowKey`. Dit zijn de unieke id's voor een entiteit en waarden die veel sneller dan andere entiteitseigenschappen kunnen worden opgevraagd. Gebruikt het systeem `PartitionKey` automatisch over veel opslagknooppunten verdelen van de tabel entiteiten. Entiteiten met dezelfde `PartitionKey` op hetzelfde knooppunt worden opgeslagen. (Het uitvoeren van bewerkingen op meerdere entiteiten die zijn opgeslagen op hetzelfde knooppunt beter dan op entiteiten die zijn opgeslagen op verschillende knooppunten.) De `RowKey` is de unieke ID van een entiteit binnen een partitie.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableClient->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Zie voor meer informatie over de eigenschappen van de tabel en typen [inzicht in de tabel Service Data Model][table-data-model].

De **TableRestProxy** klasse biedt twee alternatieve methoden voor het invoegen van entiteiten: **insertOrMergeEntity** en **insertOrReplaceEntity**. Voor het gebruik van deze methoden, maak een nieuwe **entiteit** en doorgegeven als parameter aan een van de methoden. Elke methode worden de entiteit ingevoegd als deze niet bestaat. Als de entiteit al bestaat, **insertOrMergeEntity** eigenschapswaarden bijgewerkt als de eigenschappen al bestaat en worden nieuwe eigenschappen toegevoegd als ze niet bestaan, terwijl **insertOrReplaceEntity** Hiermee vervangt u een bestaande entiteit. Het volgende voorbeeld ziet u hoe u **insertOrMergeEntity**. Als de entiteit met `PartitionKey` 'tasksSeattle' en `RowKey` '1' niet al bestaat, wordt ingevoegd. Echter, als deze eerder is ingevoegd (zoals weergegeven in het bovenstaande voorbeeld), de `DueDate` eigenschap wordt bijgewerkt, en de `Status` eigenschap is toegevoegd. De `Description` en `Location` eigenschappen ook worden bijgewerkt, maar met waarden die effectief blijven ongewijzigd. Als deze laatste twee eigenschappen zijn niet toegevoegd, zoals in het voorbeeld, maar er waren aanwezig op de doelentiteit, blijft hun bestaande waarden ongewijzigd.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableClient->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Eén entiteit ophalen
De **TableRestProxy -> getEntity** methode kunt u één entiteit ophalen door een query uitvoert voor de `PartitionKey` en `RowKey`. In het volgende voorbeeld wordt de partitiesleutel `tasksSeattle` en rijsleutel `1` worden doorgegeven aan de **getEntity** methode.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    $result = $tableClient->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Entiteit-query's worden gemaakt met behulp van filters (Zie voor meer informatie [opvragen van tabellen en entiteiten][filters]). U haalt alle entiteiten in de partitie met het filter ' PartitionKey eq *partitienaam*'. Het volgende voorbeeld laat zien hoe voor het ophalen van alle entiteiten in de `tasksSeattle` partitie door een filter toe op de **queryEntities** methode.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Een subset van entiteiten in een partitie ophalen
Hetzelfde patroon gebruikt in het vorige voorbeeld kan worden gebruikt voor het ophalen van een subset van entiteiten in een partitie. De subset met u ophalen entiteiten worden bepaald door het filter dat u gebruikt (Zie voor meer informatie [opvragen van tabellen en entiteiten][filters]). Het volgende voorbeeld laat zien hoe een filter te gebruiken voor het ophalen van alle entiteiten met een specifieke `Location` en een `DueDate` kleiner is dan een opgegeven datum.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Ophalen van een subset van entiteitseigenschappen
Een query kunt een subset van entiteitseigenschappen ophalen. Deze methode, aangeroepen *projectie*, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. Als u een eigenschap om op te halen, geeft de naam van de eigenschap in op de **Query -> addSelectField** methode. U kunt deze methode niet meerdere keren aanroepen meer eigenschappen toevoegen. Na het uitvoeren van **TableRestProxy -> queryEntities**, de geretourneerde entiteiten hebben alleen de geselecteerde eigenschappen. (Als u een subset van de tabelentiteiten retourneren wilt, gebruikt u een filter zoals weergegeven in de bovenstaande query's.)

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableClient->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Bijwerken van een entiteit
U kunt een bestaande entiteit bijwerken met behulp van de **entiteit -> setProperty** en **entiteit -> addProperty** methoden op de entiteit en vervolgens aanroepen **TableRestProxy updateEntity->**. Het volgende voorbeeld worden opgehaald van een entiteit, één eigenschap wijzigt, verwijdert u een andere eigenschap en wordt een nieuwe eigenschap toegevoegd. Houd er rekening mee dat u een eigenschap verwijderen kunt door de waarde instellen op **null**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$result = $tableClient->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();
$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.
$entity->setPropertyValue("Location", null); //Removed Location.
$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableClient->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen
Als u wilt een entiteit wilt verwijderen, geeft u de tabelnaam en de entiteit `PartitionKey` en `RowKey` naar de **TableRestProxy -> deleteEntity** methode.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete entity.
    $tableClient->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Voor controles gelijktijdigheid van taken, kunt u instellen de Etag voor een entiteit moet worden verwijderd met behulp van de **DeleteEntityOptions -> setEtag** methode en geven de **DeleteEntityOptions** object naar de ** deleteEntity** als een vierde parameter.

## <a name="batch-table-operations"></a>Batchbewerkingen tabel
De **TableRestProxy -> batch** methode kunt u meerdere bewerkingen uitvoeren in een enkele aanvraag. Het patroon hier omvat het toevoegen van bewerkingen voor **BatchRequest** object en vervolgens doorgegeven de **BatchRequest** object toe aan de **TableRestProxy -> batch** methode. Toevoegen van een bewerking voor een **BatchRequest** object, u een van de volgende methoden kunt aanroepen, kunt u meerdere keren:

* **addInsertEntity** (toevoegen van een bewerking insertEntity)
* **addUpdateEntity** (toevoegen van een bewerking updateEntity)
* **addMergeEntity** (toevoegen van een bewerking mergeEntity)
* **addInsertOrReplaceEntity** (toevoegen van een bewerking insertOrReplaceEntity)
* **addInsertOrMergeEntity** (toevoegen van een bewerking insertOrMergeEntity)
* **addDeleteEntity** (toevoegen van een bewerking deleteEntity)

Het volgende voorbeeld ziet u het uitvoeren van **insertEntity** en **deleteEntity** bewerkingen in één aanvraag. 

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

// Configure a connection string for Storage Table service.
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableClient->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Zie voor meer informatie over batchverwerking tabelbewerkingen [uitvoeren entiteit groepstransacties][entity-group-transactions].

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte voor het verwijderen van een tabel, geeft de naam van de tabel aan de **TableRestProxy -> deleteTable** methode.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete table.
    $tableClient->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van de Azure Table-service en Azure Cosmos DB hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.

* [PHP-ontwikkelaarscentrum](/develop/php/).

[download]: https://packagist.org/packages/microsoft/azure-storage-table
[require_once]: http://php.net/require_once
[table-service-timeouts]: https://docs.microsoft.com/rest/api/storageservices/setting-timeouts-for-table-service-operations

[table-data-model]: https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model
[filters]: https://docs.microsoft.com/rest/api/storageservices/Querying-Tables-and-Entities
[entity-group-transactions]: https://docs.microsoft.com/rest/api/storageservices/Performing-Entity-Group-Transactions
