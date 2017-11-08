---
title: Overzicht van Azure Service Bus-wachtrijen, onderwerpen en abonnementen messaging | Microsoft Docs
description: Overzicht van Service Bus-berichtentiteiten.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: 5bea3b56cea81362b25e696a672bf2a00e26d3ef
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus-wachtrijen, -onderwerpen en -abonnementen

Microsoft Azure Service Bus ondersteunt een set van cloud-gebaseerde, bericht georiënteerd middleware-technologieën waaronder betrouwbare message Queuing- en duurzame publiceren/abonneren messaging. Deze 'brokered' messaging-mogelijkheden worden van beschouwd als losgekoppeld van de functies voor berichten die ondersteuning voor publiceren / abonneren, tijdelijke ontkoppeling en scenario's met behulp van de werkbelasting van Service Bus-berichten voor taakverdeling. Ontkoppelde communicatie heeft veel voordelen; clients en servers kunnen bijvoorbeeld naar behoefte verbinding maken en hun bewerkingen asynchroon uitvoeren.

De messaging-entiteiten die de kern van de mogelijkheden voor messaging in Service Bus vormen zijn wachtrijen, onderwerpen en abonnementen en regels/acties.

## <a name="queues"></a>Wachtrijen

Wachtrijen bieden *First In, First Out* (FIFO-principe)-berichtbezorging naar een of meer concurrerende consumenten. Dat wil zeggen berichten doorgaans naar verwachting worden ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht wordt ontvangen en verwerkt door slechts één berichtconsument. Er is een belangrijk voordeel van het gebruik van wachtrijen 'tijdelijke ontkoppeling' bereiken van toepassingsonderdelen. Met andere woorden, hoeft de producenten (afzenders) en consumenten (ontvangers) niet te verzenden en ontvangen van berichten op hetzelfde moment omdat berichten blijvend worden opgeslagen in de wachtrij. De producent heeft bovendien geen moet worden gewacht op een reactie van de gebruiker om door te gaan om te verwerken en verzenden van berichten.

Een bijkomend voordeel is 'herverdeling van taken,' waardoor producenten en consumenten berichten met verschillende snelheden verzenden en ontvangen. In veel toepassingen varieert de de systeembelasting gedurende een bepaalde periode; Er is echter de benodigde verwerkingstijd voor elke werkeenheid doorgaans constant blijft. Tussen bericht producenten en consumenten met een wachtrij betekent dat de betreffende toepassing alleen moeten worden ingericht om te kunnen afhandelen gemiddelde belasting in plaats van piekbelasting. De lengte van de wachtrij neemt toe of af, al naargelang het binnenkomende verkeer. Dit bespaart rechtstreeks geld met betrekking tot de hoeveelheid infrastructuur die nodig zijn voor het laden van toepassingen. Als de belasting toeneemt, kunnen meer werkprocessen worden toegevoegd om te lezen uit de wachtrij. Elk bericht worden door slechts één van de werkprocessen verwerkt. Bovendien kunt deze pull-gebaseerde taakverdeling u optimaal gebruik van de werknemer computers zelfs als de computers van de werknemer met betrekking tot verwerkingskracht verschillen, zoals ze berichten op eigen maximale snelheid ophalen. Dit patroon wordt vaak aangeduid als het patroon 'concurrerende consumenten'.

Met behulp van wachtrijen naar de tussenliggende tussen bericht producenten en consumenten biedt een inherente losse koppeling tussen de onderdelen. Omdat producenten en consumenten niet op de hoogte van elkaar zijn, kan een gebruiker zonder gevolgen voor de producent worden bijgewerkt.

Maken van een wachtrij is een proces met meerdere stappen. U beheerbewerkingen voor Service Bus-berichtentiteiten (wachtrijen en onderwerpen) uitvoeren de [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) klasse, die is samengesteld door het opgeven van het basisadres van de Service Bus-naamruimte en de referenties van de gebruiker. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) biedt methoden voor het maken, opsommen en verwijderen van de messaging-entiteiten. Na het maken van een [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) object van de SAS-naam en sleutel en een naamruimte service management-object, kunt u de [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_)methode voor het maken van de wachtrij. Bijvoorbeeld:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

