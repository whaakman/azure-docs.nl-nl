---
title: Azure Table Storage en Azure Cosmos DB Table-API gebruiken met C++
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage of de Azure Cosmos DB Table-API.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: cpp
ms.topic: sample
ms.date: 04/05/2018
author: wmengmsft
ms.author: wmeng
ms.openlocfilehash: 40b84a56f93ad670a26eb876a18820e0d4037f63
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54033971"
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-c"></a>Azure-tabelopslag en Azure Cosmos DB Table-API gebruiken met C++
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

## <a name="overview"></a>Overzicht
In deze handleiding wordt beschreven hoe u veelvoorkomende scenario's uitvoert met de Azure-tabelopslagservice of de Azure Cosmos DB Table-API. De voorbeelden zijn geschreven in C++ en maken gebruik van de [Azure Storage-clientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's die worden besproken, zijn onder andere het **maken en verwijderen van een tabel** en **werken met tabelentiteiten**.

> [!NOTE]
> Deze handleiding is gericht op de Azure-opslagclientbibliotheek voor C++ versie 1.0.0 en hoger. De aanbevolen versie is opslagclientbibliotheek 2.2.0. Deze is beschikbaar via [NuGet](https://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp/).
> 

## <a name="create-an-azure-service-account"></a>Een Azure-serviceaccount maken
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Een Azure Cosmos DB Table-API-account maken
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gaat u opslagfuncties gebruiken die kunnen worden uitgevoerd binnen een C++-toepassing. Hiervoor moet u de Azure-opslagclientbibliotheek voor C++ installeren en een Azure-opslagaccount in uw Azure-abonnement maken.  

Voor het installeren van de Azure-opslagclientbibliotheek voor C++ kunt u de volgende methoden gebruiken:

* **Linux:** Volg de instructies op de pagina met het [Leesmij-bestand voor de Azure Storage-clientbibliotheek voor C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).  
* **Windows:** Klik in Visual Studio op **Tools > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager-console](/nuget/tools/package-manager-console) en druk op Enter.  
  
     Install-Package wastorage

## <a name="configure-access-to-the-table-client-library"></a>Toegang tot de tabelclientbibliotheek configureren
Voeg bovenaan het C++-bestand de volgende insluitinstructies toe waar u Azure-opslag-API's wilt gebruiken voor toegang tot tabellen:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

Een Azure-opslagclient of Cosmos DB-client gebruikt een opslagverbindingstekenreeks voor het opslaan van eindpunten en referenties voor toegang tot gegevensbeheerservices. Wanneer u een clienttoepassing uitvoert, moet u de opslagverbindingstekenreeks of Azure Cosmos DB-verbindingstekenreeks opgeven in de juiste indeling.

