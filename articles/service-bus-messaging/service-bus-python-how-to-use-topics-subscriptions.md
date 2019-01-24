---
title: Azure Service Bus-onderwerpen gebruiken met Python | Microsoft Docs
description: Informatie over het gebruik van Azure Service Bus-onderwerpen en abonnementen van Python.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/20/2018
ms.author: aschhab
ms.openlocfilehash: 280901c4b7af9cc65163ebc453a54fb7ec5d5111
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54849393"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Service Bus-onderwerpen en abonnementen gebruiken met Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe u Service Bus-onderwerpen en -abonnementen gebruikt. De voorbeelden zijn geschreven in Python en gebruik de [Azure Python SDK-pakket][Azure Python package]. De behandelde scenario's zijn **het maken van onderwerpen en abonnementen**, **het maken van abonnementsfilters**, **verzenden van berichten naar een onderwerp**, **ontvangen berichten van een abonnement**, en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Als u nodig hebt om Python te installeren of de [Azure Python-pakket][Azure Python package], Zie de [Python-installatiehandleiding](../python-how-to-install.md).

## <a name="create-a-topic"></a>Een onderwerp maken

De **ServiceBusService** object kunt u werken met onderwerpen. Voeg de volgende code aan de bovenkant van een Python-bestand waarin u wilt programmatisch toegang verkrijgen tot de Service Bus:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

De volgende code maakt een **ServiceBusService** object. Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw werkelijke naamruimte Shared Access Signature (SAS) sleutelwaarde naam en sleutel.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

U kunt verkrijgen van de waarden voor de naam van de SAS-sleutel en waarde uit de [Azure-portal][Azure portal].

```python
bus_service.create_topic('mytopic')
```

De `create_topic` methode biedt ook ondersteuning voor extra opties waarmee u kunt de standaardinstellingen van onderwerp zoals time to live van bericht of grootte van het maximum aantal onderwerp negeren. Het volgende voorbeeld wordt de grootte van het maximum aantal onderwerp ingesteld op 5 GB en een tijd naar live (TTL) waarde van één minuut:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Maken van abonnementen

Abonnementen op onderwerpen ook worden gemaakt met de **ServiceBusService** object. Abonnementen zijn met de naam en een optioneel filter dat u Hiermee beperkt u de verzameling berichten die worden geleverd aan de virtuele wachtrij van het abonnement kunnen hebben.

> [!NOTE]
> Abonnementen zijn permanent en blijft bestaan totdat ze, of het onderwerp waarnaar ze bent geabonneerd, worden verwijderd.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Als geen filter is opgegeven wanneer een nieuw abonnement wordt gemaakt, de **MatchAll** filter (standaard) wordt gebruikt. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld wordt een abonnement genaamd `AllMessages` en gebruikmaakt van de **MatchAll** filter.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

Ook kunt u filters waarmee u kunt opgeven welke berichten die worden verzonden naar een onderwerp binnen een bepaald onderwerpabonnement weergegeven.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is een **SqlFilter**, die wordt geïmplementeerd met een subset van SQL92 wordt geïmplementeerd. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie de [SqlFilter.SqlExpression][SqlFilter.SqlExpression]-syntaxis voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_regel** -methode van de **ServiceBusService** object. Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [!NOTE]
> Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft alle andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de `delete_rule` -methode van de **ServiceBusService** object.
> 
> 

Het volgende voorbeeld wordt een abonnement genaamd `HighMessages` met een **SqlFilter** dat alleen berichten selecteert die een aangepaste `messagenumber` eigenschap groter dan 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `LowMessages` met een **SqlFilter** dat alleen berichten selecteert die een `messagenumber` eigenschap minder dan of gelijk aan 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nu, wanneer een bericht is verzonden naar `mytopic` wordt het altijd bezorgd bij ontvangers die zijn geabonneerd op de **AllMessages** onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op de **HighMessages**en **LowMessages** onderwerpabonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Voor het verzenden van een bericht naar een Service Bus-onderwerp, uw toepassing moet gebruikmaken van de `send_topic_message` -methode van de **ServiceBusService** object.

Het volgende voorbeeld ziet u hoe u voor het verzenden van vijf testberichten naar `mytopic`. De `messagenumber` eigenschapswaarde van elk bericht varieert op de herhaling van de lus (deze bepaalt welke abonnementen ontvangen):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement

Berichten worden ontvangen van een abonnement met de `receive_subscription_message` methode voor het **ServiceBusService** object:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Berichten van het abonnement worden verwijderd als ze worden gelezen wanneer de parameter `peek_lock` is ingesteld op **False**. U kunt lezen (peek) en een vergrendeling van het bericht zonder deze te verwijderen uit de wachtrij door de parameter `peek_lock` naar **waar**.

Het gedrag van lezen en verwijderen van het bericht als onderdeel van de ontvangstbewerking, is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing niet verwerken van een bericht tolereren kan wanneer er een fout. Voor meer informatie over dit probleem, u hebt een scenario waarin de consument problemen met de aanvraag ontvangen en vervolgens vastloopt voordat deze wordt verwerkt. Omdat Service Bus kan het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens ontbreekt wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint het het bericht dat voor het vastlopen is verbruikt.

Als de `peek_lock` parameter is ingesteld op **waar**, wordt de ontvangst een bewerking met twee fasen, waardoor er toepassingen kunnen worden ondersteund die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met het verwerken van bericht (of deze op betrouwbare wijze voor toekomstige verwerking slaat), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `delete` methode voor het **bericht** object. De `delete` methode wordt het bericht als verbruikt gemarkeerd en wordt deze verwijderd uit het abonnement.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen

Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan het bericht te verwerken voor een of andere reden niet, dan kan worden aangeroepen de `unlock` methode voor het **bericht** object. Deze methode zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en het beschikbaar worden om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht dat in het abonnement is vergrendeld, en als de toepassing niet kan het bericht te verwerken voordat u de vergrendeling time-out is verlopen (bijvoorbeeld, als de toepassing vastloopt), wordt de Service Bus het bericht automatisch ontgrendelen en maakt deze beschikbaar zijn om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `delete` methode wordt aangeroepen, wordt het bericht zal opnieuw worden bezorgd bij de toepassing opnieuw wordt gestart. Dit gedrag wordt vaak genoemd. Ten minste eenmaal verwerken *; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt, maar in bepaalde situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Om dit te doen, kunt u de **MessageId** eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Onderwerpen en abonnementen persistent zijn en moeten expliciet worden verwijderd via de [Azure-portal] [ Azure portal] of via een programma. Het volgende voorbeeld ziet u het verwijderen van het onderwerp met de naam `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe u een abonnement genaamd verwijderen `HighMessages` uit de `mytopic` onderwerp:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Volgende stappen

Nu dat u de basisprincipes van Service Bus-onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie [wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions].
* Verwijzing voor [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
