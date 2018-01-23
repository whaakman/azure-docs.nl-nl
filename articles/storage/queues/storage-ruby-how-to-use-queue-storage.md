---
title: Hoe Queue storage gebruiken met Ruby | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten. De voorbeelden in Ruby geschreven.
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 0d7624d47a6924a5c8dec66b47ac0887ff493879
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-queue-storage-from-ruby"></a>Queue Storage gebruiken met Ruby
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's met de Microsoft Azure Queue Storage-service uitvoeren. De voorbeelden zijn geschreven met de Azure-API voor Ruby.
De scenario's worden behandeld: **invoegen**, **inspecteren**, **ophalen**, en **verwijderen** wachtrij berichten, evenals **maken en verwijderen van wachtrijen**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken
Maak een Ruby toepassing. Zie voor instructies [een Ruby-App maken in App Service op Linux](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag
Voor het gebruik van Azure-opslag die u wilt downloaden en gebruiken van het Ruby azure pakket bevat een set met gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Typ 'azure gem installeren' in het opdrachtvenster voor het installeren van de gem en afhankelijkheden.

### <a name="import-the-package"></a>Het pakket importeren
Uw favoriete teksteditor gebruiken, Voeg het volgende toe aan het begin van de Ruby bestand waarop u wilt opslag gebruiken:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Een Azure-opslag-verbinding instellen
De azure-module leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS_KEY** voor meer informatie verbinding maken met uw Azure storage-account vereist. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens voordat u **Azure::QueueService** met de volgende code:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

Als u deze waarden van een klassiek of Resource Manager-opslagaccount in de Azure-portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar het opslagaccount dat u wilt gebruiken.
3. Klik op de blade instellingen aan de rechterkant **toegangstoetsen**.
4. In de blade van de toegang tot sleutels die wordt weergegeven, ziet u de toegangssleutel 1 en toegangssleutel 2. U kunt een van beide gebruiken. 
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren. 

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken
De volgende code maakt een **Azure::QueueService** object, waarmee u werkt met wachtrijen.

```ruby
azure_queue_service = Azure::QueueService.new
```

Gebruik de **create_queue()** methode voor het maken van een wachtrij met de opgegeven naam.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een wachtrij, gebruikt u de **create_message()** methode voor het maken van een nieuw bericht en voeg deze toe aan de wachtrij.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de **peek\_messages()** methode. Standaard **peek\_messages()** peeks op één bericht. U kunt ook opgeven hoeveel berichten die u wilt bekijken.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht uit de wachtrij halen
U kunt een bericht verwijderen uit een wachtrij in twee stappen.

1. Als u aanroept **lijst\_messages()**, krijgt u het volgende bericht in een wachtrij standaard. U kunt ook opgeven hoeveel berichten die u wilt ophalen. De berichten die zijn geretourneerd door **lijst\_messages()** wordt onzichtbaar voor andere codes die berichten lezen uit deze wachtrij. U doorgeven de zichtbaarheid time-out in seconden als parameter.
2. Voor het voltooien van het bericht uit de wachtrij te verwijderen, moet u ook aanroepen **delete_message()**.

Dit proces in twee stappen van het verwijderen van een bericht zorgt ervoor dat wanneer uw code niet kan een bericht vanwege problemen met hardware of software te verwerken, een ander exemplaar van uw code kunt het bericht verschijnt en probeer het opnieuw. Uw code haalt **verwijderen\_message()** direct nadat het bericht is verwerkt.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in de wachtrij wijzigen. De code hieronder maakt gebruik van de **update_message()** methode voor het bijwerken van een bericht. De methode retourneert een tuple waarin de pop ontvangst van het bericht uit de wachtrij en een UTC date tijd-waarde die aangeeft wanneer het bericht zichtbaar in de wachtrij zijn.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Procedure: Aanvullende opties voor waarbij berichten
Er zijn twee manieren waarop u het ophalen van berichten uit een wachtrij kunt aanpassen.

1. U kunt een batch van bericht ophalen.
2. U kunt een time-out langer of korter onzichtbaarheid instellen zodat uw code meer of minder tijd voor het volledig verwerken van elk bericht.

Het volgende codevoorbeeld wordt de **lijst\_messages()** methode 15 berichten in één aanroep ophalen. Vervolgens wordt afgedrukt en elk bericht wordt verwijderd. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor elk bericht.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen
U kunt een schatting van het aantal berichten in de wachtrij ophalen. De **ophalen\_wachtrij\_metadata()** methode vraagt de queue-service om het aantal berichten bij benadering en metagegevens over de wachtrij te retourneren.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen
Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de **verwijderen\_queue()** methode voor het object in de wachtrij.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* Ga naar de [Blog Azure Storage-Team](http://blogs.msdn.com/b/windowsazurestorage/)
* Ga naar de [Azure SDK voor Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) opslagplaats op GitHub

Voor een vergelijking tussen de Azure Queue-Service die wordt besproken in dit artikel en de Azure Service Bus-wachtrijen beschreven in de [het gebruik van Service Bus-wachtrijen](/develop/ruby/how-to-guides/service-bus-queues/) artikel, Zie [Azure wachtrijen en Service Bus-wachtrijen - vergeleken en Tegenstelling tot](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)