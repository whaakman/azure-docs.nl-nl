---
title: Service Bus-onderwerpen gebruiken met PHP | Microsoft Docs
description: Informatie over Service Bus-onderwerpen gebruiken met PHP in Azure.
services: service-bus-messaging
documentationcenter: php
author: sethmanheim
manager: timlt
editor: 
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/06/2017
ms.author: sethm
ms.openlocfilehash: 4763b172375668213372e6f4f8fc87431c430e53
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-php"></a>Service Bus-onderwerpen en abonnementen gebruiken met PHP

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dit artikel laat zien hoe u Service Bus-onderwerpen en abonnementen. De voorbeelden zijn geschreven in PHP en gebruik de [Azure SDK voor PHP](../php-download-sdk.md). De scenario's worden behandeld: **maken van onderwerpen en abonnementen**, **maken van abonnementsfilters**, **verzenden van berichten naar een onderwerp**, **ontvangen van berichten van een abonnement**, en **verwijderen van onderwerpen en abonnementen**.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Een PHP-toepassing maken
De enige vereiste voor het maken van een PHP-toepassing die toegang heeft tot de Azure Blob-service is om te verwijzen naar klassen in de [Azure SDK voor PHP](../php-download-sdk.md) vanuit uw code. Eventuele ontwikkelingsprogramma's kunt u uw toepassing of een Kladblok maken.

