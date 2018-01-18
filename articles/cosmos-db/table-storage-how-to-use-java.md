---
title: Hoe Azure Table storage gebruiken met Java | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 6862475e05f49c7da823bcfb70f30ee484131d12
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-from-java"></a>Hoe Azure Table storage gebruiken met Java
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met behulp van de service Azure Table storage uitvoeren. De voorbeelden zijn geschreven in Java en gebruik de [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]. De scenario's worden behandeld: **maken**, **aanbieding**, en **verwijderen** tabellen, evenals **invoegen**, **opvragen**, **wijzigen**, en **verwijderen** entiteiten in een tabel. Zie voor meer informatie over tabellen de [Vervolgstappen](#Next-Steps) sectie.

> [!NOTE]
> Een SDK is beschikbaar voor ontwikkelaars die werken met Azure Storage op Android-apparaten. Zie voor meer informatie de [Azure-opslag-SDK voor Android][Azure Storage SDK for Android].
>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in de code die wordt uitgevoerd binnen een Webrol of worker-rol in Azure.

Om dit te doen, moet u Java Development Kit (JDK) installeren en een Azure storage-account maken in uw Azure-abonnement. Als u dit hebt gedaan, moet u controleren of uw ontwikkelsysteem voldoet aan de minimale vereisten en afhankelijkheden die worden vermeld in de [Azure-opslag-SDK voor Java] [ Azure Storage SDK for Java] opslagplaats op GitHub. Als uw systeem aan deze vereisten voldoet, kunt u de instructies voor het downloaden en installeren van de Azure Storage-bibliotheken voor Java op uw systeem vanuit die opslagplaats. Nadat u deze taken hebt voltooid, kunt u zich kunt maken van een Java-toepassing die gebruikmaakt van de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing configureren voor toegang tot tabelopslag
De volgende importinstructies toevoegen aan het begin van de Java-bestand waar u Microsoft Azure storage-API's gebruiken voor toegang tot tabellen:

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Een Azure-opslag-verbindingsreeks instellen
Een Azure-opslag-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer u in een clienttoepassing uitvoert, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw opslagaccount en de primaire toegangssleutel voor het opslagaccount vermeld in de [Azure-portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Dit voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

In een toepassing in een rol in Microsoft Azure wordt uitgevoerd, kan deze tekenreeks worden opgeslagen in het serviceconfiguratiebestand *ServiceConfiguration.cscfg*, en kunnen worden geopend met een aanroep naar de **RoleEnvironment.getConfigurationSettings** methode. Hier volgt een voorbeeld van het ophalen van de verbindingsreeks uit een **instelling** element met de naam *StorageConnectionString* in het configuratiebestand van de service:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om op te halen van de verbindingsreeks voor opslag.

## <a name="how-to-create-a-table"></a>Procedure: een tabel maken
Een **CloudTableClient** object kunt u profiteren van reference-objecten voor tabellen en entiteiten. De volgende code maakt een **CloudTableClient** object en gebruikt deze om een nieuwe **CloudTable** -object met een tabel met de naam 'mensen'. 

> [!NOTE]
> Er zijn aanvullende manieren maken **CloudStorageAccount** objecten; voor meer informatie Zie **CloudStorageAccount** in de [naslaginformatie over Azure Storage Client SDK].)
>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-tables"></a>Procedure: de tabellen
Als u een lijst met tabellen, Roep de **CloudTableClient.listTables()** methode voor het ophalen van een iterable lijst met namen van tabellen.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-to-a-table"></a>Procedure: een entiteit toevoegen aan een tabel
Entiteiten worden toegewezen aan de Java-objecten met behulp van de implementatie van een aangepaste klasse **TableEntity**. Voor het gemak de **TableServiceEntity** klasse implementeert **TableEntity** en maakt gebruik van reflectie eigenschappen toe te wijzen aan getter en setter methoden met de naam van de eigenschappen. Als u wilt een entiteit toevoegen aan een tabel, moet u eerst een klasse die de eigenschappen van uw entiteit definieert maken. De volgende code definieert een entiteitsklasse die de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel gebruikt. De partitie- en rijsleutel van een entiteit vormen samen de unieke id van de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan die met verschillende partitiesleutels.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

