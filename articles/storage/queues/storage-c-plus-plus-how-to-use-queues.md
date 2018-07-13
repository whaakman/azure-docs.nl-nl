---
title: Het gebruik van de queue-opslag (C++) | Microsoft Docs
description: Informatie over het gebruik van de queue storage-service in Azure. Voorbeelden zijn geschreven in C++.
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38618463"
---
# <a name="how-to-use-queue-storage-from-c"></a>Hoe u Queue Storage gebruiken met C++
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe algemene scenario's met behulp van de Azure Queue storage-service uitvoert. De voorbeelden zijn geschreven in C++ en maken gebruik van de [Azure Storage-clientbibliotheek voor C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). De behandelde scenario's zijn **invoegen**, **inspecteren**, **aan**, en **verwijderen** berichten in de wachtrij, evenals  **het maken en verwijderen van wachtrijen**.

> [!NOTE]
> Deze handleiding is gericht op de Azure-opslagclientbibliotheek voor C++ versie 1.0.0 en hoger. De aanbevolen versie is opslagclientbibliotheek 2.2.0. Deze is beschikbaar via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](http://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Een C++-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een C++-toepassing.

Hiervoor moet u de Azure-opslagclientbibliotheek voor C++ installeren en een Azure-opslagaccount in uw Azure-abonnement maken.

Voor het installeren van de Azure-opslagclientbibliotheek voor C++ kunt u de volgende methoden gebruiken:

* **Linux:** Volg de instructies de [Azure Storage-clientbibliotheek voor C++ Leesmij](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) pagina.
* **Windows:** klik in Visual Studio op **Tools > NuGet Package Manager > Package Manager Console**. Typ de volgende opdracht in de [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) en druk op **ENTER**.

```  
Install-Package wastorage
```

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing configureren voor toegang tot Queue Storage
Voeg dat de volgende instructies toe aan het begin van de C++-bestand dat is waar u de Azure storage-API's gebruiken voor toegang tot wachtrijen zijn:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Instellen van een Azure storage-verbindingsreeks
Een Azure-opslagclient gebruikt een opslagverbindingstekenreeks voor het opslaan van eindpunten en referenties voor toegang tot gegevensbeheerservices. Wanneer in een clienttoepassing wordt uitgevoerd, moet u de verbindingsreeks voor opslag in de volgende indeling, met de naam van uw opslagaccount en de toegangssleutel voor het opslagaccount dat is vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Zie voor meer informatie over storage-accounts en toegangssleutels [over Azure Storage-Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json). In dit voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de verbindingstekenreeks:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Als u wilt uw toepassing testen op uw lokale Windows-computer, kunt u de Microsoft Azure [opslagemulator](../common/storage-use-emulator.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) die is geïnstalleerd met de [Azure SDK](https://azure.microsoft.com/downloads/). De opslagemulator is een hulpprogramma waarmee de services Blob, Queue en Table beschikbaar zijn in Azure op uw lokale ontwikkelcomputer simuleert. In het volgende voorbeeld ziet u hoe u een statisch veld kunt declareren voor het opslaan van de verbindingstekenreeks naar uw lokale opslagemulator:  

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Selecteer eerst de Azure-opslagemulator de **Start** of drukt u op de **Windows** sleutel. Begin met typen **Azure-Opslagemulator**, en selecteer **Microsoft Azure-Opslagemulator** uit de lijst met toepassingen.

In de volgende voorbeelden wordt ervan uitgegaan dat u een van deze twee methoden hebt gebruikt om de opslagverbindingstekenreeks op te halen.

## <a name="retrieve-your-connection-string"></a>De verbindingsreeks ophalen
U kunt de **cloud_storage_account** klasse om weer te geven van de gegevens van uw Opslagaccount. Voor het ophalen van uw opslagaccountgegevens uit de opslagverbindingstekenreeks kunt u de methode **parse** gebruiken.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Hoe: een wachtrij maken
Een **cloud_queue_client** object kunt u de referentie-objecten ophalen voor wachtrijen. De volgende code maakt een **cloud_queue_client** object.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Gebruik de **cloud_queue_client** object verwijst naar de wachtrij die u wilt gebruiken. U kunt de wachtrij maken als deze nog niet bestaat.

```cpp
// Retrieve a reference to a queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create the queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Hoe: een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een bestaande wachtrij, eerst maakt u een nieuw **cloud_queue_message**. Daarna roept de **add_message** methode. Een **cloud_queue_message** kan worden gemaakt vanuit een tekenreeks of een **byte** matrix. Met deze code wordt er een wachtrij gemaakt (als deze nog niet bestaat) en het bericht 'Hello, World' toegevoegd:

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

## <a name="how-to-peek-at-the-next-message"></a>Hoe: het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder deze te verwijderen uit de wachtrij door het aanroepen van de **peek_message** methode.

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

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Hoe: de inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. Met de volgende code wordt het bericht in de wachtrij bijgewerkt met nieuwe inhoud en wordt de time-out voor de zichtbaarheid met 60 seconden verlengd. Hiermee wordt de status van de werkitems die aan het bericht zijn gekoppeld, opgeslagen en krijgt de client een extra minuut om aan het bericht te blijven werken. U kunt deze techniek gebruiken om uit meerdere stappen bestaande werkstromen in berichten in de wachtrij te volgen zonder dat u helemaal opnieuw hoeft te beginnen als een verwerkingsstap vanwege een hardware- of softwarefout is mislukt. Doorgaans houdt u ook aantal nieuwe pogingen, en als het bericht is meer dan n keer geprobeerd, verwijdert u het. Dit biedt bescherming tegen berichten die een toepassingsfout activeren telkens wanneer ze worden verwerkt.

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

## <a name="how-to-de-queue-the-next-message"></a>Hoe: het volgende bericht uit de wachtrij
Met uw code wordt een bericht in twee stappen uit de wachtrij verwijderd. Als u aanroept **get_message**, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd van **get_message** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Voor het voltooien van het bericht uit de wachtrij verwijderen, moet u ook aanroepen **delete_message**. Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. Uw code roept **delete_message** direct nadat het bericht is verwerkt.

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

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Hoe: gebruikmaken van aanvullende opties voor berichten ongedaan maken uit de wachtrij
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de **get_messages** methode voor het ophalen van 20 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een **voor** lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de vijf minuten voor alle berichten wordt gestart op hetzelfde moment, dus na 5 minuten zijn verstreken sinds de aanroep naar **get_messages**, alle berichten die niet zijn verwijderd, opnieuw zichtbaar.

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
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **download_attributes** methode vraagt de Queue-service om op te halen van de wachtrij-kenmerken, zoals het aantal berichten. De **approximate_message_count** methode haalt het geschatte aantal berichten in de wachtrij.

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

## <a name="how-to-delete-a-queue"></a>Hoe: een wachtrij verwijderen
Als wilt verwijderen van een wachtrij en alle berichten die erin zijn opgenomen, roept de **delete_queue_if_exists** methode voor het object in de wachtrij.

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
Nu dat u de basisprincipes van Queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Storage.

* [Het Blob Storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hoe u kunt Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Lijst met Azure Storage-Resources in C++](../common/storage-c-plus-plus-enumeration.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Storage-clientbibliotheek voor C++-referentie](http://azure.github.io/azure-storage-cpp)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)