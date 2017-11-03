---
title: Azure Service Bus-berichtenservice uitzonderingen | Microsoft Docs
description: Lijst met Service Bus messaging uitzonderingen en voorgestelde acties.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: sethm
ms.openlocfilehash: f927aa7a33a650354abd090b6280795875ab693f
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="service-bus-messaging-exceptions"></a>Service Bus-berichtuitzonderingen
Dit artikel worden enkele uitzonderingen die worden gegenereerd door de Microsoft Azure Service Bus-berichtenservice-API's. Deze verwijzing kan worden gewijzigd, Controleer dus regelmatig op updates.

## <a name="exception-categories"></a>Uitzondering categorieën
De berichtenservice-API's uitzonderingen die in de volgende categorieën, samen met de bijbehorende actie die u ondernemen kunt om te proberen op te lossen ze kunnen ook worden gegenereerd. Houd er rekening mee dat de betekenis en de oorzaken van een uitzondering afhankelijk van het type Berichtentiteit (wachtrijen/onderwerpen of Event Hubs variëren kunnen):

1. Gebruiker fout codering ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Algemene actie: probeert op te lossen de code voordat u doorgaat.
2. Setup/configuratiefout ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Algemene actie: Controleer de configuratie en wijzig indien nodig.
3. Tijdelijke uitzonderingen ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Algemene actie: probeer het opnieuw of gebruikers een melding ontvangen.
4. Andere uitzonderingen ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Algemene actie: specifiek zijn voor het uitzonderingstype; Raadpleeg de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzondering typen
De volgende tabel bevat messaging uitzondering typen, en hun oorzaken en opmerkingen bij de voorgestelde actie die u kunt nemen.