> [!NOTE]
> Uw PHP-installatie moet ook beschikken over de [OpenSSL-extensie](http://php.net/openssl) geïnstalleerd en ingeschakeld.
> 
> 

Dit artikel wordt beschreven hoe u servicefuncties die kunnen worden aangeroepen binnen een PHP-toepassing lokaal of in de code die wordt uitgevoerd binnen een Azure-Webrol, werkrol of website.

## <a name="get-the-azure-client-libraries"></a>De Azure-client bibliotheken ophalen
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor het gebruik van Service Bus
De Service Bus-API's gebruiken:

1. Verwijst naar de autoloader-bestand met de [require_once] [ require-once] instructie.
2. Verwijst naar alle klassen die u kunt gebruiken.

Het volgende voorbeeld laat zien hoe de autoloader-bestand en de verwijzing naar de **ServiceBusService** klasse.

> [!NOTE]
> In dit voorbeeld (en andere voorbeelden in dit artikel) wordt ervan uitgegaan dat u de PHP-clientbibliotheken voor Azure via Composer hebt geïnstalleerd. Als u de tapewisselaars handmatig of als een PEER-pakket geïnstalleerd, moet u verwijzen naar de **WindowsAzure.php** autoloader-bestand.
> 
> 

```php
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

In de volgende voorbeelden de `require_once` instructie wordt altijd weergegeven, maar alleen de klassen die nodig zijn voor het voorbeeld uit te voeren naar worden verwezen.

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen
Voor het concretiseren van een Service Bus-clienttoepassing moet u eerst een geldige verbindingsreeks in de volgende notatie hebben:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Waar `Endpoint` is meestal de indeling `https://[yourNamespace].servicebus.windows.net`.

Maken van een Azure-service-client moet u de `ServicesBuilder` klasse. U kunt:

* De verbindingsreeks rechtstreeks aan deze doorgeven.
* Gebruik de **CloudConfigurationManager (CCM)** om te controleren van meerdere externe bronnen voor de verbindingsreeks:
  * Standaard wordt geleverd met ondersteuning voor een externe bron - omgevingsvariabelen.
  * U kunt nieuwe bronnen toevoegen door het uitbreiden van de `ConnectionStringSource` klasse.

Voor de voorbeelden die hier wordt beschreven, wordt de verbindingsreeks rechtstreeks doorgegeven.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Een onderwerp maken
U kunt beheerbewerkingen uitvoeren voor Service Bus-onderwerpen via de `ServiceBusRestProxy` klasse. Een `ServiceBusRestProxy` -object is gemaakt de `ServicesBuilder::createServiceBusService` fabrieksmethode met een geschikte verbindingsreeks die de token machtigingen voor beheer ingekapseld.

Het volgende voorbeeld laat zien hoe het instantiëren van een `ServiceBusRestProxy` en roep `ServiceBusRestProxy->createTopic` voor het maken van een onderwerp met de naam `mytopic` binnen een `MySBNamespace` naamruimte:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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
> U kunt de `listTopics` methode op `ServiceBusRestProxy` objecten om te controleren of een onderwerp met een opgegeven naam al in een Servicenaamruimte bestaat.
> 
> 

## <a name="create-a-subscription"></a>Een abonnement maken
Ook onderwerpabonnementen worden gemaakt met de `ServiceBusRestProxy->createSubscription` methode. Abonnementen hebben een naam en kunnen een optioneel filter hebben waarmee de verzameling berichten wordt beperkt die aan de virtuele wachtrij van het abonnement wordt doorgegeven.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Het **MatchAll**-filter is het standaardfilter dat wordt gebruikt als er bij het maken van een nieuw abonnement geen filter is opgegeven. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld wordt een abonnement genaamd 'mysubscription' en gebruikt de standaardinstallatielocatie **MatchAll** filter.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
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

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
U kunt ook filters instellen waarmee u kunt opgeven welke berichten die naar een onderwerp worden verzonden, moeten worden weergegeven in een bepaald onderwerpabonnement. Het meest flexibele type filter dat wordt ondersteund door abonnementen is de [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter), waarmee een subset van SQL92 wordt geïmplementeerd implementeert. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie voor meer informatie over SqlFilters [SqlFilter.SqlExpression eigenschap][sqlfilter].

> [!NOTE]
> Elke regel voor een abonnement binnenkomende berichten verwerkt onafhankelijk, hun Resultaatberichten toe te voegen aan het abonnement. Bovendien elk nieuw abonnement heeft een standaardwaarde **regel** object met een filter dat hiermee alle berichten van het onderwerp toegevoegd aan het abonnement. Als u wilt alleen berichten die overeenkomen met uw filter ontvangt, moet u de standaardregel verwijderen. U kunt de standaardregel verwijderen met behulp van de `ServiceBusRestProxy->deleteRule` methode.
> 
> 

Het volgende voorbeeld wordt een abonnement genaamd `HighMessages` met een **SqlFilter** die alleen berichten selecteert die een aangepaste `MessageNumber` eigenschap groter is dan 3. Zie [berichten verzenden naar een onderwerp](#send-messages-to-a-topic) voor informatie over het toevoegen van aangepaste eigenschappen aan berichten.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Opmerking dat deze code het gebruik van een extra naamruimte vereist: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `LowMessages` met een `SqlFilter` die alleen berichten selecteert die een `MessageNumber` eigenschap kleiner dan of gelijk aan 3.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nu wanneer een bericht verzonden naar de `mytopic` onderwerp, wordt altijd bezorgd bij ontvangers die zijn geabonneerd op de `mysubscription` abonnement, en selectief bezorgd bij ontvangers die zijn geabonneerd op de `HighMessages` en `LowMessages` abonnementen (afhankelijk van bij de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Een bericht te verzenden naar een Service Bus-onderwerp, de aanroepen van uw toepassing de `ServiceBusRestProxy->sendTopicMessage` methode. De volgende code toont hoe u een bericht verzendt de `mytopic` onderwerp eerder hebt gemaakt in de `MySBNamespace` Servicenaamruimte.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
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

Berichten die worden verzonden naar Service Bus-onderwerpen, zijn exemplaren van de [BrokeredMessage] [ BrokeredMessage] klasse. [BrokeredMessage] [ BrokeredMessage] objecten hebben een aantal standaardeigenschappen en -methoden, evenals eigenschappen die kunnen worden gebruikt om aangepaste toepassingsspecifieke eigenschappen te bewaren. Het volgende voorbeeld laat zien hoe 5 testbericht verzenden naar de `mytopic` onderwerp eerder hebt gemaakt. De `setProperty` methode wordt gebruikt om een aangepaste eigenschap toevoegen (`MessageNumber`) aan elk bericht. Houd er rekening mee dat de `MessageNumber` waarde van de eigenschap varieert voor elk bericht (u kunt deze waarde gebruiken om te bepalen welke abonnementen ontvangen, zoals wordt weergegeven in de [een abonnement maken](#create-a-subscription) sectie):

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. Deze bovengrens voor de grootte van het onderwerp is 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
De beste manier om berichten te ontvangen van een abonnement is met een `ServiceBusRestProxy->receiveSubscriptionMessage` methode. Berichten kunnen worden ontvangen in twee verschillende modi: [ *ReceiveAndDelete* en *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** is de standaardmodus.

Wanneer u de [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode)-modus gebruikt, wordt het ontvangen in één bewerking uitgevoerd; dat wil zeggen als Service Bus een leesaanvraag voor een bericht in een abonnement ontvangt, wordt voor het bericht aangegeven dat het is verbruikt en wordt het bericht naar de toepassing geretourneerd. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * modus is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In de standaard [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) -modus ontvangt een bericht wordt een bewerking met twee fasen, waardoor er mogelijk worden ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het ontvangen bericht aan `ServiceBusRestProxy->deleteMessage`. Wanneer Service Bus de aanroep de `deleteMessage` waarneemt, wordt het bericht als verbruikt gemarkeerd en uit de wachtrij worden verwijderd.

Het volgende voorbeeld laat zien hoe ontvangen en verwerken van een bericht met [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modus (dit is de standaardmodus). 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe: vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de `unlockMessage` methode voor het ontvangen bericht (in plaats van de `deleteMessage` methode). Dit zorgt ervoor dat Service Bus het bericht in de wachtrij ontgrendelt en het beschikbaar maakt om opnieuw te worden ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Daarnaast is er een time-out gekoppeld aan een bericht in de wachtrij is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en het maken beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `deleteMessage` verzoek is uitgegeven, en vervolgens het bericht is opnieuw bij de toepassing bezorgd wanneer opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal* verwerking; dat wil zeggen elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als het scenario kan geen dubbele verwerking tolereren, kunnen ontwikkelaars van toepassingen moeten extra logica toevoegen aan de toepassingen voor de levering van dubbele berichten verwerken. Dit wordt vaak bereikt met behulp van de `getMessageId` methode van het bericht dat gelijk blijft bij meerdere bezorgingspogingen.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Verwijder een onderwerp of een abonnement, gebruiken de `ServiceBusRestProxy->deleteTopic` of de `ServiceBusRestProxy->deleteSubscripton` methoden, respectievelijk. Houd er rekening mee dat als een onderwerp verwijdert tevens alle abonnementen die zijn geregistreerd bij het onderwerp.

Het volgende voorbeeld ziet u hoe u verwijdert een onderwerp met de naam `mytopic` en de geregistreerde abonnementen.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {        
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
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

Met behulp van de `deleteSubscription` methode, kunt u een abonnement onafhankelijk verwijderen:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-wachtrijen hebt geleerd, gaat u naar [wachtrijen, onderwerpen en abonnementen] [ Queues, topics, and subscriptions] voor meer informatie.

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter
[require-once]: http://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
