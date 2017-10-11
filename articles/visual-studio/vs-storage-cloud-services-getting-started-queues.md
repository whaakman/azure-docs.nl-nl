---
title: Aan de slag met de opslag van de wachtrij en Visual Studio verbonden services (cloudservices) | Microsoft Docs
description: Hoe u aan de slag met Azure Queue storage in een cloud service-project in Visual Studio nadat u verbinding met een opslagaccount met Visual Studio hebt verbonden services
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 7a6e58a62b4cfbf99641559363dd0c860cdf8af2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Aan de slag met Azure Queue storage en Visual Studio verbonden services (cloud services-projecten)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Dit artikel wordt beschreven hoe u aan de slag met Azure Queue storage in Visual Studio nadat u hebt gemaakt of een Azure storage-account in een cloud services-project waarnaar wordt verwezen door het gebruik van de Visual Studio **verbonden Services toevoegen** dialoogvenster.

Leert u hoe u een wachtrij in code maken. Ook ziet u hoe u eenvoudige wachtrij bewerkingen uitvoeren, zoals het toevoegen, wijzigen, lezen en verwijderen van Wachtrijberichten. De voorbeelden zijn geschreven in C#-code en gebruik de [Microsoft Azure Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

De **verbonden Services toevoegen** bewerking installeert de juiste NuGet-pakketten voor toegang tot Azure-opslag in uw project en voegt u de verbindingsreeks voor de storage-account toe aan uw project configuratiebestanden.

* Zie [aan de slag met Azure Queue storage met .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) voor meer informatie over het bewerken van wachtrijen in code.
* Zie [documentatie Storage](https://azure.microsoft.com/documentation/services/storage/) voor algemene informatie over Azure Storage.
* Zie [Cloud Services-documentatie](https://azure.microsoft.com/documentation/services/cloud-services/) voor algemene informatie over Azure-cloudservices.
* Zie [ASP.NET](http://www.asp.net) voor meer informatie over het programmeren van ASP.NET-toepassingen.

Azure Queue Storage is een service voor de opslag van grote aantallen berichten die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd met geverifieerde aanroepen. Een enkel wachtrijbericht mag maximaal 64 KB groot zijn en een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount.

## <a name="access-queues-in-code"></a>Toegang tot wachtrijen in code
Voor toegang tot wachtrijen in Visual Studio Cloud Services-projecten, moet u de volgende items naar een C#-bronbestand die toegang hebben tot Azure Queue storage bevatten.

1. Zorg ervoor dat de naamruimtedeclaraties boven aan het C#-bestand zijn deze **met** instructies.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Ophalen van een **CloudStorageAccount** -object met gegevens over uw storage-account. De volgende code gebruiken om op te halen de uw verbindingsreeks voor opslag en opslag accountgegevens van de configuratie van Azure service.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Ophalen van een **CloudQueueClient** object om te verwijzen naar de wachtrij-objecten in uw opslagaccount.  
   
        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Ophalen van een **CloudQueue** object om te verwijzen naar een bepaalde wachtrij.
   
        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Opmerking:** alle bovenstaande code voor de code in de volgende voorbeelden gebruiken.

## <a name="create-a-queue-in-code"></a>Een wachtrij in code maken
Om de wachtrij in code maken, voegt u toe een aanroep van **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Een bericht toevoegen aan een wachtrij
Als u wilt een bericht in een bestaande wachtrij ingevoegd, maakt u een nieuwe **CloudQueueMessage** Roep vervolgens object de **AddMessage** methode.

Een **CloudQueueMessage** object kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix.

Hier volgt een voorbeeld waarmee het bericht 'Hello, World'.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Een bericht in een wachtrij lezen
U kunt het bericht vooraan in een wachtrij bekijken zonder het uit de wachtrij te verwijderen, door de methode **PeekMessage** aan te roepen.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lezen en verwijderen van een bericht in een wachtrij
Uw code kunt verwijderen (uit de wachtrij) een bericht van een wachtrij in twee stappen.

1. Roep **GetMessage** ophalen van het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd door **GetMessage**, wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden.
2. Aanroepen voor het voltooien van het bericht uit de wachtrij verwijderen **DeleteMessage**.

Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. De volgende code aanroepen **DeleteMessage** direct nadat het bericht is verwerkt.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Aanvullende opties gebruiken om te verwerken en verwijderen van Wachtrijberichten
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.

* U kunt een batch met berichten (maximaal 32) ophalen.
* U kunt een time-out langer of korter onzichtbaarheid instellen zodat uw code meer of minder tijd voor het volledig verwerken van elk bericht. In het volgende codevoorbeeld wordt de methode **GetMessages** gebruikt om 20 berichten in één aanroep op te halen. Vervolgens wordt elk bericht verwerkt met behulp van een **foreach**-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht. Houd voor ogen dat de periode van 5 minuten voor alle berichten op hetzelfde moment start. Nadat er 5 minuten zijn verstreken sinds de aanroep van **GetMessages**, worden dus alle berichten die niet zijn verwijderd, opnieuw zichtbaar.

Hier volgt een voorbeeld:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De methode **FetchAttributes** vraagt de Queue-service de wachtrij-kenmerken, zoals het aantal berichten, op te halen. De **ApproximateMethodCount** eigenschap retourneert de laatste waarde die is opgehaald door de **FetchAttributes** methode, zonder de Queue-service aanroepen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>De Async-Await-patroon gebruiken met algemene Azure Queue-API 's
In dit voorbeeld laat zien hoe het Async-Await-patroon gebruiken met algemene Azure Queue-API's. Het voorbeeld roept de async-versie van elk van de opgegeven methoden, moeten deze kunnen worden gezien door de **asynchrone** na herstel van elke methode. Wanneer u een async-methode gebruikt het async-await patroon lokale uitvoering wordt uitgesteld totdat de aanroep is voltooid. Dit gedrag kan de huidige thread voor andere werk die knelpunten in de prestaties worden voorkomen en verbetert de algehele respons van uw toepassing. Zie [Async en Await (C# en Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) voor meer informatie over het gebruik van het Async-Await-patroon in .NET.

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Een wachtrij verwijderen
Als u een wachtrij en alle berichten hierin wilt verwijderen, roept u de methode **Delete** aan in het wachtrijobject.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