## <a name="set-up-an-azure-storage-connection-string"></a>Een Azure-opslagverbindingstekenreeks instellen
 Gebruik de naam van uw opslagaccount en de toegangssleutel voor het opslagaccount dat in [Azure Portal](https://portal.azure.com) wordt genoemd als waarden voor *AccountName* en *AccountKey*. Zie [Over Azure Storage-accounts](../storage/common/storage-create-storage-account.md) voor meer informatie over opslagaccounts en toegangssleutels. In dit voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de Azure Storage-verbindingstekenreeks:  

```cpp
// Define the Storage connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="set-up-an-azure-cosmos-db-connection-string"></a>Een Azure Cosmos DB-verbindingstekenreeks instellen
Gebruik de naam van uw Azure Cosmos DB-account, uw primaire sleutel en eindpunt die worden genoemd in [Azure Portal](https://portal.azure.com) voor de waarden *AccountName*, *PrimaryKey* en *Endpoint*. In dit voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de Azure Cosmos DB-verbindingstekenreeks:

```cpp
// Define the Azure Cosmos DB connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_cosmos_db_account;AccountKey=your_cosmos_db_account_key;TableEndpoint=your_cosmos_db_endpoint"));
```


Als u uw toepassing in uw lokale Windows-computer wilt testen, kunt u de Azure-[opslagemulator](../storage/common/storage-use-emulator.md) gebruiken die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma dat de Azure-blob-, wachtrij-en tabelservices simuleert die beschikbaar zijn op uw lokale ontwikkelingsmachine. In het volgende voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de verbindingstekenreeks naar uw lokale opslagemulator:  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Voor het starten van de Azure-opslagemulator klikt u op de knop **Start** of drukt u op de Windows-toets. Begin **Azure Storage Emulator** te typen en selecteer vervolgens **Microsoft Azure Storage Emulator** uit de lijst met toepassingen.  

In de volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de opslagverbindingstekenreeks op te halen.  

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de klasse **cloud_storage_account** gebruiken om uw opslagaccountgegevens te vertegenwoordigen. Voor het ophalen van uw opslagaccountgegevens uit de opslagverbindingstekenreeks kunt u de methode **parse** gebruiken.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Haal vervolgens een referentie naar een klasse **cloud_table_client** op, omdat u hiermee referentieobjecten kunt ophalen voor tabellen en entiteiten die binnen de tabelopslagservice zijn opgeslagen. Met de volgende code maakt u een **cloud_table_client**-object met behulp van het opslagaccountobject dat we hierboven hebben opgehaald:  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Een tabel maken
Met een **cloud_table_client**-object kunt u referentieobjecten ophalen voor tabellen en entiteiten. Met de volgende code maakt u een **cloud_table_client**-object en gebruikt u deze om een nieuwe tabel te maken.

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
Als u een entiteit wilt toevoegen aan een tabel, maakt u een nieuw **table_entity**-object en geeft u dit door aan **table_operation::insert_entity**. Met de volgende code gebruikt u de voornaam van de klant als de rijsleutel en de achternaam als de partitiesleutel. De partitie- en rijsleutel van een entiteit vormen samen de unieke id van de entiteit in de tabel. Entiteiten met dezelfde partitiesleutel kunnen sneller worden opgevraagd dan entiteiten met verschillende partitiesleutels, maar het gebruik van verschillende partitiesleutels maakt een grotere schaalbaarheid van parallelle bewerkingen mogelijk. Raadpleeg de [Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../storage/common/storage-performance-checklist.md) voor meer informatie.

Met de volgende code maakt u een nieuwe instantie van de **table_entity**, waarbij een aantal klantgegevens worden opgeslagen. Vervolgens roept u met de code  **table_operation::insert_entity** aan om een **table_operation**-object te maken om een entiteit in te voegen in een tabel. Hier wordt de nieuwe tabelentiteit aan gekoppeld. Als laatste roept u met de code de methode execute aan op het **cloud_table**-object. Met de nieuwe **table_operation** stuurt u een aanvraag naar de tabelservice om de nieuwe klantentiteit in te voegen in de tabel 'mensen'.  

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
U kunt in één schrijfbewerking een batch entiteiten invoegen in de tabelservice. Met de volgende code maakt u een **table_batch_operation**-object en voegt u hier vervolgens drie invoegbewerkingen aan toe. Elke invoegbewerking wordt toegevoegd door het maken van een nieuw entiteitobject, de waarden ervan in te stellen en vervolgens de methode insert aan te roepen op het **table_batch_operation**-object om de entiteit aan een nieuwe invoegbewerking te koppelen. Vervolgens wordt **cloud_table.execute** aangeroepen om de bewerking uit te voeren.  

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

Een aantal zaken die u moet weten over batchbewerkingen:  

* U kunt maximaal 100 bewerkingen voor invoegen, verwijderen, samenvoegen, vervangen, invoegen of samenvoegen, en invoegen of vervangen in elke willekeurige combinatie in één batch uitvoeren.  
* Een batchbewerking kan een ophaalbewerking hebben als dit de enige bewerking in de batch is.  
* Alle entiteiten in een batchbewerking moeten dezelfde partitiesleutel hebben.  
* Een batchbewerking is beperkt tot een gegevensnettolading van 4 MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Alle entiteiten in een partitie ophalen
Gebruik een **table_query**-object om een tabel met alle entiteiten in een partitie op te vragen. Het volgende codevoorbeeld geeft een filter voor entiteiten waarbij 'Smith' de partitiesleutel is. In dit voorbeeld worden de velden van elke entiteit in de queryresultaten naar de console afgedrukt.  

> [!NOTE]
> Deze methoden worden momenteel niet ondersteund voor C++ in Azure Cosmos DB.

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

Met de query in dit voorbeeld krijgt u een overzicht van alle entiteiten die voldoen aan de filtercriteria. Als u grote tabellen hebt en de tabelentiteiten vaak moet downloaden, raden we u aan uw gegevens in plaats daarvan op te slaan in Azure-opslagblobs.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Een bereik van entiteiten in een partitie ophalen
Als u geen query wilt uitvoeren op alle entiteiten in een partitie, kunt u een bereik opgeven door het partitiesleutelfilter te combineren met een rijsleutelfilter. Het volgende codevoorbeeld maakt gebruik van twee filters om alle entiteiten met de partitie 'Smith' op te halen waarbij de rijsleutel (voornaam) begint met een letter die in het alfabet vóór de 'E' komt. Vervolgens worden de resultaten van de query afgedrukt.  

> [!NOTE]
> Deze methoden worden momenteel niet ondersteund voor C++ in Azure Cosmos DB.

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
U kunt een query schrijven om één specifieke entiteit op te halen. Met de volgende code gebruikt u **table_operation::retrieve_entity** om de klant 'Jeff Smith' op te geven. Deze methode retourneert één entiteit in plaats van een verzameling. De geretourneerde waarde is in **table_result**. Het opgeven van zowel partitie- als rijsleutels in een query is de snelste manier om één entiteit op te halen uit de Tabelservice.  

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
Als u een entiteit wilt vervangen, haalt u deze op uit de Tabelservice, wijzigt u het entiteitsobject en slaat u de wijzigingen weer op in de Tabelservice. Met de volgende code wijzigt u het telefoonnummer en het e-mailadres van een bestaande klant. In plaats van  **table_operation::insert_entity** aan te roepen, gebruikt u met deze code **table_operation::replace_entity**. Met deze bewerking wordt de entiteit op de server volledig vervangen, tenzij de entiteit op de server sinds het ophalen is gewijzigd. In dat geval mislukt de bewerking. Hiermee wordt voorkomen dat de toepassing per ongeluk een wijziging overschrijft die door een ander onderdeel van uw toepassing is aangebracht tussen het moment van ophalen en het moment van bijwerken. U kunt deze fout het best als volgt afhandelen: de entiteit opnieuw ophalen, de gewenste wijzigingen (indien nog geldig) aanbrengen en vervolgens een andere **table_operation::replace_entity**-bewerking uitvoeren. In de volgende sectie wordt beschreven hoe u dit gedrag kunt wijzigen.  

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
**table_operation::replace_entity**-bewerkingen mislukken als de entiteit is gewijzigd nadat deze is opgehaald van de server. Voor een succesvolle **table_operation::replace_entity**-bewerking moet u de entiteit bovendien eerst ophalen van de server. Soms weet u echter niet of de entiteit op de server aanwezig is en zijn de huidige waarden die erin zijn opgeslagen, niet relevant. Alle waarden worden door de update overschreven. Om dit tot stand te brengen, gebruikt u een **table_operation::insert_or_replace_entity**-bewerking. Met deze bewerking wordt de entiteit ingevoegd als deze niet bestaat of vervangen als deze wel bestaat, ongeacht wanneer de laatste update is uitgevoerd. In het volgende codevoorbeeld wordt de klantentiteit voor 'Jeff Smith' nog steeds opgehaald, maar vervolgens op de server opgeslagen via **table_operation::insert_or_replace_entity**. Eventuele wijzigingen die in de entiteit zijn aangebracht tussen het moment van ophalen en het moment van bijwerken, worden overschreven.  

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
Met een query naar een tabel kunnen slechts enkele eigenschappen van een entiteit worden opgehaald. Met de query in de volgende code gebruikt u de methode **table_query::set_select_columns** om alleen de e-mailadressen van entiteiten in de tabel te retourneren.  

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
> Het uitvoeren van een query voor een aantal eigenschappen van een entiteit is een efficiëntere bewerking dan het ophalen van alle eigenschappen.
> 
> 

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Zodra de entiteit is opgehaald, roept u **table_operation::delete_entity** aan met de entiteit die u wilt verwijderen. Vervolgens roept u de methode **cloud_table.execute** aan. Met de volgende code haalt u een entiteit met een partitiesleutel ‘Smith’ en de rijsleutel ‘Jeff’ op en verwijdert u deze.  

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

// Delete the table if it exists
if (table.delete_table_if_exists())
    {
        std::cout << "Table deleted!";
    }
    else
    {
        std::cout << "Table didn't exist";
    }
```

## <a name="troubleshooting"></a>Problemen oplossen
* Versiefouten in Visual Studio 2017 Community Edition

  Als er opbouwfouten voor uw project ontstaan vanwege de insluitbestanden storage_account.h en table.h, moet u de **/permissive-**-compileerschakelaar verwijderen. 
  - Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Properties**.
  - Vouw in het dialoogvenster **Eigenschappenpagina’s** de optie **Configuratie-eigenschappen** en vervolgens **C/C++** uit en selecteer **Taal**.
  - Stel **Overeenstemmingsmodus** in op **Nee**.
   
## <a name="next-steps"></a>Volgende stappen
Volg deze links koppelingen meer informatie over Azure Storage en de tabel-API in Azure Cosmos DB: 

* [Introductie tot de tabel-API](table-introduction.md)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Azure Storage-resources in C++ weergeven](../storage/common/storage-c-plus-plus-enumeration.md)
* [Naslaginformatie over de Storage-clientbibliotheek voor C++](https://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
