---
title: Azure Service Bus-onderwerpen gebruiken met Python | Microsoft Docs
description: Informatie over het gebruik van Azure Service Bus-onderwerpen en abonnementen van Python.
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 15269f9728e9dc45e6436e53b1859f76d4a7a0c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Service Bus-onderwerpen en abonnementen gebruiken met Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe u Service Bus-onderwerpen en -abonnementen gebruikt. De voorbeelden zijn geschreven in Python en gebruik de [Azure Python SDK-pakket][Azure Python package]. De scenario's worden behandeld: **maken van onderwerpen en abonnementen**, **maken van abonnementsfilters**, **verzenden van berichten naar een onderwerp**, **ontvangen van berichten van een abonnement**, en **verwijderen van onderwerpen en abonnementen**. Zie voor meer informatie over onderwerpen en abonnementen, de [Vervolgstappen](#next-steps) sectie.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Als u nodig hebt om Python te installeren of de [Azure Python-pakket][Azure Python package], Zie de [Python installatiehandleiding](../python-how-to-install.md).

## <a name="create-a-topic"></a>Een onderwerp maken
De **ServiceBusService** object kunt u werken met onderwerpen. Voeg de volgende boven aan elk Python-bestand waarin u wilt programmatisch toegang biedt tot de Service Bus:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

De volgende code maakt een **ServiceBusService** object. Vervang `mynamespace`, `sharedaccesskeyname`, en `sharedaccesskey` met uw werkelijke naamruimte Shared Access Signature (SAS) sleutelwaarde naam en een sleutel.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

U kunt verkrijgen van de waarden voor de naam van de SAS-sleutel en waarde van de [Azure-portal][Azure portal].

```python
bus_service.create_topic('mytopic')
```

De `create_topic` methode biedt ook ondersteuning voor extra opties waarmee u kunt de standaardinstellingen voor onderwerp zoals bericht of grootte van het maximum aantal onderwerp negeren. Het volgende voorbeeld wordt de grootte van het maximum aantal onderwerp 5 GB, en een tijdstip voor live (TTL)-waarde van 1 minuut:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonnementen maken
Abonnementen op onderwerpen ook worden gemaakt met de **ServiceBusService** object. Abonnementen kunnen worden genoemd en een optioneel filter waarmee de verzameling berichten in de virtuele wachtrij van het abonnement wordt beperkt.

> [!NOTE]
> Abonnementen worden permanent en blijft bestaan totdat ze, of het onderwerp waarop ze zijn aangemeld, worden verwijderd.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)
Het **MatchAll**-filter is het standaardfilter dat wordt gebruikt als er bij het maken van een nieuw abonnement geen filter is opgegeven. Wanneer de **MatchAll** filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp in de virtuele wachtrij van het abonnement geplaatst. Het volgende voorbeeld wordt een abonnement genaamd `AllMessages` en gebruikt de standaardinstallatielocatie **MatchAll** filter.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken
Ook kunt u filters waarmee u kunt opgeven welke berichten die worden verzonden naar een onderwerp weergegeven binnen een bepaald onderwerpabonnement.

Het meest flexibele type filter dat wordt ondersteund door abonnementen is een **SqlFilter**, waarmee een subset van SQL92 wordt geïmplementeerd implementeert. SQL-filters worden uitgevoerd voor de eigenschappen van de berichten die worden gepubliceerd naar het onderwerp. Zie de [SqlFilter.SqlExpression][SqlFilter.SqlExpression]-syntaxis voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

U kunt filters toevoegen aan een abonnement met behulp van de **maken\_regel** methode van de **ServiceBusService** object. Deze methode kunt u nieuwe filters toevoegen aan een bestaand abonnement.

> [!NOTE]
> Omdat het standaardfilter automatisch op alle nieuwe abonnementen toegepast wordt, moet u eerst het standaardfilter verwijderen of de **MatchAll** overschrijft alle andere filters die u kunt opgeven. U kunt de standaardregel verwijderen met behulp van de `delete_rule` methode van de **ServiceBusService** object.
> 
> 