Tabelbewerkingen met betrekking tot entiteiten vereist een **TableOperation** object. Dit object definieert de bewerking wordt uitgevoerd op een entiteit die kan worden uitgevoerd met een **CloudTable** object. De volgende code maakt een nieuw exemplaar van de **CustomerEntity** klasse met sommige gegevens van de klant worden opgeslagen. De volgende code-aanroepen **TableOperation.insertOrReplace** maken een **TableOperation** object voor het invoegen van een entiteit in een tabel en koppelt u de nieuwe **CustomerEntity** aan. Ten slotte de code roept de **uitvoeren** methode op de **CloudTable** object, waarbij u de tabel 'gebruikers' en de nieuwe **TableOperation**, die vervolgens verzendt een aanvraag naar de storage-service voor het invoegen van de nieuwe klantentiteit in de tabel 'gebruikers' of de entiteit vervangen als deze al bestaat.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a>Procedure: een batch entiteiten invoegen
U kunt een batch entiteiten met de tabelservice invoegen in één schrijfbewerking. De volgende code maakt een **TableBatchOperation** object en vervolgens voegt u drie bewerkingen voor het invoegen. Elke bewerking insert is toegevoegd door het maken van een nieuwe entiteitsobject, de waarden in te stellen en vervolgens het aanroepen van de **invoegen** methode op de **TableBatchOperation** -object op voor de entiteit koppelen aan een nieuwe insert-bewerking. En vervolgens de code aanroepen **uitvoeren** op de **CloudTable** object, waarbij u de tabel 'gebruikers' en de **TableBatchOperation** -object, dat de batch van tabelbewerkingen met de storage-service in een afzonderlijke aanvraag verzendt.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Een aantal zaken te weten over batchbewerkingen:

