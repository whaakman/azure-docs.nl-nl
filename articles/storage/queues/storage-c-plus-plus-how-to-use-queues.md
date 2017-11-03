---
title: Het gebruik van queue storage (C++) | Microsoft Docs
description: Informatie over het gebruik van de queue-service voor opslag in Azure. Voorbeelden zijn geschreven in C++.
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: 5e81d5e0af9871099b7f921f355cf94249e4d30c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-queue-storage-from-c"></a>Hoe Queue Storage gebruiken met C++
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding leert u hoe u veelvoorkomende scenario's met behulp van de Azure Queue storage-service uitvoert. De voorbeelden zijn geschreven in C++ en gebruik de [Azure Storage-clientbibliotheek voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De scenario's worden behandeld: **invoegen**, **inspecteren**, **ophalen**, en **verwijderen** wachtrij berichten, evenals **maken en verwijderen van wachtrijen**.

> [!NOTE]
> Deze handleiding is bedoeld voor de Azure Storage-clientbibliotheek voor C++ versie 1.0.0 en hoger. De aanbevolen versie is Storage-clientbibliotheek 2.2.0, die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](http://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.

Om dit te doen, moet u voor het installeren van de Azure Storage-clientbibliotheek voor C++ en een Azure storage-account maken in uw Azure-abonnement.

Voor het installeren van de Azure Storage-clientbibliotheek voor C++, kunt u de volgende methoden:

* **Linux:** Volg de instructies in de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.
* **Windows:** In Visual Studio, klikt u op **Extra > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.

```  
Install-Package wastorage
```

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing configureren voor toegang tot opslag van de wachtrij
Toevoegen aan dat de volgende instructies toe aan de bovenkant van het bestand C++ is waar u de Azure storage-API's gebruiken voor toegang tot wachtrijen instructies bevatten:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Een Azure-opslag-verbindingsreeks instellen
Een Azure-opslag-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer u in een clienttoepassing uitvoert, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw opslagaccount en de toegangssleutel voor opslag voor de storage-account wordt vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Zie voor informatie over de storage-accounts en toegangstoetsen, [over Azure Storage-Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json). Dit voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Testen van uw toepassing in uw lokale Windows-computer, kunt u de Microsoft Azure [opslagemulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma dat de services Blob, wachtrijen en tabellen beschikbaar zijn in Azure op uw lokale ontwikkelcomputer simuleert. Het volgende voorbeeld ziet u hoe u een statisch veld voor het opslaan van de verbindingsreeks naar uw lokale opslagemulator kunt declareren:  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Voor het starten van de Azure-opslagemulator, selecteer de **Start** of drukt u op de **Windows** sleutel. Begint te typen **Azure-Opslagemulator**, en selecteer **Microsoft Azure-Opslagemulator** uit de lijst met toepassingen.

De volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om op te halen van de verbindingsreeks voor opslag.

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de **cloud_storage_account** klasse vertegenwoordigt de informatie van uw Opslagaccount. Voor het ophalen van gegevens over uw storage-account van de verbindingsreeks voor opslag, kunt u de **parseren** methode.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Procedure: een wachtrij maken
Een **cloud_queue_client** object kunt u profiteren van reference-objecten voor wachtrijen. De volgende code maakt een **cloud_queue_client** object.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Gebruik de **cloud_queue_client** object verwijst naar de wachtrij die u wilt gebruiken. U kunt de wachtrij maken als deze niet bestaat.

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: een bericht in een wachtrij invoegen
Als u wilt invoegen van een bericht in een bestaande wachtrij maakt een nieuwe **cloud_queue_message**. Daarna roept de **add_message** methode. Een **cloud_queue_message** kan worden gemaakt vanuit een tekenreeks of een **byte** matrix. Met deze code wordt er een wachtrij gemaakt (als deze nog niet bestaat) en het bericht 'Hello, World' toegevoegd:

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it to the queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-the-next-message"></a>Hoe: bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de **peek_message** methode.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at the next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output the message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: de inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. Met de volgende code wordt het bericht in de wachtrij bijgewerkt met nieuwe inhoud en wordt de time-out voor de zichtbaarheid met 60 seconden verlengd. Hiermee wordt de status van de werkitems die aan het bericht zijn gekoppeld, opgeslagen en krijgt de client een extra minuut om aan het bericht te blijven werken. U kunt deze techniek gebruiken om uit meerdere stappen bestaande werkstromen in berichten in de wachtrij te volgen zonder dat u helemaal opnieuw hoeft te beginnen als een verwerkingsstap vanwege een hardware- of softwarefout is mislukt. Doorgaans houdt u ook een aantal voor nieuwe pogingen en als het bericht is meer dan n keren geprobeerd, verwijdert u het. Dit biedt bescherming tegen berichten die een toepassingsfout activeren telkens wanneer ze worden verwerkt.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the message from the queue and update the message contents.
// The visibility timeout "0" means make it visible immediately.
// The visibility timeout "60" means the client can get another minute to continue
// working on the message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output the message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-the-next-message"></a>Procedure: het volgende bericht uit de wachtrij
Met uw code wordt een bericht in twee stappen uit de wachtrij verwijderd. Als u aanroept **get_message**, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd door **get_message** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Voor het voltooien van het bericht uit de wachtrij te verwijderen, moet u ook aanroepen **delete_message**. Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. Uw code haalt **delete_message** direct nadat het bericht is verwerkt.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get the next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete the message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Hoe: gebruikmaken van aanvullende opties voor berichten in de wachtrij plaatsen
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de **get_messages** methode om 20 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een **voor** lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Let op: de vijf minuten voor alle berichten op hetzelfde moment start dus na 5 minuten zijn verstreken sinds de aanroep van **get_messages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display the contents of the message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-the-queue-length"></a>Hoe: lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **download_attributes** methode vraagt de Queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De **approximate_message_count** methode het geschatte aantal berichten in de wachtrij opgehaald.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch the queue attributes.
queue.download_attributes();

// Retrieve the cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Procedure: een wachtrij verwijderen
Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de **delete_queue_if_exists** methode voor het object in de wachtrij.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If the queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Storage.

* [Het Blob Storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hoe Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Lijst met Azure Storage-Resources in C++](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Opslagclientbibliotheek voor C++-verwijzing](http://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)