U kunt vervolgens een object in de wachtrij en een messagingfactory maken met de Service Bus-URI als argument. Bijvoorbeeld:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Vervolgens kunt u berichten verzenden naar de wachtrij. Bijvoorbeeld, als u een lijst met brokered berichten aangeroepen hebt `MessageList`, de code op het volgende voorbeeld lijkt:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

U ontvangt vervolgens berichten uit de wachtrij als volgt:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

In de [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus kan de bewerking receive is één; dat wil zeggen als Service Bus de aanvraag ontvangt, wordt het bericht als verbruikt gemarkeerd en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** modus is het eenvoudigste model en werkt het beste voor scenario's waarin de toepassing kan tolereren niet verwerken van een bericht bij een storing. Neem bijvoorbeeld een scenario waarin de consument de ontvangstaanvraag uitgeeft en het systeem vervolgens vastloopt voordat de aanvraag wordt verwerkt. Omdat Service Bus het bericht als verbruikt, wanneer de toepassing opnieuw wordt opgestart en verbruik van berichten opnieuw begint gemarkeerd, ontbreekt het bericht dat voor het vastlopen is verbruikt.

In [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modus, de ontvangstbewerking verandert in twee fasen, waardoor het mogelijk voor de ondersteuning voor toepassingen die geen ontbrekende berichten kunnen tolereren. Als Service Bus de aanvraag ontvangt, het zoeken naar het volgende bericht wordt verbruikt, wordt vergrendeld om te voorkomen dat andere consumenten het ontvangen en vervolgens terugkeert naar de toepassing. Nadat de toepassing klaar is met de verwerking van het bericht (of het bericht veilig heeft opgeslagen voor toekomstige verwerking), wordt de tweede fase van het ontvangstproces voltooid door [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) aan te roepen voor het ontvangen bericht. Wanneer Service Bus de aanroep de **Complete** aanroep, markeert deze het bericht als verbruikt.

Als de toepassing de kan bericht niet verwerken voor een bepaalde reden wordt, kan worden aangeroepen de [afbreken](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) methode voor het ontvangen bericht (in plaats van [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Hierdoor kunnen Service Bus het bericht ontgrendelt en moet opnieuw worden ontvangen door de dezelfde gebruiker of door een andere concurrerend consumenten beschikbaar maken. Ten tweede, er is een time-out gekoppeld aan de vergrendeling en als de toepassing niet kan verwerken van het bericht voordat de time-out van de vergrendeling verloopt (bijvoorbeeld als de toepassing vastloopt), en vervolgens de Service Bus het bericht ontgrendelt en wordt het beschikbaar zijn voor ontvangen opnieuw) in wezen uitvoeren van een [afbreken](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) bewerking standaard).

Houd er rekening mee dat in het geval dat de toepassing is vastgelopen na het verwerken van het bericht, maar voordat de **Complete** verzoek is uitgegeven, wordt het bericht is opnieuw bij de toepassing bezorgd wanneer opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal* verwerking; dat wil zeggen, elk bericht ten minste één keer wordt verwerkt. In bepaalde situaties kan hetzelfde bericht worden geleverd. Als het scenario kan geen dubbele verwerken tolereren en vervolgens extra logica is vereist in de toepassing voor het detecteren van duplicaten, die kan worden bereikt op basis van de **MessageId** eigenschap van het bericht dat via constant is gebleven bezorgingspogingen. Dit staat bekend als *exact één keer* verwerken.

## <a name="topics-and-subscriptions"></a>Onderwerpen en abonnementen
In tegenstelling tot wachtrijen, waarin elk bericht wordt verwerkt door een enkele gebruiker *onderwerpen* en *abonnementen* biedt een een-op-veel-vorm van communicatie, in een *publiceren/abonneren* patroon. Kan handig zijn voor het schalen van zeer groot aantal ontvangers, elk gepubliceerde bericht beschikbaar wordt gesteld aan elk abonnement dat is geregistreerd bij het onderwerp. Berichten worden verzonden naar een onderwerp en geleverd aan een of meer gekoppelde abonnementen, afhankelijk van filterregels kunnen worden ingesteld op basis van per abonnement. De abonnementen kunnen meer filters gebruiken om te beperken van de berichten die ze willen ontvangen. Berichten worden verzonden naar een onderwerp op dezelfde manier als ze worden verzonden naar een wachtrij, maar berichten worden niet ontvangen van het onderwerp rechtstreeks. In plaats daarvan worden ze ontvangen van abonnementen. Het onderwerpabonnement van een lijkt op een virtuele wachtrij die kopieën van de berichten die worden verzonden naar het onderwerp ontvangt. Berichten worden ontvangen van een abonnement identiek aan de manier waarop die ze worden ontvangen van een wachtrij.

Ter vergelijking, de functionaliteit bericht verzenden van een wachtrij rechtstreeks aan een onderwerp en de functionaliteit hiervan bericht ontvangen die zijn toegewezen aan een abonnement. Onder andere dit betekent dat abonnementen dezelfde patronen die eerder in deze sectie met betrekking tot wachtrijen beschreven ondersteunt: concurrerend consumenten, tijdelijke ontkoppeling, load leveling en taakverdeling.

Maken van een onderwerp is vergelijkbaar met het maken van een wachtrij, zoals wordt weergegeven in het voorbeeld in de vorige sectie. Maken van de service-URI en gebruik vervolgens de [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) klasse voor het maken van de naamruimte-client. U kunt maken van een onderwerp via de [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) methode. Bijvoorbeeld:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Vervolgens voegt u abonnementen naar wens:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Vervolgens kunt u een client onderwerp maken. Bijvoorbeeld:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

De afzender gebruikt, kunt u verzenden en ontvangen van berichten naar en van het onderwerp, zoals wordt weergegeven in de vorige sectie. Bijvoorbeeld:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Vergelijkbaar met wachtrijen, berichten worden ontvangen van een abonnement met een [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) object in plaats van een [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) object. Maak de abonnement-client, waarbij de naam van het onderwerp, de naam van het abonnement en (optioneel) de receive-modus als parameters doorgegeven. Bijvoorbeeld, met de **inventaris** abonnement:

```csharp
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regels en acties
In veel scenario's, moeten de berichten met specifieke kenmerken op verschillende manieren worden verwerkt. Hiervoor kunt u abonnementen op berichten die hebben eigenschappen gewenst en voert u bepaalde wijzigingen aanbrengen in de eigenschappen te vinden. Bij het Service Bus-abonnementen Zie alle berichten die naar het onderwerp, kunt u alleen een subset van die berichten kopiëren naar de abonnementenwachtrij voor virtuele. Dit wordt bereikt met behulp van abonnementsfilters. Dergelijke wijzigingen worden genoemd *filteren acties*. Wanneer een abonnement is gemaakt, kunt u een filterexpressie die wordt toegepast op de eigenschappen van het bericht, de Systeemeigenschappen opgeven (bijvoorbeeld **Label**) en aangepaste toepassingseigenschappen (bijvoorbeeld  **StoreName**.) De filterexpressie SQL is optioneel in dit geval; zonder een filterexpressie SQL wordt geen filteractie gedefinieerd voor een abonnement worden uitgevoerd op alle berichten voor dat abonnement.

Het vorige voorbeeld, om berichten te filteren die afkomstig zijn alleen van **Store1**, maakt u het abonnement Dashboard als volgt:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Met dit abonnementsfilter aanwezig is, alleen berichten met de `StoreName` eigenschap ingesteld op `Store1` worden gekopieerd naar de virtuele wachtrij voor de `Dashboard` abonnement.

Zie voor meer informatie over mogelijke filterwaarden, de documentatie voor de [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) en [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) klassen. Zie ook de [Brokered Messaging: geavanceerde Filters](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) en [onderwerp Filters](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters) voorbeelden.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over en voorbeelden van het gebruik van Service Bus-berichtenservice geavanceerde.

* [Overzicht van Service Bus-berichten](service-bus-messaging-overview.md)
* [Service Bus Brokered Messaging .NET-zelfstudie](service-bus-brokered-tutorial-dotnet.md)
* [Service Bus brokered messaging REST-zelfstudie](service-bus-brokered-tutorial-rest.md)
* [Brokered Messaging: Geavanceerde Filters voorbeeld](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

