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
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een ASP.NET Core-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services toevoegen** dialoogvenster. De **verbonden Services toevoegen** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden.

Azure queue storage is een service voor het opslaan van grote aantallen berichten die toegankelijk zijn vanuit overal ter wereld, via geverifieerde aanroepen via HTTP of HTTPS. Een enkel wachtrijbericht mag maximaal 64 kB (KB) en een wachtrij kan miljoenen berichten, tot aan de totale capaciteitslimiet van een opslagaccount bevatten.

Om te beginnen, moet u eerst een Azure-wachtrij maken in uw opslagaccount. Leert u hoe u een wachtrij in code maken. Ook ziet u hoe u eenvoudige wachtrij bewerkingen uitvoeren, zoals het toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten. De voorbeelden zijn geschreven in C\# code en het gebruik van de Azure Storage-clientbibliotheek voor .NET. Zie voor meer informatie over ASP.NET [ASP.NET](http://www.asp.net).

**Opmerking:** enkele van de API's die het uitvoeren van aanroepen naar Azure storage in ASP.NET Core zijn asynchroon. Zie [asynchrone programmering met Async en Await](http://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie. De volgende code wordt ervan uitgegaan programming async-methoden worden gebruikt.

* Zie [aan de slag met Azure Queue storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie over het bewerken van programmatisch wachtrijen.
* Zie [documentatie Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.
* Zie [Cloud Services-documentatie](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure-cloudservices.
* Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren van ASP.NET-toepassingen.

## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code
Voor toegang tot wachtrijen in ASP.NET Core projecten, moet u de volgende items naar een C# bronbestand die toegang heeft tot Azure queue storage bevatten.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het C#-bestand zijn deze **met** instructies.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Ophalen van een **CloudStorageAccount** -object met gegevens over uw storage-account. De volgende code gebruiken om op te halen de uw verbindingsreeks voor opslag en opslag accountgegevens van de configuratie van Azure service.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Ophalen van een **CloudQueueClient** object om te verwijzen naar de wachtrij-objecten in uw opslagaccount.  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Ophalen van een **CloudQueue** object om te verwijzen naar een bepaalde wachtrij.
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Opmerking:** alle bovenstaande code voor de code in de volgende voorbeelden gebruiken.

### <a name="create-a-queue-in-code"></a>Een wachtrij in code maken
Om de Azure-wachtrij in code maken, voegt u toe een aanroep van **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij
Als u wilt een bericht in een bestaande wachtrij ingevoegd, maakt u een nieuwe **CloudQueueMessage** Roep vervolgens object de **AddMessageAsync** methode.

Een **CloudQueueMessage** object kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix.

Hier volgt een voorbeeld waarmee het bericht 'Hello, World'.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de **PeekMessageAsync** methode.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij
Uw code kunt verwijderen (in wachtrij) een bericht van een wachtrij in twee stappen.

1. Roep **GetMessageAsync** ophalen van het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd door **GetMessageAsync** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden.
2. Aanroepen voor het voltooien van het bericht uit de wachtrij verwijderen **DeleteMessageAsync**.

Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. De volgende code aanroepen **DeleteMessageAsync** direct nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Gebruikmaken van aanvullende opties voor berichten waarbij
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.
Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht. In het volgende codevoorbeeld wordt de methode **GetMessages** gebruikt om 20 berichten in één aanroep op te halen. Vervolgens wordt elk bericht verwerkt met behulp van een **foreach**-lus. De time-out voor onzichtbaarheid wordt ingesteld op 5 minuten voor elk bericht. Denk eraan dat de vijf minuten voor alle berichten op hetzelfde moment start doorgegeven dus na 5 minuten zijn na de aanroep **GetMessages**, alle berichten die niet zijn verwijderd weer worden weergegeven.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **FetchAttributes** methode vraagt de queue-service voor het ophalen van de wachtrij-kenmerken, zoals het aantal berichten. De **ApproximateMethodCount** eigenschap retourneert de laatste waarde die is opgehaald door de **FetchAttributes** methode, zonder de queue-service aanroepen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>De Async-Await-patroon gebruiken met algemene queue API 's
In dit voorbeeld laat zien hoe het Async-Await-patroon gebruiken met algemene queue API's. Het voorbeeld roept de async-versie van elk van de opgegeven methoden. Deze kunnen worden gezien door de asynchrone na herstel van elke methode. Wanneer een async-methode wordt gebruikt, schort het Async-Await-patroon lokale uitvoering totdat de aanroep is voltooid. Dit gedrag kan de huidige thread voor andere werk die knelpunten in de prestaties worden voorkomen en verbetert de algehele respons van uw toepassing. Zie voor meer informatie over het gebruik van het Async-Await-patroon in .NET [Async en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

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
## <a name="delete-a-queue"></a>Een wachtrij verwijderen
Als u een wachtrij en alle berichten hierin wilt verwijderen, roept u de methode **Delete** aan in het wachtrijobject.

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

