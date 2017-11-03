---
title: Hoe Queue storage gebruiken met Java | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten. Voorbeelden die zijn geschreven in Java.
services: storage
documentationcenter: java
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 68cecc8e-38c9-4a24-99e8-cb722bc63cf9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 6735e247393e47ed18049c8055eb92b990e8bb02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-queue-storage-from-java"></a>Queue Storage gebruiken met Java
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Overzicht
Deze handleiding leert u hoe u veelvoorkomende scenario's met behulp van de Azure Queue storage-service uitvoert. De voorbeelden zijn geschreven in Java en gebruik de [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]. De scenario's worden behandeld: **invoegen**, **inspecteren**, **ophalen**, en **verwijderen** wachtrij berichten, evenals **maken** en **verwijderen** wachtrijen. Zie voor meer informatie over wachtrijen de [Vervolgstappen](#Next-Steps) sectie.

Opmerking: Een SDK is beschikbaar voor ontwikkelaars die werken met Azure Storage op Android-apparaten. Zie voor meer informatie de [Azure-opslag-SDK voor Android][Azure Storage SDK for Android].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Een Java-toepassing maken
In deze handleiding gebruikt u opslagfuncties die kunnen worden uitgevoerd binnen een Java-toepassing lokaal of in de code die wordt uitgevoerd binnen een Webrol of worker-rol in Azure.

Om dit te doen, moet u Java Development Kit (JDK) installeren en een Azure storage-account maken in uw Azure-abonnement. Als u dit hebt gedaan, moet u controleren of uw ontwikkelsysteem voldoet aan de minimale vereisten en afhankelijkheden die worden vermeld in de [Azure-opslag-SDK voor Java] [ Azure Storage SDK for Java] opslagplaats op GitHub. Als uw systeem aan deze vereisten voldoet, kunt u de instructies voor het downloaden en installeren van de Azure Storage-bibliotheken voor Java op uw systeem vanuit die opslagplaats. Nadat u deze taken hebt voltooid, kunt u zich kunt maken van een Java-toepassing die gebruikmaakt van de voorbeelden in dit artikel.

## <a name="configure-your-application-to-access-queue-storage"></a>Uw toepassing configureren voor toegang tot opslag van de wachtrij
De volgende importinstructies toevoegen aan het begin van de Java-bestand waarin u gebruiken van Azure storage-API's wilt voor toegang tot wachtrijen:

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>Een Azure-opslag-verbindingsreeks instellen
Een Azure-opslag-client gebruikt een verbindingsreeks voor opslag voor het opslaan van eindpunten en referenties voor toegang tot gegevens beheerservices. Wanneer u in een clienttoepassing uitvoert, moet u opgeven de verbindingsreeks voor opslag in de volgende indeling met de naam van uw opslagaccount en de primaire toegangssleutel voor het opslagaccount vermeld in de [Azure Portal](https://portal.azure.com) voor de *AccountName* en *AccountKey* waarden. Dit voorbeeld ziet hoe u een statisch veld voor het opslaan van de verbindingsreeks kunt declareren:

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

## <a name="how-to-create-a-queue"></a>Procedure: een wachtrij maken
Een **CloudQueueClient** object kunt u profiteren van reference-objecten voor wachtrijen. De volgende code maakt een **CloudQueueClient** object. (Opmerking: Er zijn aanvullende manieren maken **CloudStorageAccount** objecten; voor meer informatie Zie **CloudStorageAccount** in de [naslaginformatie over Azure Storage Client SDK].)

Gebruik de **CloudQueueClient** object verwijst naar de wachtrij die u wilt gebruiken. U kunt de wachtrij maken als deze niet bestaat.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a>Procedure: een bericht toevoegen aan een wachtrij
Voor het invoegen van een bericht in een bestaande wachtrij maakt u eerst een nieuwe **CloudQueueMessage**. Daarna roept de **addMessage** methode. Een **CloudQueueMessage** kan worden gemaakt vanuit een tekenreeks (in UTF-8-indeling) of een byte-matrix. Deze code wordt een wachtrij maakt (als deze niet bestaat) en het bericht "Hallo, wereld".

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a>Hoe: bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door aan te roepen **peekMessage**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: de inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. Als het bericht een werktaak vertegenwoordigt, kunt u deze functie gebruiken om de status van de werktaak bij te werken. Met de volgende code wordt het bericht in de wachtrij bijgewerkt met nieuwe inhoud en wordt de time-out voor de zichtbaarheid met 60 seconden verlengd. Hiermee wordt de status van de werkitems die aan het bericht zijn gekoppeld, opgeslagen en krijgt de client een extra minuut om aan het bericht te blijven werken. U kunt deze techniek gebruiken om uit meerdere stappen bestaande werkstromen in berichten in de wachtrij te volgen zonder dat u helemaal opnieuw hoeft te beginnen als een verwerkingsstap vanwege een hardware- of softwarefout is mislukt. Doorgaans houdt u ook het aantal nieuwe pogingen bij en als het bericht meer dan *n* keer opnieuw is geprobeerd, verwijdert u het. Dit biedt bescherming tegen berichten die een toepassingsfout activeren telkens wanneer ze worden verwerkt.

De volgende code voorbeeld gezocht in de wachtrij van berichten, zoekt de client het eerste bericht dat overeenkomt met "Hallo, wereld" voor de inhoud vervolgens wijzigt het bericht inhoud en wordt afgesloten.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

U kunt ook updates het volgende codevoorbeeld alleen het eerste zichtbare bericht in de wachtrij.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a>Hoe: lengte van de wachtrij ophalen
U kunt een schatting ophalen van het aantal berichten in de wachtrij. De **downloadAttributes** methode vraagt de Queue-service voor verschillende huidige waarden, met inbegrip van een telling van het aantal berichten in een wachtrij zijn. Het aantal is alleen een benadering omdat berichten kunnen worden toegevoegd of verwijderd nadat de Queue-service op uw aanvraag reageert. De **getApproximateMessageCount** methode retourneert de laatste waarde die is opgehaald door de aanroep **downloadAttributes**, zonder de Queue-service aanroepen.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a>Procedure: het volgende bericht in wachtrij
Uw code dequeues een bericht van een wachtrij in twee stappen. Als u aanroept **retrieveMessage**, krijgt u het volgende bericht in een wachtrij. Een bericht dat wordt geretourneerd door **retrieveMessage** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. Standaard blijft het bericht onzichtbaar gedurende 30 seconden. Voor het voltooien van het bericht uit de wachtrij te verwijderen, moet u ook aanroepen **deleteMessage**. Dit proces in twee stappen voor het verwijderen van een bericht zorgt ervoor dat als de code er niet in slaagt een bericht te verwerken vanwege hardware- of softwareproblemen, een ander exemplaar van uw code hetzelfde bericht kan ophalen en het opnieuw kan proberen. Uw code haalt **deleteMessage** direct nadat het bericht is verwerkt.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a>Aanvullende opties voor berichten waarbij
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen. Ten eerste kunt u berichten batchgewijs (maximaal 32) ophalen. Ten tweede kunt u een langere of kortere time-out voor onzichtbaarheid instellen, zodat uw code meer of minder tijd krijgt voor het volledig verwerken van elk bericht.

Het volgende codevoorbeeld wordt de **retrieveMessages** methode om 20 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een **voor** lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten (300 seconden) voor elk bericht. Denk eraan dat de vijf minuten wordt gestart voor alle berichten op hetzelfde moment, dus wanneer vijf minuten verstreken sinds de aanroep van **retrieveMessages**, alle berichten die niet zijn verwijderd weer zichtbaar worden.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a>Procedure: de wachtrijen lijst
Aanroepen voor een lijst van de huidige wachtrijen, de **CloudQueueClient.listQueues()** methode een verzameling van retourneert **CloudQueue** objecten.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a>Procedure: een wachtrij verwijderen
Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de **deleteIfExists** methode op de **CloudQueue** object.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* [Azure-opslag-SDK voor Java][Azure Storage SDK for Java]
* [naslaginformatie over Azure Storage Client SDK][naslaginformatie over Azure Storage Client SDK]
* [REST-API van Azure Storage-Services][Azure Storage Services REST API]
* [Azure Storage-teamblog][Azure Storage Team Blog]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[naslaginformatie over Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
