---
title: Aan de slag met queue storage en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Aan de slag met Azure queue-opslag in een ASP.NET Core-project in Visual Studio
services: storage
author: ghogen
manager: douge
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 8a91614e7dfb804e6a902967ce60f898ed0e54ad
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999758"
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met queue storage en Visual Studio verbonden services (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

In dit artikel wordt beschreven hoe u aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **Connected Services** functie. De **Connected Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure storage in uw project en de verbindingsreeks voor het opslagaccount toegevoegd aan uw project-configuratiebestanden. (Zie [documentatie voor Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure queue storage is een service voor het opslaan van grote aantallen berichten die kunnen worden benaderd vanaf elke locatie ter wereld via geverifieerde oproepen met HTTP of HTTPS. Een enkel wachtrijbericht mag maximaal 64 kilobytes (KB) en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount bevatten. Zie ook [aan de slag met Azure Queue storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie over het bewerken van programmatisch wachtrijen.

Als u wilt beginnen, moet u eerst een Azure-wachtrij maken in uw storage-account. In dit artikel wordt weergegeven over het maken van een wachtrij in C# en hoe u eenvoudige wachtrijbewerkingen zoals het toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten uit te voeren.  De code gebruikt de Azure Storage-clientbibliotheek voor .NET. Zie voor meer informatie over ASP.NET [ASP.NET](https://www.asp.net).

Enkele van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code

Voor toegang tot wachtrijen in ASP.NET Core-projecten, bevatten de volgende items in een C# bronbestand die toegang heeft tot Azure-wachtrijopslag. Alle deze code voor de code in de volgende secties gebruiken.

1. Toevoegen van de benodigde `using` instructies:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Krijgen een `CloudStorageAccount` -object met gegevens van uw opslagaccount. Gebruik de volgende code om op te halen van de verbindingsreeks voor opslag en gegevens over het opslagaccount van de configuratie van de Azure-service:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Krijgen een `CloudQueueClient` object om te verwijzen naar de wachtrijobjecten in uw storage-account:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Krijgen een `CloudQueue` object om te verwijzen naar een bepaalde wachtrij:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Een wachtrij in code maken

Aanroepen voor het maken van de Azure-wachtrij in de code, `CreateIfNotExistsAsync`:

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Voor het invoegen van een bericht in een bestaande wachtrij, maakt u een nieuw `CloudQueueMessage` object en roep vervolgens de `AddMessageAsync` methode. Een `CloudQueueMessage` object kan worden gemaakt op basis van een tekenreeks (in UTF-8-indeling) of een bytematrix.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen

U kunt het bericht vooraan in een wachtrij bekijken zonder deze te verwijderen uit de wachtrij door het aanroepen van de `PeekMessageAsync` methode:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (uit de wachtrij verwijderen) een bericht van een wachtrij in twee stappen.

1. Bel `GetMessageAsync` om op te halen van het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd van `GetMessageAsync` wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden.
1. Als u klaar bent met het bericht verwijderen uit de wachtrij, wilt aanroepen `DeleteMessageAsync`.

Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. De volgende code aanroepen `DeleteMessageAsync` direct nadat het bericht is verwerkt:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>Aanvullende opties voor berichten eruit worden verwijderd

Er zijn twee manieren om aan te passen bericht dat wordt opgehaald uit een wachtrij. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de `GetMessages` methode voor het ophalen van 20 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een `foreach` lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de vijf minuten timer start voor alle berichten op hetzelfde moment, dus na vijf minuten zijn verstreken, alle berichten die niet zijn verwijderd opnieuw zichtbaar.

```cs
// Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
//   delete each message after processing.

foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen

U kunt een schatting ophalen van het aantal berichten in de wachtrij. De `FetchAttributes` methode vraagt de queue-service om op te halen van de wachtrij-kenmerken, zoals het aantal berichten. De `ApproximateMethodCount` eigenschap retourneert de laatste waarde die is opgehaald door de `FetchAttributes` methode, zonder het aanroepen van de queue-service.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Het Async-Await-patroon gebruiken met algemene queue API 's

In dit voorbeeld ziet u hoe u het async-await-patroon met algemene queue-API's die eindigt op `Async`. Wanneer een asynchrone methode wordt gebruikt, wordt het async-await patroon lokale uitvoering wordt onderbroken totdat de aanroep is voltooid. Dit gedrag kunt de huidige thread andere werkzaamheden die voorkomen dat er knelpunten en verbetert de algehele respons van uw toepassing.

```cs
// Create a message to add to the queue.
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message.
await messageQueue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="delete-a-queue"></a>Een wachtrij verwijderen

Als wilt verwijderen van een wachtrij en alle berichten die erin zijn opgenomen, roept de `Delete` methode voor het object in de wachtrij:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
