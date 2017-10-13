---
title: Aan de slag met Azure Queue Storage met .NET | Microsoft Docs
description: Azure Queues biedt betrouwbare, asynchrone uitwisseling van berichten tussen toepassingsonderdelen. Met Cloud Messaging kunnen onderdelen van uw toepassing onafhankelijk van elkaar worden opgeschaald.
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c0f82537-a613-4f01-b2ed-fc82e5eea2a7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: tamram
ms.openlocfilehash: 00c737205c8970bf3cd3036b8bfa653c962949d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>Aan de slag met Azure Queue Storage met .NET
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Overzicht
Azure Queue Storage biedt uitwisseling van berichten tussen toepassingsonderdelen in de cloud. Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt daarnaast ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

### <a name="about-this-tutorial"></a>Over deze zelfstudie
Deze zelfstudie laat zien hoe u .NET-code kunt schrijven voor een aantal algemene scenario's die gebruikmaken van Azure Queue Storage. Scenario's die aan bod komen, zijn onder meer het maken en verwijderen van wachtrijen en het toevoegen, lezen en verwijderen van wachtrijberichten.

**Geschatte duur:** 45 minuten

**Vereisten:**

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-clientbibliotheek voor .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager voor .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* Een [Azure Storage-account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Using-instructies toevoegen
Voeg de volgende `Program.cs`-instructies aan het begin van het bestand `using` toe:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-the-connection-string"></a>De verbindingsreeks parseren
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>De Queue-serviceclient maken
Met de **CloudQueueClient**-klasse kunt u wachtrijen ophalen die zijn opgeslagen in Queue Storage. Hier volgt één manier om de serviceclient te maken:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
U bent nu klaar om code te schrijven die gegevens leest uit en schrijft naar Queue Storage.

## <a name="create-a-queue"></a>Een wachtrij maken
In dit voorbeeld ziet u hoe u een wachtrij kunt maken als deze nog niet bestaat:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a>Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een bestaande wachtrij maakt u eerst een nieuwe **CloudQueueMessage**. Daarna roept u de methode **AddMessage** aan. Een **CloudQueueMessage** kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een **byte**matrix. Met deze code wordt er een wachtrij gemaakt (als deze nog niet bestaat) en het bericht 'Hello, World' toegevoegd:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-the-next-message"></a>Bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het uit de wachtrij te verwijderen, door de methode **PeekMessage** aan te roepen.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-the-contents-of-a-queued-message"></a>De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. Met de volgende code wordt het bericht in de wachtrij bijgewerkt met nieuwe inhoud en wordt de time-out voor de zichtbaarheid met 60 seconden verlengd. Hiermee wordt de status van de werkitems die aan het bericht zijn gekoppeld, opgeslagen en krijgt de client een extra minuut om aan het bericht te blijven werken. U kunt deze techniek gebruiken om uit meerdere stappen bestaande werkstromen in berichten in de wachtrij te volgen zonder dat u helemaal opnieuw hoeft te beginnen als een verwerkingsstap vanwege een hardware- of softwarefout is mislukt. Doorgaans houdt u ook het aantal nieuwe pogingen bij en als het bericht meer dan *n* keer opnieuw is geprobeerd, verwijdert u het. Dit biedt bescherming tegen berichten die een toepassingsfout activeren telkens wanneer ze worden verwerkt.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-the-next-message"></a>Het volgende bericht uit de wachtrij verwijderen
Met uw code wordt een bericht in twee stappen uit de wachtrij verwijderd. Wanneer u **GetMessage** aanroept, wordt het volgende bericht in een wachtrij opgehaald. Een bericht dat wordt geretourneerd door **GetMessage**, wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden. Om het bericht definitief uit de wachtrij te verwijderen, moet u ook **DeleteMessage** aanroepen. Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. Uw code haalt **DeleteMessage** op zodra het bericht is verwerkt.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Het Async-Await-patroon gebruiken met algemene Queue Storage-API's
Dit voorbeeld laat zien hoe u het Async-Await-patroon gebruikt met algemene Queue Storage-API's. In het voorbeeld wordt de asynchrone versie aangeroepen van elk van de opgegeven methoden. Dit ziet u aan het achtervoegsel *Async* van elke methode. Wanneer er een asynchrone methode wordt gebruikt, schort het Async-Await-patroon lokale uitvoering uit totdat de aanroep is voltooid. Dit gedrag stelt de huidige thread in staat andere bewerkingen uit te voeren, zodat knelpunten in de prestaties worden voorkomen en de algehele respons van uw toepassing verbetert. Zie [Async en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie over het gebruik van het Async-Await-patroon in .NET.

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a>Gebruikmaken van aanvullende opties voor het verwijderen van berichten uit de wachtrij
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.
Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. In het volgende codevoorbeeld wordt de methode **GetMessages** gebruikt om 20 berichten in één aanroep op te halen. Vervolgens wordt elk bericht verwerkt met behulp van een **foreach**-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd voor ogen dat de periode van 5 minuten voor alle berichten op hetzelfde moment start. Nadat er 5 minuten zijn verstreken sinds de aanroep van **GetMessages**, worden dus alle berichten die niet zijn verwijderd, opnieuw zichtbaar.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De methode **FetchAttributes** vraagt de Queue-service de wachtrij-kenmerken, zoals het aantal berichten, op te halen. De eigenschap **ApproximateMessageCount** retourneert de laatste waarde die is opgehaald door de methode **FetchAttributes**, zonder de Queue-service aan te roepen.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a>Een wachtrij verwijderen
Als u een wachtrij en alle berichten hierin wilt verwijderen, roept u de methode **Delete** aan in het wachtrijobject.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```
    

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Queue Storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* Bekijk de naslagdocumentatie over de Queue-service voor meer informatie over beschikbare API's:
  * [Naslaginformatie over de Storage-clientbibliotheek voor .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [Naslaginformatie over REST API](http://msdn.microsoft.com/library/azure/dd179355)
* Leer hoe u de code die u schrijft om te werken met Azure Storage, kunt vereenvoudigen met behulp van de [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).
* Bekijk meer functiehandleidingen voor informatie over aanvullende mogelijkheden voor het opslaan van gegevens in Azure.
  * [Aan de slag met Azure Table Storage met .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) voor het opslaan van gestructureerde gegevens.
  * [Aan de slag met Azure Blob Storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md) voor het opslaan van niet-gestructureerde gegevens.
  * [Verbinding maken met SQL Database met behulp van .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md) voor het opslaan van relationele gegevens.

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