* U kunt uitvoeren van maximaal 100 invoegen, verwijderen, samenvoegen, vervangen, invoegen of samenvoegen, en invoegen of vervangen bewerkingen in een willekeurige combinatie in één batch.
* Een batchbewerking kan een bewerking ophalen hebben als dit de enige bewerking in de batch.
* Alle entiteiten in een batchbewerking moeten dezelfde partitiesleutel hebben.
* Een batchbewerking is beperkt tot een nettolading met gegevens van 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Hoe: alle entiteiten in een partitie ophalen
Om te vragen een tabel voor entiteiten in een partitie, kunt u een **TableQuery**. Roep **TableQuery.from** voor het maken van een query op een bepaalde tabel die een resultaattype opgegeven retourneert. De volgende code geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. **TableQuery.generateFilterCondition** is een Help-methode om filters voor query's te maken. Roep **waar** op de verwijzing geretourneerd door de **TableQuery.from** methode voor het filter toepassen op de query. Wanneer de query wordt uitgevoerd met een aanroep naar **uitvoeren** op de **CloudTable** object, wordt een **Iterator** met de **CustomerEntity** resultaattype opgegeven. U kunt de **Iterator** geretourneerd in een voor elke lus in beslag neemt de resultaten. Deze code wordt de velden van elke entiteit in de queryresultaten naar de console afgedrukt.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Procedure: een bereik van entiteiten in een partitie ophalen
Als u geen query uitvoeren op alle entiteiten in een partitie wilt, kunt u een bereik met behulp van vergelijkingsoperators in een filter opgeven. De volgende code combineert twee filters om alle entiteiten in de partitie 'Smith', waarbij de rijsleutel (voornaam) met een letter maximaal 'E begint' in het alfabet. Vervolgens worden de resultaten van de query afgedrukt. Als u de toegevoegd aan de tabel in de batch entiteiten invoegen van deze handleiding, worden slechts twee entiteiten geretourneerd ditmaal (Ben en Denise Smith); Jeff Smith is niet opgenomen.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a>Hoe: Eén entiteit ophalen
U kunt een query schrijven om één specifieke entiteit op te halen. De volgende code aanroepen **TableOperation.retrieve** met partitie-sleutel en rij sleutel parameters om op te geven van de klant 'Jeff Smith', in plaats van het maken van een **TableQuery** en doe hetzelfde met filters. Wanneer uitgevoerd, wordt de bewerking ophalen retourneert één entiteit in plaats van een verzameling. De **getResultAsType** methode cast het resultaat van het type van de doel-toewijzing, een **CustomerEntity** object. Als u dit type is niet compatibel met het opgegeven type voor de query, wordt er een uitzondering opgetreden. Als er is geen entiteit heeft een exacte partitie en de rijsleutel overeen met een null-waarde geretourneerd. Het opgeven van zowel partitie- als rijsleutels in een query is de snelste manier om één entiteit op te halen uit de Tabelservice.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a>Procedure: een entiteit wijzigen
Voor het wijzigen van een entiteit ophalen uit de tabelservice, wijzigingen aanbrengen in het entiteitsobject en sla de wijzigingen terug naar de tabelservice met een vervangings- of merge-bewerking. De volgende code wijzigt het telefoonnummer van een bestaande klant. In plaats van aanroepen **TableOperation.insert** zoals we gedaan om in te voegen, wordt deze code roept **TableOperation.replace**. De **CloudTable.execute** methode roept de tabelservice en de entiteit wordt vervangen, tenzij een andere toepassing gewijzigd deze in de tijd sinds deze toepassing wordt opgehaald. Wanneer dit gebeurt, wordt een uitzondering gegenereerd en de entiteit moet worden opgehaald, gewijzigd en opnieuw worden opgeslagen. Dit patroon van een nieuwe poging optimistische gelijktijdigheid is gemeenschappelijk in een gedistribueerd opslagsysteem.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a>Hoe: query uitvoeren op een subset van entiteitseigenschappen
Een query naar een tabel ophalen slechts enkele eigenschappen van een entiteit. Deze methode, projectie genoemd, verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. De query in de volgende code gebruikt de **Selecteer** methode om te worden alleen de e-mailadressen van entiteiten in de tabel retourneren. De resultaten worden geprojecteerd in een verzameling van **tekenreeks** met behulp van een **EntityResolver**, doet de typeconversie op de entiteiten die zijn geretourneerd door de server. U kunt meer informatie over projectie in [Azure-tabellen: blogbericht introductie tot Upsert en Query projectie][Azure Tables: Introducing Upsert and Query Projection]. Houd er rekening mee dat projectie niet wordt ondersteund op de emulator van de lokale opslag, zodat deze code wordt alleen uitgevoerd als met een account in de tabelservice.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a>Hoe: invoegen of vervangen van een entiteit
Vaak wilt u een entiteit toevoegen aan een tabel zonder te weten te komen of deze al in de tabel voorkomt. Een bewerking voor invoegen of vervangen, kunt u enkele vragen die de entiteit wordt ingevoegd als deze bestaat niet of u de bestaande versie vervangen als dit het geval is. Voortbouwend op de eerdere voorbeelden, met de volgende code wordt ingevoegd of vervangen van de entiteit voor 'Walter Harp'. Na het maken van een nieuwe entiteit deze code roept de **TableOperation.insertOrReplace** methode. Deze code roept vervolgens **uitvoeren** op de **CloudTable** object met de tabel en het invoegen of vervangen als de parameters van een tabel is. Bijwerken van slechts een deel van een entiteit, de **TableOperation.insertOrMerge** methode in plaats daarvan kan worden gebruikt. Houd er rekening mee dat invoegen of vervangen wordt niet ondersteund in de emulator van de lokale opslag, zodat deze code wordt alleen uitgevoerd als met een account in de tabelservice. U kunt meer informatie over invoegen of vervangen en insert-of-samenvoegen in deze [Azure-tabellen: blogbericht introductie tot Upsert en Query projectie][Azure Tables: Introducing Upsert and Query Projection].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a>Procedure: een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Wanneer de entiteit is opgehaald, aanroepen **TableOperation.delete** met de entiteit wilt verwijderen. Roep vervolgens **uitvoeren** op de **CloudTable** object. Met de volgende code wordt een klantentiteit opgehaald en verwijderd.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a>Procedure: een tabel verwijderen
De volgende code wordt tot slot een tabel uit een opslagaccount verwijderd. Een tabel die is verwijderd is niet beschikbaar worden gemaakt voor een bepaalde tijd na de verwijdering, meestal minder dan 40 seconden zijn.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Volgende stappen

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]
* [naslaginformatie over Azure Storage Client SDK][naslaginformatie over Azure Storage Client SDK]
* [Azure Storage REST API][Azure Storage REST API]
* [Azure Storage-teamblog][Azure Storage Team Blog]

Voor meer informatie gaat u naar [Azure voor Java-ontwikkelaars](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[naslaginformatie over Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