Het volgende voorbeeld wordt een abonnement genaamd `HighMessages` met een **SqlFilter** die alleen berichten selecteert die een aangepaste `messagenumber` eigenschap groter is dan 3:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Op deze manier in het volgende voorbeeld wordt een abonnement genaamd `LowMessages` met een **SqlFilter** die alleen berichten selecteert die een `messagenumber` eigenschap kleiner dan of gelijk aan 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Nu wanneer een bericht verzonden naar `mytopic` wordt het altijd bezorgd bij ontvangers die zijn geabonneerd op de **AllMessages** onderwerpabonnement en selectief bezorgd bij ontvangers die zijn geabonneerd op de **HighMessages** en **LowMessages** onderwerpabonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Als u wilt een bericht verzendt naar een Service Bus-onderwerp, de toepassing moet gebruiken de `send_topic_message` methode van de **ServiceBusService** object.

Het volgende voorbeeld toont hoe vijf testberichten naar verzendt `mytopic`. Houd er rekening mee dat de `messagenumber` eigenschapswaarde van elk bericht varieert op de herhaling van de lus (deze bepaalt welke abonnementen ontvangen):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De koptekst, die de standaard- en aangepaste toepassingseigenschappen bevat, kan maximaal 64 kB groot zijn. Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten in een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
Berichten worden ontvangen van een abonnement met de `receive_subscription_message` methode op de **ServiceBusService** object:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Berichten van het abonnement verwijderd als ze worden gelezen wanneer de parameter `peek_lock` is ingesteld op **False**. U kunt lezen (peek) en het bericht zonder het te verwijderen uit de wachtrij door de parameter vergrendelen `peek_lock` naar **True**.

Het gedrag van lezen van en het bericht is verwijderd als onderdeel van de bewerking receive is het eenvoudigste model en werkt het beste voor scenario's waarin een toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt heeft gemarkeerd, klikt u vervolgens wanneer de toepassing opnieuw wordt opgestart en het verbruik van berichten opnieuw begint, ontbreekt het bericht dat voor het vastlopen is verbruikt.

Als de `peek_lock` parameter is ingesteld op **True**, wordt het ontvangen, wordt een bewerking met twee fasen, waardoor er mogelijk worden ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus een aanvraag ontvangt, wordt het volgende te verbruiken bericht gevonden, wordt het bericht vergrendeld om te voorkomen dat andere consumenten het ontvangen en wordt het bericht vervolgens naar de toepassing geretourneerd. Nadat de toepassing klaar is met verwerking van het bericht (of veilig heeft opgeslagen voor toekomstige verwerking), is de tweede fase van het ontvangstproces voltooid door het aanroepen van `delete` methode op de **bericht** object. De `delete` methode markeert het bericht als verbruikt en wordt deze verwijderd uit het abonnement.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Het vastlopen van de toepassing en onleesbare berichten afhandelen
Service Bus biedt functionaliteit om netjes te herstellen bij fouten in uw toepassing of problemen bij het verwerken van een bericht. Als een ontvangende toepassing kan niet worden verwerkt het bericht om een bepaalde reden, dan kan worden aangeroepen de `unlock` methode op de **bericht** object. Dit zorgt ervoor dat Service Bus het bericht in het abonnement ontgrendelt en beschikbaar zijn om opnieuw te ontvangen, ofwel door dezelfde consumerende toepassing of door een andere consumerende toepassing.

Er is ook een time-out gekoppeld aan een bericht in het abonnement is vergrendeld en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus het bericht automatisch ontgrendelt en wordt het beschikbaar worden om opnieuw te ontvangen.

In het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de `delete` methode wordt aangeroepen en vervolgens het bericht opnieuw bezorgd bij de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal verwerken*, dat wil zeggen elk bericht ten minste één keer wordt verwerkt maar in sommige situaties hetzelfde bericht opnieuw kan worden bezorgd. Als in het scenario dubbele verwerking niet wordt getolereerd, dan moeten toepassingsontwikkelaars extra logica toevoegen aan de toepassing om dubbele berichtbezorging af te handelen. Dit wordt vaak bereikt met behulp van de **MessageId** eigenschap van het bericht dat gelijk bij meerdere bezorgingspogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen
Onderwerpen en abonnementen worden permanent en expliciet moet worden verwijderd via de [Azure-portal] [ Azure portal] of via een programma. Het volgende voorbeeld laat zien hoe u het onderwerp met de naam `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Als een onderwerp wordt verwijderd, worden ook alle abonnementen verwijderd die zijn geregistreerd bij het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code laat zien hoe u een abonnement met de naam `HighMessages` van de `mytopic` onderwerp:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Service Bus-onderwerpen hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie [wachtrijen, onderwerpen en abonnementen][Queues, topics, and subscriptions].
* Referentiemateriaal [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
