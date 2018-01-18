---
title: Azure Table storage gebruiken met C++ | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: a71098583af8722f2e191e0e665ac87ebd30f355
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-with-c"></a>Azure Table storage gebruiken met C++
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's uitvoeren met behulp van de service Azure Table storage. De voorbeelden zijn geschreven in C++ en gebruik de [Azure Storage-clientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's worden behandeld: **maken en het verwijderen van een tabel** en **werken met tabelentiteiten**.

> [!NOTE]
> Deze handleiding is bedoeld voor de Azure Storage-clientbibliotheek voor C++ versie 1.0.0 en hoger. De aanbevolen versie is Storage-clientbibliotheek 2.2.0, die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties dat kunnen worden uitgevoerd binnen een C++-toepassing. Om dit te doen, moet u voor het installeren van de Azure Storage-clientbibliotheek voor C++ en een Azure storage-account maken in uw Azure-abonnement.  

Voor het installeren van de Azure Storage-clientbibliotheek voor C++, kunt u de volgende methoden:

* **Linux:** Volg de instructies op de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.  
* **Windows:** In Visual Studio, klikt u op **Extra > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op Enter.  
  
     Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Uw toepassing configureren voor toegang tot Table storage
Toevoegen aan dat de volgende instructies op de bovenkant van het bestand C++ is waar u de Azure storage-API's gebruiken voor toegang tot tabellen bevatten:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Een Azure-opslag-verbindingsreeks instellen
Een Azure-opslag-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer een client-toepassing wordt uitgevoerd, moet u de verbindingsreeks voor opslag in de volgende indeling opgeven. De naam van uw opslagaccount en de toegangssleutel voor opslag gebruiken voor het opslagaccount wordt vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Zie voor informatie over de storage-accounts en toegangstoetsen, [over Azure storage-accounts](../storage/common/storage-create-storage-account.md). Dit voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:  

