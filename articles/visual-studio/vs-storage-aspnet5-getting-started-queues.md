---
title: Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET Core) | Microsoft Docs
description: Hoe u aan de slag met Azure queue storage in een ASP.NET Core-project in Visual Studio
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: kraigb
ms.openlocfilehash: d83ad26e4f96e1a7670c5212b7e9ca8182b7cec4
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

Dit artikel wordt beschreven hoe u aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services** functie. De **verbonden Services** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden. (Zie [documentatie Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.)

Azure queue storage is een service voor het opslaan van grote aantallen berichten die toegankelijk zijn vanuit overal ter wereld, via geverifieerde aanroepen via HTTP of HTTPS. Een enkel wachtrijbericht mag maximaal 64 kB (KB) en een wachtrij kan miljoenen berichten, tot aan de totale capaciteitslimiet van een opslagaccount bevatten. Zie ook [aan de slag met Azure Queue storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie over het bewerken van programmatisch wachtrijen.

Om te beginnen, moet u eerst een Azure-wachtrij maken in uw opslagaccount. In dit artikel wordt weergegeven hoe u een wachtrij maakt in C# en hoe u eenvoudige wachtrij uitvoert zoals het toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten.  De code gebruikt de Azure Storage-clientbibliotheek voor .NET. Zie voor meer informatie over ASP.NET [ASP.NET](http://www.asp.net).

Sommige van de Azure Storage-API's zijn asynchroon en de code in dit artikel wordt ervan uitgegaan dat async-methoden worden gebruikt. Zie [asynchrone programmering](https://docs.microsoft.com/dotnet/csharp/async) voor meer informatie.

## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code

Voor toegang tot wachtrijen in ASP.NET Core projecten, bevatten de volgende items in een C# bronbestand die toegang heeft tot Azure queue storage. Alle deze code voor de code in de volgende secties gebruiken.

1. Voeg de vereiste `using` instructies:
    ```cs
    using Microsoft.Framework.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Framework.Logging.LogLevel;
    ```

1. Ophalen van een `CloudStorageAccount` -object met gegevens over uw storage-account. Gebruik de volgende code om de verbindingsreeks voor opslag en de gegevens van de storage-account van de configuratie van de Azure-service:

    ```cs
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Ophalen van een `CloudQueueClient` object om te verwijzen naar de wachtrij-objecten in uw opslagaccount:

    ```cs
    // Create the CloudQueueClient object for the storage account.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Ophalen van een `CloudQueue` object om te verwijzen naar een specifieke wachtrij:

    ```cs
    // Get a reference to the CloudQueue named "messagequeue"
    CloudQueue messageQueue = queueClient.GetQueueReference("messagequeue");
    ```

### <a name="create-a-queue-in-code"></a>Een wachtrij in code maken

Aanroepen voor het maken van de Azure-wachtrij in de code, '' CreateIfNotExistsAsync':

```cs
// Create the CloudQueue if it does not exist.
await messageQueue.CreateIfNotExistsAsync();
```

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij

Als u wilt een bericht in een bestaande wachtrij ingevoegd, maakt u een nieuwe `CloudQueueMessage` Roep vervolgens object de `AddMessageAsync` methode. Een `CloudQueueMessage` object kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix.

```cs
// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
await messageQueue.AddMessageAsync(message);
```

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen

U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de `PeekMessageAsync` methode:

```cs
// Peek the next message in the queue.
CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();
```

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij

Uw code kunt verwijderen (in wachtrij) een bericht van een wachtrij in twee stappen.

1. Roep `GetMessageAsync` ophalen van het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd door `GetMessageAsync` wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden.
1. Aanroepen voor het voltooien van het bericht uit de wachtrij verwijderen `DeleteMessageAsync`.

Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. De volgende code aanroepen `DeleteMessageAsync` direct nadat het bericht is verwerkt:

```cs
// Get the next message in the queue.
CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

// Process the message in less than 30 seconds.

// Then delete the message.
await messageQueue.DeleteMessageAsync(retrievedMessage);
```

## <a name="additional-options-for-dequeuing-messages"></a>Aanvullende opties voor berichten waarbij

Er zijn twee manieren om aan te passen ophalen van berichten uit een wachtrij. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. Het volgende codevoorbeeld wordt de `GetMessages` methode om 20 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een `foreach` lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd er rekening mee dat de vijf minuten timer start voor alle berichten op hetzelfde moment dus na 5 minuten zijn verstreken, alle berichten die niet zijn verwijderd zichtbaar opnieuw.

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

U kunt een schatting ophalen van het aantal berichten in de wachtrij. De `FetchAttributes` methode vraagt de queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De `ApproximateMethodCount` eigenschap retourneert de laatste waarde die is opgehaald door de `FetchAttributes` methode, zonder de queue-service aanroepen.

```cs
// Fetch the queue attributes.
messageQueue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = messageQueue.ApproximateMessageCount;

// Display the number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>De Async-Await-patroon gebruiken met algemene queue API 's

Dit voorbeeld ziet u het gebruik van het async-await-patroon met gemeenschappelijke wachtrij-API's eindigend op `Async`. Wanneer een async-methode wordt gebruikt, wordt het async-await patroon lokale uitvoering wordt uitgesteld totdat de aanroep is voltooid. Dit gedrag kan de huidige thread voor andere werk die knelpunten in de prestaties worden voorkomen en verbetert de algehele respons van uw toepassing.

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

Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de `Delete` methode voor het object in de wachtrij:

```cs
// Delete the queue.
messageQueue.Delete();
```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
