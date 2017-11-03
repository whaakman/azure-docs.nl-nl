---
title: Service Bus-wachtrijen gebruiken met PHP | Microsoft Docs
description: Informatie over het gebruiken van Service Bus-wachtrijen in Azure Codevoorbeelden geschreven in PHP.
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3514812f7f087582035dad5d9a4d620652aa4da9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-php"></a>Service Bus-wachtrijen gebruiken met PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe u Service Bus-wachtrijen gebruiken. De voorbeelden zijn geschreven in PHP en gebruik de [Azure SDK voor PHP](../php-download-sdk.md). De scenario's worden behandeld: **maken van wachtrijen**, **verzenden en ontvangen berichten**, en **verwijderen van wachtrijen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken
De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de Azure Blob-service is de verwijzende klassen in de [Azure SDK voor PHP](../php-download-sdk.md) vanuit uw code. Eventuele ontwikkelingsprogramma's kunt u uw toepassing of een Kladblok maken.

> [!NOTE]
> Uw PHP-installatie moet ook beschikken over de [OpenSSL-extensie](http://php.net/openssl) geïnstalleerd en ingeschakeld.
> 
> 

In deze handleiding gebruikt u de functies van de service die kunnen worden aangeroepen vanuit binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een Azure-Webrol, werkrol of website.

## <a name="get-the-azure-client-libraries"></a>De Azure-client bibliotheken ophalen
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Voor het gebruik van de Service Bus-wachtrij API's, het volgende doen:

1. Verwijst naar de autoloader-bestand met de [require_once] [ require_once] instructie.
2. Verwijst naar alle klassen die u kunt gebruiken.

Het volgende voorbeeld laat zien hoe de autoloader-bestand en de verwijzing naar de `ServicesBuilder` klasse.

> [!NOTE]
> In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de PHP-clientbibliotheken voor Azure via Composer hebt geïnstalleerd. Als u de tapewisselaars handmatig of als een PEER-pakket geïnstalleerd, moet u verwijzen naar de **WindowsAzure.php** autoloader-bestand.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de onderstaande voorbeelden de `require_once` instructie worden altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen
Voor het concretiseren van een Service Bus-client, moet u eerst een geldige verbindingsreeks in deze indeling hebben:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Waar `Endpoint` is meestal de indeling `[yourNamespace].servicebus.windows.net`.

Maken van een Azure-service-client moet u de `ServicesBuilder` klasse. U kunt:

* De verbindingsreeks rechtstreeks aan deze doorgeven.
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
  * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen
  * U kunt nieuwe bronnen toevoegen door het uitbreiden van de `ConnectionStringSource` klasse

Voor de voorbeelden die hier wordt beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Een wachtrij maken
U kunt beheerbewerkingen uitvoeren voor Service Bus-wachtrijen via de `ServiceBusRestProxy` klasse. Een `ServiceBusRestProxy` -object is gemaakt de `ServicesBuilder::createServiceBusService` fabrieksmethode met een geschikte verbindingsreeks die de token machtigingen voor beheer ingekapseld.

Het volgende voorbeeld laat zien hoe het instantiëren van een `ServiceBusRestProxy` en roep `ServiceBusRestProxy->createQueue` voor het maken van een wachtrij met de naam `myqueue` binnen een `MySBNamespace` Servicenaamruimte:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> U kunt de `listQueues` methode op `ServiceBusRestProxy` objecten om te controleren of een wachtrij met een opgegeven naam al in een naamruimte bestaat.
> 
> 

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht te verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `ServiceBusRestProxy->sendQueueMessage` methode. De volgende code toont hoe u een bericht verzendt de `myqueue` wachtrij eerder hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Berichten verzonden naar (en ontvangen van) Service Bus-wachtrijen, exemplaren van zijn de [BrokeredMessage] [ BrokeredMessage] klasse. [BrokeredMessage] [ BrokeredMessage] objecten hebben een aantal standaardmethoden en eigenschappen die worden gebruikt om aangepaste toepassingsspecifieke eigenschappen en een hoofdtekst met willekeurige toepassingsgegevens.

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. Deze bovengrens voor de grootte van de wachtrij is 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen uit een wachtrij

De beste manier om berichten te ontvangen van een wachtrij is met een `ServiceBusRestProxy->receiveQueueMessage` methode. Berichten kunnen worden ontvangen in twee verschillende modi: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) en [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** is de standaardmodus.

Wanneer u [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) -modus ontvangt een één bewerking uitgevoerd; dat wil zeggen als Service Bus een leesaanvraag voor een bericht in een wachtrij ontvangt, wordt het bericht als verbruikt gemarkeerd en naar de toepassing wordt geretourneerd. De [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete)-modus is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing het niet verwerken van een bericht bij een fout kan tolereren. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In de standaard [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) -modus ontvangt een bericht wordt een bewerking met twee fasen, waardoor er mogelijk worden ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Wanneer Service Bus een aanvraag ontvangt, het zoeken naar het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en vervolgens terugkeert naar de toepassing. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het ontvangen bericht aan `ServiceBusRestProxy->deleteMessage`. Wanneer Service Bus de aanroep de `deleteMessage` aanroep, wordt het bericht als verbruikt markeren en verwijderen uit de wachtrij.

Het volgende voorbeeld laat zien hoe ontvangen en verwerken van een bericht met [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) modus (dit is de standaardmodus).

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen

Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de `unlockMessage` methode voor het ontvangen bericht (in plaats van de `deleteMessage` methode). Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en beschikbaar zijn om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `deleteMessage` verzoek is uitgegeven, en vervolgens het bericht opnieuw bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal* verwerking; dat wil zeggen elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als het scenario kan geen dubbele verwerking tolereren, wordt vervolgens extra logica toe te voegen op toepassingen op de levering van dubbele berichten verwerken aanbevolen. Dit wordt vaak bereikt met behulp van de `getMessageId` methode van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-wachtrijen hebt geleerd, gaat u naar [wachtrijen, onderwerpen en abonnementen] [ Queues, topics, and subscriptions] voor meer informatie.

Ga voor meer informatie, ook naar de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