```cpp
// Define the connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Testen van uw toepassing in uw lokale Windows-computer, kunt u de Azure [opslagemulator](../storage/common/storage-use-emulator.md) die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma dat de Azure Blob, wachtrijen en tabellen services beschikbaar zijn op uw lokale ontwikkelcomputer simuleert. Het volgende voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks naar uw lokale opslagemulator kunt declareren:  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

De Azure-opslagemulator starten, klikt u op de **Start** knop of druk op de Windows-toets. Begint te typen **Azure-Opslagemulator**, en selecteer vervolgens **Microsoft Azure-Opslagemulator** uit de lijst met toepassingen.  

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om op te halen van de verbindingsreeks voor opslag.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de **cloud_storage_account** klasse vertegenwoordigt de gegevens van uw storage-account. Voor het ophalen van gegevens over uw storage-account van de verbindingsreeks voor opslag, kunt u de parseermethode.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Vervolgens maakt geen verwijzing ophalen naar een **cloud_table_client** klasse, zoals het kunt u profiteren van reference-objecten voor tabellen en entiteiten die zijn opgeslagen in de tabel storage-service. De volgende code maakt een **cloud_table_client** object met behulp van het opslagobject account we hierboven opgehaald:  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Een tabel maken
Een **cloud_table_client** object kunt u profiteren van reference-objecten voor tabellen en entiteiten. De volgende code maakt een **cloud_table_client** object en gebruikt deze om een nieuwe tabel maken.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen aan een tabel, maakt u een nieuwe **table_entity** object en doorgegeven aan **table_operation::insert_entity**. De volgende code gebruikt de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel. De partitie- en rijsleutel van een entiteit vormen samen de unieke id van de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan die met verschillende partitiesleutels, maar met verschillende partitiesleutels kunt grotere schaalbaarheid van parallelle bewerking. Zie voor meer informatie [Microsoft Azure storage controlelijst voor prestaties en schaalbaarheid](../storage/common/storage-performance-checklist.md).

De volgende code maakt een nieuw exemplaar van **table_entity** met sommige gegevens van de klant worden opgeslagen. De volgende code-aanroepen **table_operation::insert_entity** maken een **table_operation** object voor het invoegen van een entiteit in een tabel en de nieuwe Tabelentiteit gekoppeld. Ten slotte de code de execute-methode aanroepen op de **cloud_table** object. En de nieuwe **table_operation** verzendt een aanvraag naar de tabel-service naar de nieuwe klantentiteit in de tabel 'gebruikers' invoegen.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create the table operation that inserts the customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute the insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen
U kunt een batch entiteiten met de service tabel invoegen in één schrijfbewerking. De volgende code maakt een **table_batch_operation** object en vervolgens voegt u drie bewerkingen voor het invoegen. Elke bewerking insert is toegevoegd door het maken van een nieuwe entiteitsobject, de waarden in te stellen en vervolgens het aanroepen van de methode invoegen op het **table_batch_operation** -object op voor de entiteit koppelen aan een nieuwe insert-bewerking. Vervolgens **cloud_table.execute** aangeroepen om de bewerking niet uitvoeren.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it to the table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it to the table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity to add to the table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities to the batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute the batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

Een aantal zaken te weten over batchbewerkingen:  

* U kunt maximaal 100 invoegen, verwijderen, samenvoegen, vervangen, invoegen of merge en invoegen of vervangen bewerkingen uitvoeren in een willekeurige combinatie in één batch.  
* Een batchbewerking kan een bewerking ophalen hebben als dit de enige bewerking in de batch.  
* Alle entiteiten in een batchbewerking moeten dezelfde partitiesleutel hebben.  
* Een batchbewerking is beperkt tot een nettolading met gegevens van 4 MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruiken om te vragen een tabel voor alle entiteiten in een partitie, een **table_query** object. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct the query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print the fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

De query in dit voorbeeld wordt de entiteiten die overeenkomen met de filtercriteria. Als er grote tabellen en moet de tabelentiteiten downloaden vaak we raden aan dat opslaat u uw gegevens in Azure storage-blobs in plaats daarvan.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van entiteiten in een partitie ophalen
Als u geen query wilt uitvoeren op alle entiteiten in een partitie, kunt u een bereik opgeven door het partitiesleutelfilter te combineren met een rijsleutelfilter. Het volgende codevoorbeeld maakt gebruik van twee filters om alle entiteiten met de partitie 'Smith' op te halen waarbij de rijsleutel (voornaam) begint met een letter die in het alfabet vóór de 'E' komt. Vervolgens worden de resultaten van de query afgedrukt.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through the results, displaying information about the entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a>Eén entiteit ophalen
U kunt een query schrijven om één specifieke entiteit op te halen. De volgende code gebruikt **table_operation::retrieve_entity** om op te geven van de klant 'Jeff Smith'. Deze methode retourneert één entiteit in plaats van een verzameling en de geretourneerde waarde is in **table_result**. Het opgeven van zowel partitie- als rijsleutels in een query is de snelste manier om één entiteit op te halen uit de Tabelservice.  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output the entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a>Een entiteit vervangen
Ter vervanging van een entiteit ophalen uit de tabelservice, wijzigt het entiteitsobject en sla de wijzigingen terug naar de tabel-service. De volgende code wijzigt het telefoonnummer en e-mailadres van een bestaande klant. In plaats van aanroepen **table_operation::insert_entity**, deze code gebruikt **table_operation::replace_entity**. Met deze bewerking wordt de entiteit op de server volledig vervangen, tenzij de entiteit op de server sinds het ophalen is gewijzigd. In dat geval mislukt de bewerking. Hiermee wordt voorkomen dat de toepassing per ongeluk een wijziging overschrijft die door een ander onderdeel van uw toepassing is aangebracht tussen het moment van ophalen en het moment van bijwerken. De juiste verwerking van deze fout wordt de entiteit opnieuw ophalen, breng uw wijzigingen (indien nog geldig) en voert u een andere **table_operation::replace_entity** bewerking. In de volgende sectie wordt beschreven hoe u dit gedrag kunt wijzigen.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation to replace the entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit the operation to the Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a>Een entiteit invoegen of vervangen
**table_operation::replace_entity** bewerkingen mislukken als de entiteit is gewijzigd nadat deze is opgehaald van de server. Bovendien moet u ophalen de entiteit van de server eerst om **table_operation::replace_entity** lukken. Soms echter, u niet weet of de entiteit op de server bestaat en de huidige waarden die erin niet relevant zijn: de update worden deze allemaal overschreven. Hiervoor gebruikt u een **table_operation::insert_or_replace_entity** bewerking. Met deze bewerking wordt de entiteit ingevoegd als deze niet bestaat of vervangen als deze wel bestaat, ongeacht wanneer de laatste update is uitgevoerd. In het volgende codevoorbeeld wordt de klantentiteit voor Jeff Smith nog steeds opgehaald, maar deze wordt vervolgens terug naar de server via opgeslagen **table_operation::insert_or_replace_entity**. Wijzigingen in de entiteit tussen het ophalen en de update-bewerking worden overschreven.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation to insert-or-replace the entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit the operation to the Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen
Een query naar een tabel ophalen slechts enkele eigenschappen van een entiteit. De query in de volgende code gebruikt de **table_query::set_select_columns** methode om te worden alleen de e-mailadressen van entiteiten in de tabel retourneren.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define the query, and select only the Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display the results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> Opvragen van enkele eigenschappen van een entiteit is een efficiëntere bewerking dan bij het ophalen van alle eigenschappen.
> 
> 

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Wanneer de entiteit is opgehaald, aanroepen **table_operation::delete_entity** met de entiteit wilt verwijderen. Roep vervolgens de **cloud_table.execute** methode. De volgende code haalt en verwijdert een entiteit met een partitiesleutel van 'Smith' en een rijsleutel van 'Jeff'.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte wordt met het volgende codevoorbeeld een tabel uit een opslagaccount verwijderd. Een verwijderde tabel kan gedurende een bepaalde tijd na de verwijdering niet opnieuw worden gemaakt.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van table storage hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Storage:  

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Het Blob storage gebruiken met C++](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hoe Queue storage gebruiken met C++](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Lijst van Azure Storage-resources in C++](../storage/common/storage-c-plus-plus-enumeration.md)
* [Opslagclientbibliotheek voor C++-verwijzing](http://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
