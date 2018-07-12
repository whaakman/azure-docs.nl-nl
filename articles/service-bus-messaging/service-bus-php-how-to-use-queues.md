---
title: Over het gebruik van Service Bus-wachtrijen met PHP | Microsoft Docs
description: Informatie over het gebruiken van Service Bus-wachtrijen in Azure Voorbeelden van code geschreven in PHP.
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3514812f7f087582035dad5d9a4d620652aa4da9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38531619"
---
# <a name="how-to-use-service-bus-queues-with-php"></a>Over het gebruik van Service Bus-wachtrijen met PHP
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Deze handleiding wordt beschreven hoe u Service Bus-wachtrijen. De voorbeelden zijn geschreven in PHP en gebruik de [Azure SDK voor PHP](../php-download-sdk.md). De behandelde scenario's zijn **maken van wachtrijen**, **verzenden en ontvangen van berichten**, en **verwijderen van wachtrijen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken
De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de Azure Blob-service is de verwijzing naar klassen in de [Azure SDK voor PHP](../php-download-sdk.md) uit vanuit uw code. Alle hulpprogramma's voor ontwikkeling kunt u maken van uw toepassing of Kladblok.

> [!NOTE]
> Uw PHP-installatie moet ook beschikken over de [OpenSSL-extensie](http://php.net/openssl) geïnstalleerd en ingeschakeld.
> 
> 

In deze handleiding gebruikt u servicefuncties die kunnen worden opgeroepen binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een Azure-Webrol, werkrol of website.

## <a name="get-the-azure-client-libraries"></a>De Azure-client-bibliotheken ophalen
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
Voor het gebruik van de API's van de Service Bus-wachtrij, het volgende doen:

1. Verwijzen naar de autoloader bestand met de [require_once] [ require_once] instructie.
2. Verwijzen naar alle klassen die u kunt gebruiken.

Het volgende voorbeeld laat zien hoe om op te nemen de autoloader bestands- en naslaginformatie over de `ServicesBuilder` klasse.

> [!NOTE]
> In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de PHP-clientbibliotheken voor Azure via Composer hebt geïnstalleerd. Als u de bibliotheken handmatig of als een PEER-pakket hebt geïnstalleerd, moet u verwijzen naar de **WindowsAzure.php** autoloader-bestand.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de onderstaande voorbeelden de `require_once` instructie worden altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren wordt verwezen.

## <a name="set-up-a-service-bus-connection"></a>Instellen van een Service Bus-verbinding
Als u wilt maken van een Service Bus-clienttoepassing, moet u eerst een geldige verbindingsreeks in deze indeling hebben:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Waar `Endpoint` is meestal de notatie `[yourNamespace].servicebus.windows.net`.

Het maken van een Azure-service-client moet u de `ServicesBuilder` klasse. U kunt:

* Hiermee geeft u de verbindingsreeks rechtstreeks aan.
* Gebruik de **CloudConfigurationManager (CCM) van de** om te controleren op meerdere externe bronnen voor de verbindingsreeks:
  * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen
  * U kunt nieuwe bronnen toevoegen door uit te breiden de `ConnectionStringSource` klasse

In de voorbeelden die hier worden beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Een wachtrij maken
U kunt beheerbewerkingen voor Service Bus-wachtrijen via de `ServiceBusRestProxy` klasse. Een `ServiceBusRestProxy` object is gemaakt de `ServicesBuilder::createServiceBusService` factory methode met een juiste verbindingsreeks die kapselt de token machtigingen om dit te beheren.

Het volgende voorbeeld laat zien hoe exemplaar maken van een `ServiceBusRestProxy` en roep `ServiceBusRestProxy->createQueue` te maken van een wachtrij met de naam `myqueue` binnen een `MySBNamespace` Servicenaamruimte:

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
> U kunt de `listQueues` methode voor `ServiceBusRestProxy` objecten om te controleren of een wachtrij met een opgegeven naam al in een naamruimte bestaat.
> 
> 

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Een bericht verzenden naar een Service Bus-wachtrij, het aanroepen van uw toepassing de `ServiceBusRestProxy->sendQueueMessage` methode. De volgende code laat zien hoe u een bericht naar de `myqueue` wachtrij eerder hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

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

Berichten verzonden naar (en ontvangen van) Service Bus-wachtrijen, exemplaren van zijn de [BrokeredMessage] [ BrokeredMessage] klasse. [BrokeredMessage] [ BrokeredMessage] objecten hebben een aantal standaard-methoden en eigenschappen die worden gebruikt voor het opslaan van aangepaste toepassingsspecifieke eigenschappen en een hoofdtekst met willekeurige toepassingsgegevens.

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten in een wachtrij. Deze bovengrens voor de grootte van de wachtrij is 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij

De beste manier om berichten te ontvangen van een wachtrij is met een `ServiceBusRestProxy->receiveQueueMessage` methode. Kunnen u berichten ontvangen in twee verschillende modi: [ *ReceiveAndDelete* ](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) en [ *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock). **PeekLock** is de standaardmodus.

Bij het gebruik van [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete) modus, ontvangen is een eenmalige bewerking uitgevoerd; dat wil zeggen, Service Bus een leesaanvraag voor een bericht in een wachtrij ontvangt, wordt het bericht als verbruikt gemarkeerd en geeft dit terug aan de toepassing. De [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode.receiveanddelete)-modus is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing het niet verwerken van een bericht bij een fout kan tolereren. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In de standaard [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) modus, ontvangen een bericht wordt een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Wanneer Service Bus een aanvraag ontvangt, deze vindt het volgende bericht om te worden verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en vervolgens terugkeert naar de toepassing. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door door te geven van het ontvangen bericht `ServiceBusRestProxy->deleteMessage`. Wanneer Service Bus ziet de `deleteMessage` aanroep, wordt het bericht als verbruikt markeren en verwijderen uit de wachtrij.

Het volgende voorbeeld laat zien hoe ontvangen en verwerken van een bericht met [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode.peeklock) -modus (de standaardinstelling).

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

Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de `unlockMessage` methode voor het ontvangen bericht (in plaats van de `deleteMessage` methode). Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het toegankelijk maken voor het opnieuw worden ontvangen door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling is verlopen (bijvoorbeeld, als de toepassing vastloopt), Service Bus wordt het bericht automatisch ontgrendelen en maken beschikbaar voor het opnieuw worden ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `deleteMessage` aanvraag is uitgegeven, wordt het bericht zal opnieuw worden bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal* bewerking; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als het scenario kan geen dubbele verwerking tolereren, wordt klikt u vervolgens aanvullende logica toevoegen aan toepassingen voor het afhandelen van dubbele berichtbezorging aanbevolen. Dit wordt vaak bereikt met behulp van de `getMessageId` -methode van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Service Bus-wachtrijen hebt geleerd, Zie [wachtrijen, onderwerpen en abonnementen] [ Queues, topics, and subscriptions] voor meer informatie.

Ga voor meer informatie, ook naar de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once