| **Uitzonderingstype** | **Oorzaak-beschrijving/voorbeelden** | **Voorgestelde actie** | **Houd er rekening mee op automatische/direct opnieuw proberen** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server heeft niet gereageerd op de aangevraagde bewerking binnen de opgegeven tijd die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). De server kan de aangevraagde bewerking hebt voltooid. Dit kan gebeuren als gevolg van netwerk- of andere vertragingen infrastructuur. |Controleer de systeemstatus voor consistentie en probeer indien nodig. Zie [time-out-uitzonderingen](#timeoutexception). |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruikersbewerking is niet toegestaan in de server of de service. Zie het uitzonderingsbericht voor meer informatie. Bijvoorbeeld: [Complete()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) deze uitzondering wordt gegenereerd als het bericht is ontvangen [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modus. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking is ongeldig. |Probeer het opnieuw helpt niet. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Een poging is gedaan om aan te roepen, een bewerking op een object dat al is gesloten, afgebroken of verwijderd. In zeldzame gevallen, is de ambient transactie al verwijderd. |Controleer de code en zorg ervoor dat deze bewerkingen worden uitgevoerd op een verwijderd object niet gestart. |Probeer het opnieuw helpt niet. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |De [TokenProvider](/dotnet/api/microsoft.azure.servicebus.tokenprovider) object kan niet een token verkrijgen, het token is ongeldig of het token bevat niet de claims die zijn vereist voor de bewerking niet uitvoeren. |Zorg ervoor dat de tokenprovider wordt gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |Probeer het opnieuw kan helpen in sommige gevallen; Pogingslogica aan code toevoegen. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer argumenten doorgegeven aan de methode zijn ongeldig.<br /> De URI die is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) pad segment(en) bevat.<br /> Het URI-schema is doorgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [maken](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) is ongeldig. <br />De eigenschapswaarde is groter dan 32KB. |Controleer de aanroepende code en controleer of dat de argumenten juist zijn. |Probeer het opnieuw helpt niet. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |Entiteit gekoppeld aan de bewerking is niet aanwezig of is verwijderd. |Controleer of dat de entiteit bestaat. |Probeer het opnieuw helpt niet. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Poging een bericht ontvangen met een bepaalde volgnummer. Dit bericht is niet gevonden. |Zorg ervoor dat het bericht niet al is ontvangen. Controleer de wachtrij voor onbestelbare berichten om te zien of het bericht deadlettered is. |Probeer het opnieuw helpt niet. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Client kan geen verbinding maken met Service Bus. |Zorg ervoor dat de opgegeven hostnaam juist is en de host bereikbaar is. |Probeer het opnieuw kan u helpen als er onregelmatige verbindingsproblemen. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |Service is niet kunnen verwerken van de aanvraag op dit moment. |Client kan wachten op een bepaalde tijd en probeer het opnieuw. |Client kan opnieuw na een bepaalde interval. Als een nieuwe poging in een andere uitzondering resulteert, controleert u gedrag van deze uitzondering voor het opnieuw. |
| [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception) |Vergrendelingstoken die is gekoppeld aan het bericht is verlopen of de lock-token is niet gevonden. |Verwijderen van het bericht. |Probeer het opnieuw helpt niet. |
| [SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception) |Vergrendeling die zijn gekoppeld aan deze sessie wordt verbroken. |Afbreken de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) object. |Probeer het opnieuw helpt niet. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Algemene messaging uitzondering die kan worden gegenereerd in de volgende gevallen:<br /> Een poging is gedaan maken een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) met een naam of het pad dat tot een andere entiteittype (bijvoorbeeld een onderwerp behoort).<br />  Wordt geprobeerd een bericht te verzenden groter is dan 256KB. De server of -service is een fout opgetreden tijdens het verwerken van de aanvraag. Zie het uitzonderingsbericht voor meer informatie. Dit is meestal een tijdelijke uitzondering. |Controleer de code en ervoor te zorgen dat alleen serialiseerbaar objecten worden gebruikt voor de hoofdtekst van bericht (of gebruik een aangepaste serialisatiefunctie). Raadpleeg de documentatie voor de ondersteunde typen van de eigenschappen en alleen de gebruikstypen die worden ondersteund. Controleer de [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) eigenschap. Als het **true**, kunt u de bewerking opnieuw. |Gedrag voor opnieuw proberen is niet gedefinieerd en niet kan helpen. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Poging tot maken van een entiteit met een naam die al door een andere entiteit in de naamruimte van die service gebruikt wordt. |Verwijder de bestaande entiteit of kies een andere naam voor de entiteit moet worden gemaakt. |Probeer het opnieuw helpt niet. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |De Berichtentiteit heeft de maximaal toegestane grootte bereikt, of het maximum aantal verbindingen met een naamruimte is overschreden. |Maak schijfruimte vrij in de entiteit door het ontvangen van berichten van de entiteit of de onderliggende wachtrijen. Zie [QuotaExceededException](#quotaexceededexception). |Probeer het opnieuw kan u helpen als berichten in de tussentijd zijn verwijderd. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus retourneert deze uitzondering als u probeert te maken van een ongeldige regel in te grijpen. Als er een fout optreedt tijdens het verwerken van de regelactie voor het desbetreffende bericht, Service Bus deze uitzondering koppelt aan een bericht deadlettered. |Controleer de regelactie op juistheid. |Probeer het opnieuw helpt niet. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus retourneert deze uitzondering als u probeert te maken van een ongeldig filter. Als een fout opgetreden tijdens het verwerken van het filter voor het desbetreffende bericht, Service Bus deze uitzondering koppelt aan een bericht deadlettered. |Controleer het filter op juistheid. |Probeer het opnieuw helpt niet. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Poging tot het accepteren van een sessie met een bepaalde sessie-ID, maar de sessie is momenteel vergrendeld door een andere client. |Zorg ervoor dat de sessie is ontgrendeld door andere clients. |Probeer het opnieuw kan u helpen als de sessie in de tussentijd is vrijgegeven. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Te veel bewerkingen maken deel uit van de transactie. |Verminder het aantal bewerkingen die deel uitmaken van deze transactie. |Probeer het opnieuw helpt niet. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Aanvraag voor een Runtimebewerking op een uitgeschakelde entiteit. |Activeer de entiteit. |Probeer het opnieuw kan u helpen als de entiteit in de tussentijd is geactiveerd. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus retourneert deze uitzondering als u een bericht verzendt naar een onderwerp dat is vooraf filtering ingeschakeld en geen van de filters overeenkomt met. |Zorg ervoor dat minstens één filter overeenkomt met. |Probeer het opnieuw helpt niet. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een bericht nettolading overschrijdt de limiet van 256 KB. Houd er rekening mee dat de limiet van 256 KB is de totale berichtgrootte, waaronder Systeemeigenschappen en een .NET-overhead. |Reduceer de grootte van de nettolading van het bericht en probeer het opnieuw. |Probeer het opnieuw helpt niet. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |De ambient transactie (*Transaction.Current*) is ongeldig. Mogelijk is het voltooid of afgebroken. De binnenste uitzondering biedt mogelijk meer informatie. | |Probeer het opnieuw helpt niet. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Een bewerking wordt uitgevoerd voor een transactie is onzeker, of wordt geprobeerd de transactie doorvoeren en de transactie is onzeker. |Uw toepassing moet deze uitzondering (als een speciaal geval), verwerken, omdat de transactie al doorgevoerd is mogelijk. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) geeft aan dat een quotum voor een specifieke entiteit is overschreden.

### <a name="queues-and-topics"></a>Wachtrijen en onderwerpen
Voor wachtrijen en onderwerpen is dit vaak de grootte van de wachtrij. De eigenschap van het bericht bevat verdere details, zoals in het volgende voorbeeld:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

Het bericht geeft aan dat het onderwerp de maximale grootte in deze case 1 GB (de limiet voor de standaardwaarde) overschreden. 

### <a name="namespaces"></a>Naamruimten

Voor naamruimten, [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) kan erop wijzen dat een toepassing het maximum aantal verbindingen met een naamruimte heeft overschreden. Bijvoorbeeld:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Algemene oorzaken
Er zijn twee algemene oorzaken voor deze fout: de wachtrij voor onbestelbare berichten en niet-functionerende bericht ontvangers.

1. **[Wachtrij voor onbestelbare berichten](service-bus-dead-letter-queues.md)**  een lezer kan niet worden voltooid berichten en berichten naar de wachtrij/onderwerp worden geretourneerd wanneer de vergrendeling is verlopen. Dit kan gebeuren als er een uitzondering dat aanroepen verhindert optreedt bij de lezer [BrokeredMessage.Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete). Nadat een bericht is gelezen 10 keer, wordt deze verplaatst naar de wachtrij voor onbestelbare berichten standaard. Dit gedrag wordt bepaald door de [QueueDescription.MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) eigenschap en heeft een standaardwaarde van 10. Als berichten in de wachtrij voor onbestelbare berichten opstapelen, innemen deze ruimte.
   
    Het probleem op te lossen lezen en voltooi de berichten uit de wachtrij voor onbestelbare berichten, zoals u zou vanuit een andere wachtrij doen. U kunt de [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) methode om te formatteren pad van de wachtrij voor onbestelbare berichten.
2. **Ontvanger gestopt** een ontvanger ontvangen van berichten van een wachtrij of het abonnement is gestopt. De manier om dit te identificeren is om te kijken naar de [QueueDescription.MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) eigenschap die u de volledige uitsplitsing van de berichten kunt. Als de [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) eigenschap hoge of groeiende, wordt de berichten niet snel ze worden geschreven wordt gelezen.

### <a name="event-hubs"></a>Event Hubs
Event Hubs is een limiet van 20 consumergroepen per Event Hub. Wanneer u probeert te maken, krijgt u een [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een gebruiker gestart bewerking langer dan de time-out voor de bewerking duurt. 

Controleer de waarde van de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) eigenschap, kunt u deze limiet door kan ook leiden tot een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Wachtrijen en onderwerpen
Wachtrijen en onderwerpen, de time-out is opgegeven in de [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) eigenschap als onderdeel van de verbindingsreeks of via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Het foutbericht zelf kan afwijken, maar bevat altijd de time-outwaarde die is opgegeven voor de huidige bewerking. 



## <a name="next-steps"></a>Volgende stappen

Zie voor de volledige Service Bus-.NET API-verwijzing de [Azure .NET API-verwijzingen](/dotnet/api/overview/azure/service-bus).

Voor meer informatie over [Service Bus](https://azure.microsoft.com/services/service-bus/), Zie de volgende artikelen:

* [Overzicht van Service Bus-berichten](service-bus-messaging-overview.md)
* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-architectuur](service-bus-architecture.md)

